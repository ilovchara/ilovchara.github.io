---
title: Three 基本数据结构
date: 2023-04-27 14:48:19
tags: 算法
description: 基本数据结构
typora-root-url: Three
categories: 算法
---

## 第三章 - 数据结构

![image-20230420172340030](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230420172340030.png)

```c++
1.基本数据结构
    链表 - 栈和堆 - 队列 - 哈希表的使用 - 并查集 - 字典树 - kmp - 
2.离散数学基本知识
3.线段树
```

### 链表

> [链表看这一篇就够了](https://blog.51cto.com/u_15018701/2616916)

```c++
 //个人学习的时候并不是很了解，就是，将一些没有联系的数据串起来。这样可以用当前的数据访问到下一个数据或者上一个数据。可以理解成为，数存储在一个数组中，然后我们再声明一个数组，存储着这些数据的位置。
 就像火车一样，每一节车厢中装载的货物，和每一节车厢的编号两者互相独立，但是又互相联系；关系就仅仅是位置上的对应关系而已。
        
    //链表是一种递归的数据结构，他或者为空（null），或者是指向一个结点（node）的引用，该结点含有一个泛型的元素和一个指向另一条链表的引用。
 链表是一种常见且基础的数据结构，是一种线性表，但是他不是按线性顺序存取数据，而是在每一个节点里存到下一个节点的地址。我们也可以这样理解，链表是通过指针串联在一起的线性结构，每一个链表结点由两部分组成，数据域及指针域，链表的最后一个结点指向null。也就是我们所说的空指针。
```

#### 单链表

![单链表图解](format.webp)

```c++
//1.单链表
 一个单向链表包含两个值: 当前节点的值和一个指向下一个节点的链接。我们通过上面说到的可视化表示方法，将单链表可视化，如上图所示。
//2.实现
 知道了单链表的特性，我们使用数组来实现一下这个单链表，下面是代码。       
//实现细究
首先，我们要实现一个链表，应该有几个准备工作;
1.声明一个数组 用于存入我们的数据
2.声明位置数组，用来存储我们的数据下标（对应的车厢）
3.声明一个变量，用于插入操作 一般用我们的idx
4.声明开头的数据下标 head 和 尾坐标 nell

//链表使用的时候要初始化 
void init()
{
   head = -1;
   idx = 0;
}
//声明我们所需要的数据类型
int e[N],  ne[N],   idx ,   head;
 存储值  存储下标 操作变量  头指针
 //e[N] 和 ne[N] 这两个看着是错开的 实际上是因为我们的头指针占了一个位置（叫做指针 是因为它很像一个标尺一样）

实现的一些功能
//将x插入头指针
void add_to_head(int x)
  {
    ne[idx] = head;
    e[idx] = x;
    head = idx++;
    idx++;  //指针移动位置 
  }
//插入k位置 把x
void add(int k,int x)
{
  //把head 换成 ne[k] 就好了
   ne[idx] = ne[k];
   e[idx] = x;
   ne[k] = idx++;
   idx++;  
}
//将下标k后面的点删除
void remove(int k)
{ 
  //ne[k]表示的是k数据的下一个数据的位置
  //那么 ne[ne[k]] 就是跳过了下一个数据 到第三个数据上了
  //也就是当前数据的下一个位置 变成了第三个数据的位置了
  ne[k] = ne[ne[k]]; //这里用递归来理解
  //理解ne是下一个位置就行 实际上是用idx来构建序列的
}
```

> [C++链表及其创建](http://c.biancheng.net/view/1570.html)

![结构](2-1Q20316324S20.gif)

![img](2-1Q203163321H2.gif)

```c++
//链表的结构
 链表中的每个结点都包含一个或多个保存数据的成员。例如，存储在结点中的数据可以是库存记录；或者它可以是由客户的姓名、地址和电话号码等组成的客户信息记录。
 非空链表的第一个结点称为链表的头。要访问链表中的结点，需要有一个指向链表头的指针。从链表头开始，可以按照存储在每个结点中的后继指针访问链表中的其余结点。最后一个结点中的后继指针被设置为 nullptr 以指示链表的结束。
//个人理解
     链表就是用线串起来的数据块，每一个节点的两个值都有其特定的作用：next指针负责穿起来节点，数据块负责存储数据。数据的起点就是链表头head，数据的终点指向null。

//c++表示
     虽然说我们可以用数组实现我们的链表，但是这样实现的链表不够简洁，我们可以使用结构体来实现（本质上是一样的）
        struct ListNode
        {
            double value;//数据
            ListNode *next;//下一个数据的位置
        };
 在以上代码中，ListNode 就是要存储在链表中的结点的类型，结构成员 value 是结点的数据部分，而另一个结构成员 next 则被声明为 ListNode 的指针，它是指向下一个结点的后继指针。
 在已经声明了一个数据类型来表示结点之后，即可定义一个初始为空的链表，方法是定义一个用作链表头的指针并将其初始化为 nullptr，示例如下
        ListNode *head = nullptr;   
```

#### 双向链表

> <https://blog.csdn.net/slandarer/article/details/91863177>
>
> <http://c.biancheng.net/view/1570.html>

![双向链表](双向链表.png)

```c++
//2.双向链表
 比起单链表，多了一个指向前的指针，双向链表有三个整数值: 数值、向后的节点链接、向前的节点链接，所以双链表既能向前查询也可以向后查询。
 还有一个常用的链表则为循环单链表，则单链表尾部的指针指向头节点。
//用数组构造双链表
 比起单链表中，多了一个指向前面的指针
//那就多声明一个前继路线
int l[N],r[N],idx,e[N];
void init()
{
  l[0] = -1;
  idx = 0;  
}

//往k的右边 插入数据
void add(int k,int x)
{
  e[idx] = x;
  r[idx] = r[k];
  l[idx] = l[k];
  l[r[k]] = idx;
  r[k] = idx;
  
}
//往k的左边插入数据
void add(int k,int x)
{
  e[idx] = k;
  l[idx] = l[k];
  r[idx] = r[k];
//理解 l[k] 是k左边的位置 那么r[l[k]] 就是 k这个位置
  r[l[k]] = idx;
  l[k] = idx;
  
}

```

```c++
//上述使用多个数组来构造我们的链表，但实际使用的时候我们并不会这样子做（有时候会），我们可以学习一种新的构造方法（其实是一样的），利用结构体和指针关系来构造我们的链表

//用这样的思路来构造：就是说你现在有一个小纸条，上面写着一个抽屉的地址，那个抽屉里有一些你需要的东西，和一个新的写着地址的小纸条，这个小纸条又指向了一个新的抽屉，大体可以这么理解

//第一部分—构建抽屉 
typedef struct listpoint //这里是抽屉的结构体
{
    int data;//我们存储的数据
    listpoint *next;//指向下一个抽屉的指针
}listpoint;

//我们在抽屉里不仅仅可以放一个数，我们可以往里面放一个收纳盒，例如，在下面的结构体中包含了另一个结构体
typedef struct data
{
    int number;
    string name;
    string sex;
}data;

typedef struct listpoint
{
    data *information;//收纳盒
    listpoint *next;//下一个箱子（的地址）
    listpoint *last;//上一个箱子（的地址）
}listpoint;

//第二部分—创建一个链表
/*链表每一个节点都是指向  listpoint结构的指针，所以返回值是listpoint *,n是指创建的链表的节点数目*/
listpoint *create_normal_list(int n)     
{
    listpoint *head,*normal,*end;/*创建头节点，其他节点，和尾节点*/
    head=(listpoint*)malloc(sizeof(listpoint));
    head->information=(data*)malloc(sizeof(data));
    /*分配内存*/
    end=head;/*最开始最后一个节点就是头节点，注意因为通过指针可以直接对地址上的东西进行操作，此时end和head指向同一个地址，对end所指向地址进行操作，等同于对head地址所做的操作*/
    for(int i=0;i<n;i++)
    {
        normal=(listpoint*)malloc(sizeof(listpoint));
        normal->information=(data*)malloc(sizeof(data));
        /*给新节点分配内存*/
        cout<<"input the number :";
        cin>>normal->information->number;
        cout<<"input the name   :";
        cin>>normal->information->name;
        cout<<"input the sex    :";
        cin>>normal->information->sex;
        cout<<"----------------------------------"<<endl;
       /* 往新节点存入数据，注意我们只给后面的节点存入数据，head不存数据*/
        end->next=normal;/*往end后增添新节点*/
        normal->last=end;/*新节点的上一个节点就是end*/
        end=normal;/*最后一个节点变成新节点*/
    }
    end->next=NULL;/*链表的最后指向一个新地址*/
    head->last=NULL;/*链表最开始的节点没有上一个节点*/
    return head;
}
```

#### 循环链表

##### 循环单链表*

> [数据结构——从单链表到单向循环链表 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/107808443)

 就是在单链表的基础之上，将最后本指向null的改为指向链表头。

![循环单链表](循环单项链表.png)

 循环链表主要用于操作系统中的任务维护。有许多例子，循环链表用于计算机科学，包括浏览器，记录用户过去访问过的页面记录也可以以循环链表的形式保存，并且可以在点击前一个按钮时再次访问。

 对单向链表中任一个节点的访问都需要从头结点开始；而对单向循环链表从任意节点出发都能遍历整个列表**，**极大的增强了其灵活性。

```c++
//c++实现
//1.节点类，
class Node{
public:
 int data;
 Node *pointer=NULL;
};
//2.链表类
class SingleLinkedList {
public:
 SingleLinkedList();
 bool isEmpty();
 void add(int data);
 void traversal();

private:
     Node *head;
};
//3.构造函数
SingleLinkedList::SingleLinkedList() {
 head = new Node;
 head->data = 0;
 head->pointer = head;
}
//4.判断是否为空：isEmpty()
bool SingleLinkedList::isEmpty() {
 // 头结点不指向任何结点，为空
 if (head->pointer == head) {
  return true;
 }
 else {
  return false;
 }
}
//5.头插法：add()
void SingleLinkedList::add(int data) {
 // 当原列表仅有头结点时，直接插入新节点即可
 if (isEmpty()) {
  head->pointer = new Node;
  head->pointer->pointer = head;
  head->pointer->data = data;
 }
 // 列表非空时
 else {
  // 临时存储头结点的直接后继
  Node *temp = head->pointer;
  head->pointer = new Node;
  head->pointer->data = data;
  head->pointer->pointer = temp;
 }
}
//6.遍历：traversal()
void SingleLinkedList::traversal() {
 // 链表为空，结束函数
 if(isEmpty()) {
  return;
 }
 else {
  Node *current;
  // 指向头结点的直接后继
  current = head->pointer;
  int counter = 1;
  // 遍历链表，输出每个节点的值
  while (current != head)
  {
   printf("Element in %d is %d \n", counter, current->data);
   counter++;
   current = current->pointer;
  }
 }
}
```

##### 循环双链表*

> [循环双链表](https://blog.csdn.net/sum_TW/article/details/61624039?ydreferer=aHR0cHM6Ly93d3cuZ29vZ2xlLmNvbS8%3D)

 和循环单链表一样，只是多了双链表的属性。

![img](Center.png)

双向循环链表初始化

```c++
//创建链表
void creatList(Node** h)
{
    Node* pn=NULL;//存储新的节点
    Node* p=NULL;//头节点的替身
    int d;
    printf("请输入数据\n");
    scanf("%d",&d);
    pn=creteNode(d);
    pn->next=NULL;//next指向空
    pn->pre=NULL;//pre指向空
    sum=1;
    *h=pn;
    p=*h;
    while(1)
    {
        printf("请输入数据\n");
        scanf("%d",&d);
        if(d==0)
        {
            //创建到最后一个节点时，最后一个节点next为头节点
            p->next=*h;
            (*h)->pre=p;//头节点的pre为最后一个节点
            break;
        }
        pn=creteNode(d);
        p->next=pn;
        pn->pre=p;
        p=p->next;
        sum++;//元素个数++
    }
}
```

插入操作

![双向循环链表插入](双向循环链表插入.png)

```c++
//头插法
int addFont(int d,Node** h)//修改头节点  传入二级指针
{
    int i,n=sum;
    Node* pn=NULL;
    pn=creteNode(d);
    Node* p=*h;
    for(i=0;i<n-1;i++)
    {
        p=p->next;
    }
    //p为最后一个节点
    pn->next=*h;//新节点成为头节点
    p->next=pn;//最后一个节点p的下一个节点为新节点pn
    pn->pre=p;//新的头节点的pre为p
 
    (*h)->pre=pn;//原头节点的前一个节点为新节点
 
    *h=pn;//新节点为头节点
 
    sum++;
}
 
//插入
int insertNode(int n,int d,Node** h)//在n位置插入d
{
    if((n<1)||(*h==NULL)||(n>sum))
    {
        printf("插入位置不合法||链表为空!\n");
        return 0;
    }
     Node* pn=creteNode(d);//创建新的节点
    //插入位置为1，即插入头节点的位置
    if(n==1)
    {
       addFont(d,h);//调用头插法
       return 0;
    }
    else if(n==sum)
    {
        addBack(d,*h);
        return 0;
    }
    else
    {
       Node* pf=findNode(*h,n-1); //找到要删除的节点的前一个节点
       pn->next=pf->next;//前一个节点的next等于新的节点的next
       pf->next->pre=pn;//pf的next节点的pre应该为pn
       pf->next=pn;//前一个节点的next等于新的节点
       pn->pre=pf;//新节点的前一个节点为找到的前一个节点pf
       sum++;
       return 1;
    }
}
```

删除操作

![img](双向循环链表删除.png)

```c++
//删除头节点
int DelectFont(Node** h)
{
 //DelectFont(h);
    int i, n1=sum;
    Node* p=*h;
    Node* pd=NULL;;
    for(i=0;i<n1-1;i++)
    {
        p=p->next;
    }
    
    //p为最后一个节点
    pd=*h;
    *h=pd->next;//头节点的下一个节点成头节点
    
    p->next=*h;//最后一个节点的next为头节点
    
    (*h)->pre=p;//pd节点成为了头节点，它的前驱为p
    sum--;//元素个数-1
  
    return 0;
}
 
//删除第n个位置的元素
int deleteNode(int n,Node** h)
{
    //int i;//循环变量
    //判断头节点是否为空，位置是不是合法
    if((*h==NULL)||(n<1)||(n>sum))
    {
        printf("删除的链表为空||删除的位置不合法！so 插入失败\n");
        return 0;
    }
    Node* pd=NULL;
    //删除头节点
    if(n==1)
    {
       DelectFont(h);
       return 0;
    }
 
    //删除
    //找到要删除的节点的前一个节点
    Node* pf=findNode(*h,n-1);
    pd=pf->next;//将要删除的节点的给pd
    pf->next=pd->next;//将删除元素的前一个的next指向删除元素的后一个元素
    pd->pre=NULL;//pd的pre指向空
   
    pd->next->pre=pf;//将删除元素的后一个元素的前驱给pf
    pd->next=NULL;//pdnext指向空
    sum--;
    return 1;
}

```

总

```c++
#include <stdio.h>
#include <stdlib.h>
 
struct node{
    int data;
    struct node* next;
    struct node* pre;
};
typedef struct node Node;
#define SIZE sizeof(Node)
int sum;//节点个数
 
Node* creteNode(int d);//创建节点
void creatList(Node** h)；//创建链表
Node* findNode(Node* h,int n)；//查找某个节点的位置
int addBack(int d,Node* h)；//末尾增加一个新的节点
int addFont(int d,Node** h)；//修改头节点  传入二级指针
int insertNode(int n,int d,Node** h)；//在n位置插入d
int DelectFont(Node** h);//删除头节点
int deleteNode(int n,Node** h);//删除第n个位置的元素
void print(Node* h);//打印链表
 
//创建节点
Node* creteNode(int d)
{
    Node* pn=(Node*)malloc(SIZE);
    pn->data=d;
    pn->next=NULL;
    pn->pre=NULL;
    return pn;
}
 
//创建链表
void creatList(Node** h)
{
    Node* pn=NULL;//存储新的节点
    Node* p=NULL;//头节点的替身
    int d;
    printf("请输入数据\n");
    scanf("%d",&d);
    pn=creteNode(d);
    pn->next=NULL;//next指向空
    pn->pre=NULL;//pre指向空
    sum=1;
    *h=pn;
    p=*h;
    while(1)
    {
        printf("请输入数据\n");
        scanf("%d",&d);
        if(d==0)
        {
            //创建到最后一个节点时，最后一个节点next为头节点
            p->next=*h;
            (*h)->pre=p;//头节点的pre为最后一个节点
            break;
        }
        pn=creteNode(d);
        p->next=pn;
        pn->pre=p;
        p=p->next;
        sum++;//元素个数++
    }
}
 
//查找某个节点的位置
Node* findNode(Node* h,int n)
{
    int i;
    if((h==NULL)||(n<0)||(n>sum))
    {
            printf("查找位置不合法||链表为空！\n");
            return NULL;
    }
    if(n==1)
    {
        return h;
    }
    for(i=1;i<n;i++)
    {
        h=h->next;
    }
    return h;
} 
 
//末尾增加一个新的节点
int addBack(int d,Node* h)
{
    int i,n=sum;
    Node *pn=NULL;
    pn=creteNode(d);
    Node* p=h;
    for(i=0;i<n-1;i++)
    {
        p=p->next;
    }
    //此时的p指向了最后一个元素
    p->next=pn;//最后一个节点p的next为新节点
    pn->pre=p;//新节点的前一个节点为p
    pn->next=h;//pn的最后一个节点为头节点
    h->pre=pn;//头节点的pre为新节点
    sum++;
}
 
//头插法
int addFont(int d,Node** h)//修改头节点  传入二级指针
{
    int i,n=sum;
    Node* pn=NULL;
    pn=creteNode(d);
    Node* p=*h;
    for(i=0;i<n-1;i++)
    {
        p=p->next;
    }
    //p为最后一个节点
    pn->next=*h;//新节点成为头节点
    p->next=pn;//最后一个节点p的下一个节点为新节点pn
    pn->pre=p;//新的头节点的pre为p
 
    (*h)->pre=pn;//原头节点的前一个节点为新节点
 
    *h=pn;//新节点为头节点
 
    sum++;
}
 
//插入
int insertNode(int n,int d,Node** h)//在n位置插入d
{
    if((n<1)||(*h==NULL)||(n>sum))
    {
        printf("插入位置不合法||链表为空!\n");
        return 0;
    }
     Node* pn=creteNode(d);//创建新的节点
    //插入位置为1，即插入头节点的位置
    if(n==1)
    {
       addFont(d,h);//调用头插法
       return 0;
    }
    else if(n==sum)
    {
        addBack(d,*h);
        return 0;
    }
    else
    {
       Node* pf=findNode(*h,n-1); //找到要删除的节点的前一个节点
       pn->next=pf->next;//前一个节点的next等于新的节点的next
       pf->next->pre=pn;//pf的next节点的pre应该为pn
       pf->next=pn;//前一个节点的next等于新的节点
       pn->pre=pf;//新节点的前一个节点为找到的前一个节点pf
       sum++;
       return 1;
    }
}
 
//删除头节点
int DelectFont(Node** h)
{
 //DelectFont(h);
    int i, n1=sum;
    Node* p=*h;
    Node* pd=NULL;;
    for(i=0;i<n1-1;i++)
    {
        p=p->next;
    }
    
    //p为最后一个节点
    pd=*h;
    *h=pd->next;//头节点的下一个节点成头节点
    
    p->next=*h;//最后一个节点的next为头节点
    
    (*h)->pre=p;//pd节点成为了头节点，它的前驱为p
    sum--;//元素个数-1
  
    return 0;
}
 
//删除第n个位置的元素
int deleteNode(int n,Node** h)
{
    //int i;//循环变量
    //判断头节点是否为空，位置是不是合法
    if((*h==NULL)||(n<1)||(n>sum))
    {
        printf("删除的链表为空||删除的位置不合法！so 插入失败\n");
        return 0;
    }
    Node* pd=NULL;
    //删除头节点
    if(n==1)
    {
       DelectFont(h);
       return 0;
    }
 
    //删除
    //找到要删除的节点的前一个节点
    Node* pf=findNode(*h,n-1);
    pd=pf->next;//将要删除的节点的给pd
    pf->next=pd->next;//将删除元素的前一个的next指向删除元素的后一个元素
    pd->pre=NULL;//pd的pre指向空
   
    pd->next->pre=pf;//将删除元素的后一个元素的前驱给pf
    pd->next=NULL;//pdnext指向空
    sum--;
    return 1;
}
 
//打印链表
void print(Node* h)
{
    int i,n=sum;
    printf("list:\n");
    Node* p=h;
    printf("正序遍历：\n");
    for(i=1;i<=n;i++)
    {
        printf("%d ",p->data);
        p=p->next;
    }
    //上面遍历到第一个节点
    printf("\n");
    printf("反序遍历：\n");
    for(i=1;i<=n;i++)
    {
        printf("%d ",p->pre->data);
        p=p->pre;
    }
    printf("\n");
}
 
int main()
{
    Node* head=NULL;
    creatList(&head);
    deleteNode(2,&head);
    print(head);
    return 0;
}
```

##### 随机链表*（暂时不学）

![list](list.png)

#### 链表实现的功能*

> [数据结构中堆、栈和队列的理解](https://www.jianshu.com/p/5f148c3e4f7d)

### 栈

**栈的定义：**

栈是限定仅在表尾进行插入和删除操作的线性表。我们把允许插入和删除的一端称为栈顶，另一端称为栈底，不含任何数据元素的栈称为空栈。栈的特殊之处在于它限制了这个线性表的插入和删除位置，它始终只在栈顶进行。

而且栈是一种具有后进先出的数据结构，又称为后进先出的线性表，简称 LIFO（Last In First Out）结构。也就是说后存放的先取，先存放的后取，这就类似于我们要在取放在箱子底部的东西（放进去比较早的物体），我们首先要移开压在它上面的物体（放进去比较晚的物体）。

堆栈中定义了一些操作。两个最重要的是PUSH和POP。PUSH操作在堆栈的顶部加入一个元素。POP操作相反，在堆栈顶部移去一个元素，并将堆栈的大小减一。

![栈](栈.png)

用我们的数组实现的栈，最关键的点在于：我们是否可以直接不管我们插入栈的数据。 栈的原理就是 先进后出 这个我们知道，那么我们只要声明一个数组就可以实现这个功能

```c++
//用数组实现栈功能
//栈 和 操作栈的变量
int stk[N],tt;//变量用来控制我们的栈的容量

//插入栈的数据
stk[++tt] = x;
//pop弹出 这样的数据会直接消失 记得保存stk[kk] 栈顶数据 
tt--; 
//判断栈顶是否为空 (这里是伪代码)
if(tt>0) no empty;
else empty;
//栈顶
stk[kk]
  
//总结 
    栈的作用就是实现两个操作 1.压栈 2.弹栈 - 压栈的操作在用数组实现的时候，我们可以理解 竖向数组的增高（但实际上在现实中更像手枪压弹夹一样）弹栈 - 就是去顶（更像手枪把子弹打出去）
 通常，声明一个栈的时候，我们会用一个变量来维护我们这个栈。 这个变量起到实现上述两个操作的作用 - 例如： h[N]是我们的一个栈 top是我们维护栈的变量
 那么： push操作就是 h[++top] = a(你塞入的子弹)；pop操作就是  top--;
query(栈头) h[top] //重点是插入的时候是先 ++ 在赋值位置的 这样可以保证沉底
//stl中的栈
 <stack> 是栈的头文件名称，用的时候声明一下就好了。
```

栈的应用——递归

在高级语言中，调用自己和其它函数没有本质的不同。我们把一个直接用自己或通过一系列的调用语句间接地调用自己的函数，称作递归函数。每个递归函数必须至少有一个条件，满足时递归不再执行，即不再引用自身而是返回值退出。

递归和迭代的区别是：迭代使用的是循环结构，递归使用的是选择结构。 递归能使程序的结构更清晰、更简洁、更容易让人理解，从而减少读懂代码的时间。但是大量的递归调用会建立函数的副本，会耗费大量的时间和内存。迭代则不需要反复调用函数和占用额外的内存。因此我们应该视不同情况选择不同的代码实现方式。

在前行阶段，对于每一层递归，函数的局部变量、参数值以及返回地址都被压入栈中。在退回阶段，位于栈顶的局部变量、参数值和返回地址被弹出，用于返回调用层次中执行代码的其余部分，也就是恢复了调用的状态。

#### **用栈实现计算器**

中缀表达式实现

```c++
//不支持括号
#include <iostream>
#include <stack>
#include <string>

using namespace std;

int calculate(string s) {
    stack<int> nums;
    stack<char> ops;
    int num = 0;
    for (int i = 0; i < s.size(); i++) {
        char c = s[i];
        if (isdigit(c)) {
            num = num * 10 + (c - '0');
        }
        if (!isdigit(c) && c != ' ' || i == s.size() - 1) {
            if (!ops.empty() && (ops.top() == '*' || ops.top() == '/')) {
                int n = nums.top();
                nums.pop();
                if (ops.top() == '*') {
                    nums.push(n * num);
                } else {
                    nums.push(n / num);
                }
                ops.pop();
            } else if (!ops.empty() && (ops.top() == '+' || ops.top() == '-')) {
                if (ops.top() == '+') {
                    nums.push(num);
                } else {
                    nums.push(-num);
                }
                ops.pop();
            } else {
                nums.push(num);
            }
            ops.push(c);
            num = 0;
        }
    }
    int res = 0;
    while (!nums.empty()) {
        res += nums.top();
        nums.pop();
    }
    return res;
}

int main() {
    string s = "3+2*2";
    cout << calculate(s) << endl;
    return 0;
}
```

```c++
//支持括号
#include <iostream>
#include <stack>
#include <string>

using namespace std;

int calculate(string s) {
    stack<int> nums;
    stack<char> ops;
    int num = 0;
    for (int i = 0; i < s.size(); i++) {
        char c = s[i];
        if (isdigit(c)) {
            num = num * 10 + (c - '0');
        }
        if (c == '(') {
            int j = i + 1, cnt = 1;
            while (cnt != 0) {
                if (s[j] == '(') cnt++;
                if (s[j] == ')') cnt--;
                j++;
            }
            num = calculate(s.substr(i + 1, j - i - 2));
            i = j - 1;
        }
        if (!isdigit(c) && c != ' ' || i == s.size() - 1) {
            if (!ops.empty() && (ops.top() == '*' || ops.top() == '/')) {
                int n = nums.top();
                nums.pop();
                if (ops.top() == '*') {
                    nums.push(n * num);
                } else {
                    nums.push(n / num);
                }
                ops.pop();
            } else if (!ops.empty() && (ops.top() == '+' || ops.top() == '-')) {
                if (ops.top() == '+') {
                    nums.push(num);
                } else {
                    nums.push(-num);
                }
                ops.pop();
            } else {
                nums.push(num);
            }
            ops.push(c);
            num = 0;
        }
    }
    int res = 0;
    while (!nums.empty()) {
        res += nums.top();
        nums.pop();
    }
    return res;
}

int main() {
    string s = "3+2*(2+1)";
    cout << calculate(s) << endl;
    return 0;
}
```

逆波兰式实现

```c++
#include <iostream>
#include <stack>
#include <string>
#include <vector>

using namespace std;

int evalRPN(vector<string>& tokens) {
    stack<int> nums;
    for (int i = 0; i < tokens.size(); i++) {
        string s = tokens[i];
        if (s == "+" || s == "-" || s == "*" || s == "/") {
            int num2 = nums.top();
            nums.pop();
            int num1 = nums.top();
            nums.pop();
            if (s == "+") {
                nums.push(num1 + num2);
            } else if (s == "-") {
                nums.push(num1 - num2);
            } else if (s == "*") {
                nums.push(num1 * num2);
            } else {
                nums.push(num1 / num2);
            }
        } else {
            nums.push(stoi(s));
        }
    }
    return nums.top();
}

int main() {
    vector<string> tokens = {"3", "4", "+", "5", "-"};
    cout << evalRPN(tokens) << endl;
    return 0;
}
```

### 堆

堆是一种经过排序的树形数据结构，每个节点都有一个值，通常我们所说的堆的数据结构是指二叉树。所以堆在数据结构中通常可以被看做是一棵树的数组对象。而且堆需要满足一下两个性质：
 （1）堆中某个节点的值总是不大于或不小于其父节点的值；
 （2）堆总是一棵完全二叉树。

堆分为两种情况，有最大堆和最小堆。将根节点最大的堆叫做最大堆或大根堆，根节点最小的堆叫做最小堆或小根堆。下图图一就是一个最大堆，图二就是一个最小堆。在一个摆放好元素的最小堆中，可以看到，父结点中的元素一定比子结点的元素要小，但对于左右结点的大小则没有规定谁大谁小。

堆常用来实现优先队列，堆的存取是随意的，这就如同我们在图书馆的书架上取书，虽然书的摆放是有顺序的，但是我们想取任意一本时不必像栈一样，先取出前面所有的书，书架这种机制不同于箱子，我们可以直接取出我们想要的书。

![优先队列](二叉树.webp)

```c++
#include <iostream> 
using namespace std; 
// 定义堆的最大容量 
const int MAX_SIZE = 100; 
// 定义堆的数据结构 
int heap[MAX_SIZE]; 
int heapSize; 
// 向堆中插入一个元素 
void insert(int data) 
{ 
    // 如果堆已满，则不能插入 
    if (heapSize == MAX_SIZE) 
        return; 
    // 将新元素插入到堆的末尾 
    heap[heapSize] = data; 
    heapSize++; 
    // 将新元素上浮到正确的位置 
    int current = heapSize - 1; 
    while (current != 0 && heap[current] > heap[(current - 1) / 2]) 
    { 
        int temp = heap[(current - 1) / 2]; 
        heap[(current - 1) / 2] = heap[current]; 
        heap[current] = temp; 
        current = (current - 1) / 2; 
    } 
} 
// 从堆中删除一个元素 
void deleteElement() 
{ 
    // 如果堆为空，则不能删除 
    if (heapSize == 0) 
        return; 
    // 将堆的最后一个元素放到堆顶 
    heap[0] = heap[heapSize - 1]; 
    heapSize--; 
    // 将堆顶元素下沉到正确的位置 
    int current = 0; 
    while (current * 2 + 1 < heapSize) 
    { 
        int largest = current; 
        if (heap[largest] < heap[current * 2 + 1]) 
            largest = current * 2 + 1; 
        if (current * 2 + 2 < heapSize && heap[largest] < heap[current * 2 + 2]) 
            largest = current * 2 + 2; 
        if (largest == current) 
            break; 
        int temp = heap[current]; 
        heap[current] = heap[largest]; 
        heap[largest] = temp; 
        current = largest; 
    } 
} 
int main() 
{ 
    // 初始化堆 
    heapSize = 0; 
    // 向堆中插入元素 
    insert(10); 
    insert(20); 
    insert(30); 
    insert(40); 
    insert(50); 
    // 从堆中删除元素 
    deleteElement(); 
    // 输出堆中的元素 
    for (int i = 0; i < heapSize; i++) 
        cout << heap[i] << " "; 
    cout << endl; 
    return 0; 
}
```

#### 优先队列

![优先队列](优先队列.png)

### 队列

队列是只允许在一端进行插入操作、而在另一端进行删除操作的线性表。允许插入的一端称为队尾，允许删除的一端称为队头。它是一种特殊的线性表，特殊之处在于它只允许在表的前端进行删除操作，而在表的后端进行插入操作，和栈一样，队列是一种操作受限制的线性表。

而且队列是一种先进先出的数据结构，又称为先进先出的线性表，简称 FIFO（First In First Out）结构。也就是说先放的先取，后放的后取，就如同行李过安检的时候，先放进去的行李在另一端总是先出来，后放入的行李会在最后面出来。

解决假溢出的办法就是后面满了，就再从头开始，也就是头尾相接的循环。我们把队列的这种头尾相接的顺序存储结构称为循环队列。

队列和游标卡尺差不多，就是一个区间和线段组成的数据类型

![img](队列.png)

```c++
//声明变量 队列的性质就是先进先出
int q[N],hh,tt;
//插入数据
q[++tt];//hh是头（头是向右边移动的） tt是转移元素的变量（tt是新入队的成员）
//可以将 hh 和 tt看做是一个区间
hh++//pop
//判断队列是否为空
  if(hh<=tt) not empty
  else empty;
//取出队头元素
q[hh];
```

#### 单调队列

在中学的时候，我们会学一种函数的性质，叫做单调性。单调性是指，在一个区间内部我们可以预测这个数据类型的运行方向。那么显而易见的，单调队列指的是，我们可以预计和控制这个数据类型的运行方向。

队列的性质上面有提到，是一种先进先出的数据结构。我们可以利用这个结构，做一个可动的队列或者说，让数据集通过我们的队列，在通过的时候记录他们的数据变化。这样的操作，就叫做单调队列。

以一个较简单的例子：滑动窗口，大概题意就是，有一个数据条，需要通过我们的窗口（队列），通过的时候记录这个时候队列的最大值和最小值（或者别的什么东西 - 变化一下题目嘛）。

![滑动窗口](image-20230504225237230.png)

```c++
//维护窗口
int h = 1,t = 0; //清空队列
for(int i = 1;;i<=n;i++){ //枚举数据集
    while(h<=t && a[q[t]]>=a[i]) t--;//队尾出队
    q[++t] = i;//队尾入队
    if(q[h]<i-k+1) h++;//对头出队（滑出窗口）
    if(i>=k) printf("%d ",a[q[h]]);//打印最值
}
```

![维护窗口最小值](image-20230504230354394.png)

### 并查集

并查集是一种树形的数据结构，支持两种操作，并：将两个不同的元素合并成一个集合；查：查找一个数据的所在集合

在模拟树状结构的时候，fa[x]存储的是节点的父节点，可以理解为特定集合的编号。这里的x和赋值给f[x]的值的关系，可以理解为**节点和父节点连接**的关系。初始化的时候，将每一个节点编号都初始化为自己(自己为一个集合)。

![并查集](image-20230503221426175.png)

#### **查找**

可以理解为顺着当前节点向上爬，找到我们当前集合的根节点。但实际上，我们并不是这样做，我们可以将这个操作进行优化，我们在找根节点的过程，可以顺便将每一个经过的节点的**“父节点”都改为我们的“根节点”**，一个查找循环下来，整一个集合的值都是我们的，根节点（也就是我们当前集合编号），下面是代码解释。

![路径压缩](image-20230503222836310.png)

```c++
//未用路径压缩
int find(int x)
{
    //如果当前这个节点指向的是根节点，说明我们找到了目标集合，返回就行。
 if(fa[x] == x) return x;
    //如果不是，那就递归查找，直到找到我们的目标集合（一定找得到）。
    return find(fa[x]); 
}

//使用路径压缩
int find(int x)
{
    if(fa[x] == x) return x;
    return fa[x] = find(f[a]); //这里就是给路过的节点赋值 - 用的是递归
}
```

#### **合并**

![合并](image-20230503224008918.png)

合并操作，可以理解为两个集合的根节点指向即可。这里举个例子，A集合和B集合，那么B集合序号指向A集合序号即可。

```c++
//合并代码
void unionset(int x,int y){
    //将x的集合编号，赋值成y的集合编号
    //如果再次路径压缩，那么所有x集合的编号都会变成y的编号
    fa[find(x)] = find(y); 
}
```

但是，这样合并可以进行优化，我们可以这样想，如果我们合并两个集合的数量级不在一个级别上，比如一个集合元素个数为2，一个集合元素个数为2e10，这样将集合元素为2的集合作为新集合的标号，确实不太合适。所以说，我们可以直接给每一个集合标记他们的元素数量，然后对其进行合并即可。

```c++
vector<int> siz(N,1);
void unioset(int x,int y){
    x = find(x),y = find(y); //找到各自的集合编号
    if(x == y) return;
    if(siz[x]>siz[y]) swap(x,y);//判断哪个集合更大
    fa[x] = y;//小集合指向大集合
    siz[y]+=siz[x]; //合并一个大集合要加数量
}
```

![并查集的变化](image-20230503230130914.png)

### Trie

Trie树，即字典树，又称单词查找树或键树，是一种树形结构，是一种哈希树的变种。典型应用是用于统计和排序大量的字符串（但不仅限于字符串），所以经常被搜索引擎系统用于文本词频统计。它的优点是：利用字符串的公共前缀来减少查询时间，最大限度地减少无谓的字符串比较。

Trie的核心思想是空间换时间。利用字符串的**公共前缀**来降低查询时间的开销以达到提高效率的目的。

![字典树](image-20230504131211298.png)

#### 插入

Trie树，就是利用字符串的相同前缀，来节省我们查找的时间。这个树的根没有意义，只是为了构造这个树存在。可以说，根衔接的都是一个独立的单词（子树）。

![字典树](image-20230504132336132.png)

我们可以想象一下，在一个根下面链接一个一个单词会是什么样子，是不是像一个拂尘一样。我们可以在这个基础之上，对这些单词进行处理：单词是由26个字母排列组合成的，所以说不管单词怎么变，肯定有些单词的前缀是相等的。例如：appear 和 apple。这两个前缀相同，那么我们就可以让"app"为同一支，从p分出两个枝e和p。（当然分支最多26个）以此类推构建字典树。

```c++
//字典树构造
char s[N]
    
int ch[N][26];//儿子数组：存储从节点p沿着j这条路走到的子节点。26是26个字母的映射值（就是1代表a，2代表b以此类推）

int cnt[N];//存储以节点p结尾的单词的插入次数

int idx;//用来节点编号

//插入字符串
void insert(char *s)
{
    int p = 0;
    //遍历全部字符串的字符
    for(int i = 0;s[i];i++){
        int j = s[i]-'a';//字母映射 - 变为0-25的编号
        if(!ch[p][j]) ch[p][j]=++idx;//不存在这个值（就是树中无这个子树）
        p = ch[p][j];
    }
    cnt[p]++;//插入次数 - 表示当前节点子树个数
}
```

#### **查询**

和插入的操作差不多，只要找到我们插入的时候做的标记就行。

```c++
int query(char *s)
{
    int p = 0;;
    for(int i = 0;s[i];i++)
    {
        int j = s[i]-'a';//映射
        if(!ch[p][j]) return 0;;
        p = ch[p][j]; //查找符合，直到子串末尾。都符合则成立
    }
    return cnt[p];
}
```

![总结](image-20230504140704449.png)

### KMP

在学习kmp算法之前，我们先弄清楚暴力求解字符串匹配问题。

给出一个主串S，判断字符串F是否是S的子串，如果是返回T在S中出现的第一个位置，否则返回0.

使用暴力算法解题，从主串S的第一个字符开始和模式串T的第一个字符进行比较，若相等则比较二者后续字符，否则，主串回溯到第二个字符，模式串回溯到第一个字符；继续比较，不匹配，主串回溯到第三个字符，模式串回溯到第一个字符。重新上述过程，直到模式串T中的字符全部比较完毕，说明匹配成功；否则匹配失败。
暴力算法将每一种情况都列举了出来，时间复杂度很高(大概是o(n^2))

```c++
#include <iostream>
using namespace std;

int BF(char *S,char *T){
    int i=0,j=0;
    while(S[i]!='\0'&&T[j]){
        if(S[i]==T[j]){
            i++;
            j++;
        }else{
            i=i-j+1;//主串回溯到上次回溯位置的下一个位置
            j=0;//模式串回溯到第一个位置
        }
    }
    if(T[j]=='\0')
        return i-j+1; //返回T在S中第一次出现的位置
    else return 0;
}

int main(){

    char S[50]={'a','b','a','c','b','a','b','c'};
    char T[50]={'b','a','b','c'};
    cout<<BF(S,T)<<"\n";
    return 0;
}
```

在使用暴力匹配的时候，我们不难发现，每一轮如果匹配失效，那么就会直接放弃这一轮匹配的所有信息；进入到下一轮就重新匹配了。

Kmp算法就是将这一部分信息利用起来，使得下一轮匹配的时候不用重新开始，而是在前面的基础之上回退我们匹配串和前移我们的主串。这样就保留了匹配的信息。

![比较](image-20230504163459568.png)

那我们怎么利用信息呢：我们需要找到匹配串的相等前后缀，将其记录起来：这里就用next数组存储信息

#### next函数

![相等前后缀](image-20230504164818659.png)

那这个next数组有什么用，我们找到相等前后缀的目的是为了保留信息，如果在7这个位置匹配失败了，那么只要找到在6这个位置的前后缀相等的，跳回到最大相等前后缀就行。因为在7之前我们的字符串是完全匹配的，可以吧ne数组理解为匹配位置的长度。

![next数组](image-20230504170809559.png)

```c++
ne[1] = 0;
for(int i = 2,j = 0;i<=n;i++){
    while(j&&P[i]!=P[j+1]) j = ne[j];
    if(P[i] == P[j+1]) j++;
    ne[i] = j;
}
```

![next构造](image-20230504171245977.png)

#### 匹配

这里，使用了双指针思想。i指针维护主串，j指针维护匹配串，i指针只能向前移动，j指针可以来回调整。

![匹配过程](image-20230504172311018.png)

```c++
for(int i = 1,j = 0;i<=m;i++){
    while(j&&S[i]!=P[j+1]) j = ne[j];//回调操作 - ne数组存储的是最大匹配串长度（1-j的），这里赋值相当于将j回调到最大匹配的位置
    if(S[i] == P[j+1]) j++;
    if(j==n) printf("%d\n",i-n+1);
}
```

![kmp时间复杂度](image-20230504172557271.png)

那么总的时间复杂度就是O(m+n)

![总结](image-20230504173358331.png)
