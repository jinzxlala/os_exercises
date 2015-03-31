#lec9 虚存置换算法spoc练习

## 个人思考题
1. 置换算法的功能？

2. 全局和局部置换算法的不同？

3. 最优算法、先进先出算法和LRU算法的思路？

4. 时钟置换算法的思路？

5. LFU算法的思路？

6. 什么是Belady现象？

7. 几种局部置换算法的相关性：什么地方是相似的？什么地方是不同的？为什么有这种相似或不同？

8. 什么是工作集？

9. 什么是常驻集？

10. 工作集算法的思路？

11. 缺页率算法的思路？

12. 什么是虚拟内存管理的抖动现象？

13. 操作系统负载控制的最佳状态是什么状态？

## 小组思考题目

----
(1)（spoc）请证明为何LRU算法不会出现belady现象
设Sn为物理页面数为n的LRU算法维护栈，S(n+k)为物理页面数为n+k时的LRU算法维护栈。
题目为，在任意时刻，Sn∈S(n+k)且任意x∈Sn,存在x1∈S(n+k),满足loc(x) <= loc(x1),即物理页面数量增加不会导致缺页率降低。

>
```
采用数学归纳法。
<1>Sn和S(n+k)都为空，满足题目条件
<2>设时间T=t时满足条件，那么T = t + 1时，对页面请求xt，有以下几种情况：
   1:xt∈Sn,且xt∈S(n+k),那么这一步后，xt位于Sn和S(n+k)的顶部，满足locSn(xt) < locSn+k(xt);
   2:xt∉Sn,且xt∈S(n+k),此时在Sn栈顶压入xt且弹出了原先的栈顶元素，xt在S(n+k)中被置于栈顶，此时仍有locSn(xt) < locSn+k(xt)
   3:xt∉Sn,且xt∉S(n+k),此时都在栈尾加入xt，Sn中为n，S(n+k)中为n+k。对于弹出的元素，若两者相同，则依旧满足条件，若不相同，则在S(n+k)弹出的元素在Sn中已经找不到。依然满足Sn包含于S(n+k);
即T=t+1时，条件成立。
综上所述，原命题得证，LRU算法不会出现belady现象。
```

(2)（spoc）根据你的`学号 mod 4`的结果值，确定选择四种替换算法（0：LRU置换算法，1:改进的clock 页置换算法，2：工作集页置换算法，3：缺页率置换算法）中的一种来设计一个应用程序（可基于python, ruby, C, C++，LISP等）模拟实现，并给出测试。请参考如python代码或独自实现。
 - [页置换算法实现的参考实例](https://github.com/chyyuu/ucore_lab/blob/master/related_info/lab3/page-replacement-policy.py)
>
```
student ID = 2012011365, id % 4 = 1

#include <iostream>
#include <vector>
using namespace std;
struct Page{
    int visit;//bit visit;
    int change;//bit change;
    int addr;//address
    Page(int add):visit(0),change(0),addr(add){};
};
vector<Page *> pageList;
vector<Page *> testList;
int pointer;
void init(){
    for(int i = 0; i < 5; i++){
        Page *temp = new Page(i * 10 + i);
        pageList.push_back(temp);
    }
    testList.push_back(new Page(11));
    testList.push_back(new Page(12));
    testList.push_back(new Page(33));
    testList.push_back(new Page(32));
    testList.push_back(new Page(54));
    testList.push_back(new Page(54));
}
void printList(){
    cout << "===========List===============" << endl;
    for(int i = 0;i < pageList.size(); i++){
        cout << "<" << i << "> :" << "[ " << pageList[i]->addr << " | " <<pageList[i]->visit << " | " << pageList[i]->change <<" ]" << endl;
    }
    cout << "===========List End===========" << endl;
}
int replace_by_clock(Page *page, int op){//op:0->read, 1->write
    int place = pointer;
    while(1){
        if(!pageList[pointer]->visit && !pageList[pointer]->change){
            pageList[pointer] = page;
            pageList[pointer]->visit = 1;
            pageList[pointer]->change = op;
            return pointer;
        }
        else if(pageList[pointer]->visit && pageList[pointer]->change){
            pageList[pointer]->visit = 0;
        }
        else{
            pageList[pointer]->visit = pageList[pointer]->change = 0;
        }
        pointer = (pointer + 1) % pageList.size();
    }
    return place;
}
int findPage(Page *page, int op){//op:0->read,1->write
    int lenth = (int)pageList.size();
    int flag = 0;
    for(int i = 0; i < lenth; i++){
        if(pageList[i]->addr == page->addr){
            flag = 1;
            pageList[i]->change = op;
            pageList[i]->visit = 1;
            return i;
        }
    }
    pointer = (replace_by_clock(page, op) + 1) % lenth;
    return pointer;
}
void test(){
    for(int i = 0; i < testList.size(); i++){
        cout << "find " << testList[i]->addr << endl;
        findPage(testList[i], i % 2);
        printList();
        cout << "pointer = " << pointer << endl;
    }
}
int main(int argc, const char * argv[]) {
    init();
    test();
    return 0;
}

``` 
## 扩展思考题
（1）了解LIRS页置换算法的设计思路，尝试用高级语言实现其基本思路。此算法是江松博士（导师：张晓东博士）设计完成的，非常不错！

参考信息：

 - [LIRS conf paper](http://www.ece.eng.wayne.edu/~sjiang/pubs/papers/jiang02_LIRS.pdf)
 - [LIRS journal paper](http://www.ece.eng.wayne.edu/~sjiang/pubs/papers/jiang05_LIRS.pdf)
 - [LIRS-replacement ppt1](http://dragonstar.ict.ac.cn/course_09/XD_Zhang/(6)-LIRS-replacement.pdf)
 - [LIRS-replacement ppt2](http://www.ece.eng.wayne.edu/~sjiang/Projects/LIRS/sig02.ppt)
