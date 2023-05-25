---
title: One 基本算法
date: 2023-04-26 08:20:07
tags: 算法
description: 简单介绍一下基础的算法
typora-root-url: One
categories: 算法
---

## 第一章 基础算法

## 时间复杂度和空间复杂度的基本估计

![时间复杂度](时间复杂度.png)

## 基本算法说明

### 枚举

```c++
//1.枚举的定义
  枚举（英语：Enumerate）是基于已有知识来猜测答案的一种问题求解策略。
  枚举的思想是不断地猜测，从可能的集合中一一尝试，然后再判断题目的条件是否成立。
//2.简单来说，枚举就是在有限次的比对中，找到与正确答案符合的数据。
    枚举经常出现，算是最为基本的算法 - 之后对枚举的优化也是一些算法复杂度低的原因
    总体来说，枚举的复杂度需要看我们取得的数据的个数 一个那就是 o(n) 多个就是o(n^i)
//3.简单举例
    要求一个数组中，有多少个0
    for(int i = i;i<=n;i++) if(a[i]==0) ans++
    return ans
```

```c++
//深入研究
 枚举算法是我们在日常中使用到的最多的一个算法，它的核心思想就是:枚举所有的可能。
 枚举法的本质就是从所有候选答案中去搜索正确的解,使用该算法需要满足两个条件：
  (1)可预先确定候选答案的数量；//可以确定答案就在我们枚举的范围之中
  (2)候选答案的范围在求解之前必须有一个确定的集合。//范围
  枚举算法简单粗暴，他暴力的枚举所有可能，尽可能地尝试所有的方法。虽然枚举算法非常暴力，而且速度可能很慢，但确实我们最应该优先考虑的！因为枚举法变成实现最简单，并且得到的结果总是正确的。
 枚举算法分为循环枚举、子集枚举、排列枚举三种。
```

### 模拟

```c++
//说白了 模拟题就是应用题 给出一个项目让你实现它

//写模拟题时，遵循以下的建议有可能会提升做题速度：
  1.在动手写代码之前，在草纸上尽可能地写好要实现的流程。
  2.在代码中，尽量把每个部分模块化，写成函数、结构体或类。
  3.对于一些可能重复用到的概念，可以统一转化，方便处理：如，某题给你 "YY-MM-DD 时：分" 把它抽取到一个函数，处理成秒，会减少概念混淆。
  4.调试时分块调试。模块化的好处就是可以方便的单独调某一部分。
  5.写代码的时候一定要思路清晰，不要想到什么写什么，要按照落在纸上的步骤写。
```

### 递归和分治 - 简单了解

```c++
//1.递归 
 函数调用自身。递归的基本思想是某个函数直接或者间接地调用自身，这样原问题的求解就转换为了许多性质相同但是规模更小的子问题。求解时只需要关注如何把原问题划分成符合条件的子问题，而不需要过分关注这个子问题是如何被解决的。
     int func(传入数值) {
        if (终止条件) return 最小子问题解;
        return func(缩小规模);
  }
//2.递归的实现
 只需要在乎，把大问题分解为许多小问题就行了，不用在意子问题如何实现。 当然，我们得初始化刚开始的子问题（要不然后面的问题就无法按找 小问题变为大问题递推出来）
//3.缺点
    递归是利用堆栈来实现的。每当进入一个函数调用，栈就会增加一层栈帧，每次函数返回，栈就会减少一层栈帧。而栈不是无限大的，当递归层数过多时，就会造成 栈溢出 的后果。（但是我没遇过）
//4.递归的要点
    明白一个函数的作用并相信它能完成这个任务，千万不要跳进这个函数里面企图探究更多细节， 否则就会陷入无穷的细节无法自拔，人脑能压几个栈啊。（明白每个函数能做的事，并相信他们能够完成和我之前想的不谋而合）
//举例 遍历二叉树
    void traverse(TreeNode* root) {
     if (root == nullptr) return;
        traverse(root->left);
     traverse(root->right);
}

```

```c++
//1.分治
 说白了就是，将大问题分成多个小问题（一般都是 二分 logn 次啦），然后再对小问题得出的数据进行操作，归并排序就是将小问题按照既定规则拼回去.其他的也差不多
//2.分治法的流程
 大概的流程可以分为三步：分解 -> 解决 -> 合并。
1.分解原问题为结构相同的子问题。
2.分解到某个容易求解的边界之后，进行递归求解。
3.将子问题的解合并成原问题的解。

//分治法能解决的问题一般有如下特征：
1.该问题的规模缩小到一定的程度就可以容易地解决。
2.该问题可以分解为若干个规模较小的相同问题，即该问题具有最优子结构性质，利用该问题分解出的子问题的解可以合并为该问题的解。
3.该问题所分解出的各个子问题是相互独立的，即子问题之间不包含公共的子问题（子问题独立）。（公共的子问题需要动态规划来解决）
```

### 贪心 - 简单了解

```c++
//1.概念
 贪心算法（英语：greedy algorithm），是用计算机来模拟一个「贪心」的人做出决策的过程。这个人十分贪婪，每一步行动总是按某种指标选取最优的操作。而且他目光短浅，总是只看眼前，并不考虑以后可能造成的影响。
 可想而知，并不是所有的时候贪心法都能获得最优解，所以一般使用贪心法的时候，都要确保自己能证明其正确性。一般来说，我们可以引用某一个步骤来反证我们的答案是否正确
//2.常见题型
 「我们将 XXX 按照某某顺序排序，然后按某种顺序（例如从小到大）选择。」。（离线 先处理在选择）
  「我们每次都取 XXX 中最大/小的东西，并更新 XXX。」（有时「XXX 中最大/小的东西」可以优化，比如用优先队列维护）（边处理 边选择）
//3.两种方法
      01.排序解法 用排序法常见的情况是输入一个包含几个（一般一到两个）权值的数组，通过排序然后遍历模拟计算的方法求出最优值。
      02.后悔解法 思路是无论当前的选项是否最优都接受，然后进行比较，如果选择之后不是最优了，则反悔，舍弃掉这个选项；否则，正式接受。如此往复。
//4.特殊性质
      贪心算法与动态规划的不同在于它对每个子问题的解决方案都做出选择，不能回退。动态规划则会保存以前的运算结果，并根据以前的结果对当前进行选择，有回退功能。
```

### 排序 - 简单介绍

> [十大经典排序算法 | 菜鸟教程 (runoob.com)](https://www.runoob.com/w3cnote/ten-sorting-algorithm.html)

#### 排序的时间复杂度分析

![sort](sort.png)

![排序数据比较](排序数据比较.png)

#### 冒泡排序操作

![冒泡排序](冒泡排序.gif)

```c++
冒泡排序
 冒泡排序的时间复杂度是 o(n^2),这个复杂度是由冒泡排序的操作约束的。冒泡排序的原理是，选择我们序列中的一个值，对其进行这样的操作：
 -将当前这个值和下一个数据对比，如果当前这个值大于下一个数据，交换；否则，下一个值作为新的交换值代替之前的值执行交换程序。直到最后，没有数据进行比对，退出程序。
// 菜鸟 - 比较相邻的元素。如果第一个比第二个大，就交换他们两个。对每一对相邻元素作同样的工作，从开始第一对到结尾的最后一对。这步做完后，最后的元素会是最大的数。针对所有的元素重复以上的步骤，除了最后一个。持续每次对越来越少的元素重复上面的步骤，直到没有任何一对数字需要比较。
    
//代码    
#include <iostream>
using namespace std;
template<typename T> //整数或浮点数皆可使用,若要使用类(class)或结构体(struct)时必须重载大于(>)运算符
void bubble_sort(T arr[], int len) {
        int i, j;
        for (i = 0; i < len - 1; i++)
                for (j = 0; j < len - 1 - i; j++)
                        if (arr[j] > arr[j + 1])
                                swap(arr[j], arr[j + 1]);
}
int main() {
        int arr[] = { 61, 17, 29, 22, 34, 60, 72, 21, 50, 1, 62 };
        int len = (int) sizeof(arr) / sizeof(*arr);
        bubble_sort(arr, len);
        for (int i = 0; i < len; i++)
                cout << arr[i] << ' ';
        cout << endl;
        float arrf[] = { 17.5, 19.1, 0.6, 1.9, 10.5, 12.4, 3.8, 19.7, 1.5, 25.4, 28.6, 4.4, 23.8, 5.4 };
        len = (float) sizeof(arrf) / sizeof(*arrf);
        bubble_sort(arrf, len);
        for (int i = 0; i < len; i++)
                cout << arrf[i] << ' '<<endl;
        return 0;
}    
```

#### 选择排序操作

![选择排序](selectionSort.gif)

```c++
2.选择排序
    
//选择排序的时间复杂度是o(n^2) - 是因为选择排序需要进行 n-1 轮比较，每轮比较需要比较 n-i 次，所以总共需要比较 (n-1) + (n-2) + … + 1 = n(n-1)/2 次，因此时间复杂度为 O(n²)。

 首先在未排序序列中找到最小（大）元素，存放到排序序列的起始位置。再从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。重复第二步，直到所有元素均排序完毕。就是每次选中一个未排序序列的起点，然后在未排序序列中，找到一个“最小值”直到遍历完区间之后，将其标记未排序序列（区间缩小），重复直到全为排序区间，退出程序。
    
//代码
template<typename T> //整數或浮點數皆可使用，若要使用物件（class）時必須設定大於（>）的運算子功能
void selection_sort(std::vector<T>& arr) {
        for (int i = 0; i < arr.size() - 1; i++) {
                int min = i;
                for (int j = i + 1; j < arr.size(); j++)
                        if (arr[j] < arr[min])
                                min = j;
                std::swap(arr[i], arr[min]);
        }
}     
```

#### 插入排序操作

![插入排序](insertionSort.gif)

```c++
//插入排序的复杂度是 o(n^2) 每一个数据最多要比较（n-1)次，所以说n*(n-1) = n^2

 将第一待排序序列第一个元素看做一个有序序列，把第二个元素到最后一个元素当成是未排序序列。从头到尾依次扫描未排序序列，将扫描到的每个元素插入有序序列的适当位置。（如果待插入的元素与有序序列中的某个元素相等，则将待插入元素插入到相等元素的后面。）
 它的工作原理是通过构建有序序列，对于未排序数据，在已排序序列中从后向前扫描，找到相应位置并插入。   
        
//和打扑克牌是一个道理，开始的时候我们只有一张牌（假设），我们将这张牌当做一个有序序列，每次加入新的牌的时候和有序序列进行比较插入，按照插入的规则（从小到大 还是 从大到小）进行判断插入。（感觉就是和选择排序是相反的，从无序中抽数据和有序序列进行比对）

//代码
void insertion_sort(int arr[],int len){
        for(int i=1;i<len;i++){
                int key=arr[i];
                int j=i-1;
                while((j>=0) && (key<arr[j])){
                        arr[j+1]=arr[j];
                        j--;
                }
                arr[j+1]=key;
        }
}
```

#### 希尔排序操作

![希尔排序](Sorting_shellsort_anim.gif)

> 引用文章<[(106条消息) 排序算法 | 希尔shell排序，算法的图解、实现、复杂度和稳定性分析_shell排序时间复杂度_比特的一天的博客-CSDN博客](https://blog.csdn.net/qq_43473694/article/details/112197066)>

```c++
//希尔排序的时间复杂度是：O(n^1.3） 在最坏情况之下是o(n^2) - 因为是基于插入排序
 希尔排序：先追求部分元素有序，然后逼近全局有序！希尔排序是基于插入排序的以下两点性质而提出改进方法的：
 - 插入排序在对几乎已经排好序的数据操作时，效率高，即可以达到线性排序的效率；
 - 但插入排序一般来说是低效的，因为插入排序每次只能将数据移动一位；
 选择一个增量序列 t1，t2，……，tk，其中 ti > tj, tk = 1；按增量序列个数 k，对序列进行 k 趟排序；每趟排序，根据对应的增量 ti，将待排序列分割成若干长度为 m 的子序列，分别对各子表进行直接插入排序。仅增量因子为 1 时，整个序列作为一个表来处理，表长度即为整个序列的长度。
        
//代码
template<typename T>
void shell_sort(T array[], int length) {
    int h = 1;
    while (h < length / 3) {
        h = 3 * h + 1;
    }
    while (h >= 1) {
        for (int i = h; i < length; i++) {
            for (int j = i; j >= h && array[j] < array[j - h]; j -= h) {
                std::swap(array[j], array[j - h]);
            }
        }
        h = h / 3;
    }
}   
```

#### 归并排序

![归并排序](mergeSort.gif)

```c++
//归并排序的时间复杂度是 o(log n)
 归并排序的算法思想是：分治法。 将大问题分解成许多的子问题，在将子问题合并成为我们的大问题
//实现方法
 自上而下的递归（所有递归的方法都可以用迭代重写，所以就有了第 2 种方法）自下而上的迭代
 申请空间，使其大小为两个已经排序序列之和，该空间用来存放合并后的序列；设定两个指针，最初位置分别为两个已经排序序列的起始位置；比较两个指针所指向的元素，选择相对小的元素放入到合并空间，并移动指针到下一位置；重复步骤 3 直到某一指针达到序列尾；将另一序列剩下的所有元素直接复制到合并序列尾。 

//非递归
template<typename T> // 整數或浮點數皆可使用,若要使用物件(class)時必須設定"小於"(<)的運算子功能
void merge_sort(T arr[], int len) {
    T *a = arr;
    T *b = new T[len];
    for (int seg = 1; seg < len; seg += seg) {
        for (int start = 0; start < len; start += seg + seg) {
            int low = start, mid = min(start + seg, len), high = min(start + seg + seg, len);
            int k = low;
            int start1 = low, end1 = mid;
            int start2 = mid, end2 = high;
            while (start1 < end1 && start2 < end2)
                b[k++] = a[start1] < a[start2] ? a[start1++] : a[start2++];
            while (start1 < end1)
                b[k++] = a[start1++];
            while (start2 < end2)
                b[k++] = a[start2++];
        }
        T *temp = a;
        a = b;
        b = temp;
    }
    if (a != arr) {
        for (int i = 0; i < len; i++)
            b[i] = a[i];
        b = a;
    }
    delete[] b;
}

//递归
template<typename T> // 整數或浮點數皆可使用,若要使用物件(class)時必須設定"小於"(<)的運算子功能
void merge_sort(T arr[], int len) {
    T *a = arr;
    T *b = new T[len];
    for (int seg = 1; seg < len; seg += seg) {
        for (int start = 0; start < len; start += seg + seg) {
            int low = start, mid = min(start + seg, len), high = min(start + seg + seg, len);
            int k = low;
            int start1 = low, end1 = mid;
            int start2 = mid, end2 = high;
            while (start1 < end1 && start2 < end2)
                b[k++] = a[start1] < a[start2] ? a[start1++] : a[start2++];
            while (start1 < end1)
                b[k++] = a[start1++];
            while (start2 < end2)
                b[k++] = a[start2++];
        }
        T *temp = a;
        a = b;
        b = temp;
    }
    if (a != arr) {
        for (int i = 0; i < len; i++)
            b[i] = a[i];
        b = a;
    }
    delete[] b;
}
```

#### 快速排序

![快速排序](quickSort.gif)

```c++
//快速排序的时间复杂度是o(nlog n),一般而言是比其他nlogn排序是要快的

 从数列中挑出一个元素，称为 "基准"（pivot）;
 重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个分区退出之后，"该基准就处于数列的中间位置"。这个称为分区（partition）操作；
    递归（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序；

//就是由基准分成两部分，再由两部分的基准分成另外两部分，以此类推。 - 直到所有部分都符合基准，即可退出。
        
//代码        
//严蔚敏《数据结构》标准分割函数
 int Paritition1(int A[], int low, int high) {
   int pivot = A[low];
   while (low < high) {
     while (low < high && A[high] >= pivot) {
       --high;
     }
     A[low] = A[high];
     while (low < high && A[low] <= pivot) {
       ++low;
     }
     A[high] = A[low];
   }
   A[low] = pivot;
   return low;
 }

 void QuickSort(int A[], int low, int high) //快排母函数
 {
   if (low < high) {
     int pivot = Paritition1(A, low, high);
     QuickSort(A, low, pivot - 1);
     QuickSort(A, pivot + 1, high);
   }
 }
```

#### 堆排序

![堆排序](heapSort.gif)

![堆排序](https://www.runoob.com/wp-content/uploads/2019/03/Sorting_heapsort_anim.gif)

```c++
//堆排序 - 是利用数据结构中的堆的性质设计的算法，它的时间复杂度是o(nlogn)
 大顶堆：每个节点的值都大于或等于其子节点的值，在堆排序算法中用于升序排列；
 小顶堆：每个节点的值都小于或等于其子节点的值，在堆排序算法中用于降序排列；
//算法步骤
 创建一个堆 H[0……n-1]；
 把堆首（最大值）和堆尾互换；
 把堆的尺寸缩小 1，并调用 shift_down(0)，目的是把新的数组顶端数据调整到相应位置；
 重复步骤 2，直到堆的尺寸为 1。
//代码
#include <iostream>
#include <algorithm>
using namespace std;

void max_heapify(int arr[], int start, int end) {
    // 建立父節點指標和子節點指標
    int dad = start;
    int son = dad * 2 + 1;
    while (son <= end) { // 若子節點指標在範圍內才做比較
        if (son + 1 <= end && arr[son] < arr[son + 1]) // 先比較兩個子節點大小，選擇最大的
            son++;
        if (arr[dad] > arr[son]) // 如果父節點大於子節點代表調整完畢，直接跳出函數
            return;
        else { // 否則交換父子內容再繼續子節點和孫節點比較
            swap(arr[dad], arr[son]);
            dad = son;
            son = dad * 2 + 1;
        }
    }
}

void heap_sort(int arr[], int len) {
    // 初始化，i從最後一個父節點開始調整
    for (int i = len / 2 - 1; i >= 0; i--)
        max_heapify(arr, i, len - 1);
    // 先將第一個元素和已经排好的元素前一位做交換，再從新調整(刚调整的元素之前的元素)，直到排序完畢
    for (int i = len - 1; i > 0; i--) {
        swap(arr[0], arr[i]);
        max_heapify(arr, 0, i - 1);
    }
}

int main() {
    int arr[] = { 3, 5, 3, 0, 8, 6, 1, 5, 8, 6, 2, 4, 9, 4, 7, 0, 1, 8, 9, 7, 3, 1, 2, 5, 9, 7, 4, 0, 2, 6 };
    int len = (int) sizeof(arr) / sizeof(*arr);
    heap_sort(arr, len);
    for (int i = 0; i < len; i++)
        cout << arr[i] << ' ';
    cout << endl;
    return 0;
}
```

#### 计数排序

![计数排序](countingSort.gif)

```c++
//计数排序的核心在于将输入的数据值转化为键存储在额外开辟的数组空间中。作为一种线性时间复杂度的排序，计数排序要求输入的数据必须是有确定范围的整数。
//计数排序的时间复杂度是o(n+K)
 简单说，就是用我们"需要排序的序列的值",创建一个标记数组，只要序列出现过的值，都标记成1，就说明它出现了。但是，这种方法只对数来说比较方便。 - 可以说是数组下标和排序序列有映射关系
      
//算法步骤
（1）找出待排序的数组中最大和最小的元素
（2）统计数组中每个值为i的元素出现的次数，存入数组C的第i项
（3）对所有的计数累加（从C中的第一个元素开始，每一项和前一项相加）
（4）反向填充目标数组：将每个元素i放在新数组的第C(i)项，每放一个元素就将C(i)减去1

//代码（c）
#include <stdio.h>
#include <stdlib.h>
#include <time.h>

void print_arr(int *arr, int n) {
        int i;
        printf("%d", arr[0]);
        for (i = 1; i < n; i++)
                printf(" %d", arr[i]);
        printf("\n");
}

void counting_sort(int *ini_arr, int *sorted_arr, int n) {
        int *count_arr = (int *) malloc(sizeof(int) * 100);
        int i, j, k;
        for (k = 0; k < 100; k++)
                count_arr[k] = 0;
        for (i = 0; i < n; i++)
                count_arr[ini_arr[i]]++;
        for (k = 1; k < 100; k++)
                count_arr[k] += count_arr[k - 1];
        for (j = n; j > 0; j--)
                sorted_arr[--count_arr[ini_arr[j - 1]]] = ini_arr[j - 1];
        free(count_arr);
}

int main(int argc, char **argv) {
        int n = 10;
        int i;
        int *arr = (int *) malloc(sizeof(int) * n);
        int *sorted_arr = (int *) malloc(sizeof(int) * n);
        srand(time(0));
        for (i = 0; i < n; i++)
                arr[i] = rand() % 100;
        printf("ini_array: ");
        print_arr(arr, n);
        counting_sort(arr, sorted_arr, n);
        printf("sorted_array: ");
        print_arr(sorted_arr, n);
        free(arr);
        free(sorted_arr);
        return 0;
}        


```

#### 桶排序 - 不懂

元素分布在桶中：

![桶](Bucket_sort_1.svg_.png)

然后，元素在每个桶中排序：

![桶](Bucket_sort_2.svg_.png)

```c++
//桶排序是计数排序的升级版。它利用了函数的映射关系，高效与否的关键就在于这个映射函数的确定。
//为了使桶排序更加高效，我们需要做到这两点：在额外空间充足的情况下，尽量增大桶的数量使用的映射函数能够将输入的 N 个数据均匀的分配到 K 个桶中
计数排序每个位置只能装一种数据，而桶排序中，可以装入一个范围（集合）的数据是这个样子吗 - 感觉类似于哈希表
        
        
//代码
#include <iostream>
#include <algorithm>
using namespace std;

const int N = 100010;

int n;
int a[N], b[N];

int main()
{
    cin >> n;
    for (int i = 0; i < n; i ++ ) cin >> a[i];
    int maxv = *max_element(a, a + n);
    for (int i = 0; i < n; i ++ ) b[a[i]] ++ ;
    for (int i = 0, j = 0; i <= maxv; i ++ )
        while (b[i] -- ) a[j ++ ] = i;
    for (int i = 0; i < n; i ++ ) cout << a[i] << ' ';
    return 0;
}        
```

#### 基数排序 - 不懂

![基数排序](radixSort.gif)

```c++
//基数排序：根据键值的每位数字来分配桶；
//基数排序是一种非比较型整数排序算法，其原理是将整数按位数切割成不同的数字，然后按每个位数分别比较。由于整数也可以表达字符串（比如名字或日期）和特定格式的浮点数，所以基数排序也不是只能使用于整数。


//代码
int maxbit(int data[], int n) //辅助函数，求数据的最大位数
{
    int maxData = data[0];              ///< 最大数
    /// 先求出最大数，再求其位数，这样有原先依次每个数判断其位数，稍微优化点。
    for (int i = 1; i < n; ++i)
    {
        if (maxData < data[i])
            maxData = data[i];
    }
    int d = 1;
    int p = 10;
    while (maxData >= p)
    {
        //p *= 10; // Maybe overflow
        maxData /= 10;
        ++d;
    }
    return d;
/*    int d = 1; //保存最大的位数
    int p = 10;
    for(int i = 0; i < n; ++i)
    {
        while(data[i] >= p)
        {
            p *= 10;
            ++d;
        }
    }
    return d;*/
}
void radixsort(int data[], int n) //基数排序
{
    int d = maxbit(data, n);
    int *tmp = new int[n];
    int *count = new int[10]; //计数器
    int i, j, k;
    int radix = 1;
    for(i = 1; i <= d; i++) //进行d次排序
    {
        for(j = 0; j < 10; j++)
            count[j] = 0; //每次分配前清空计数器
        for(j = 0; j < n; j++)
        {
            k = (data[j] / radix) % 10; //统计每个桶中的记录数
            count[k]++;
        }
        for(j = 1; j < 10; j++)
            count[j] = count[j - 1] + count[j]; //将tmp中的位置依次分配给每个桶
        for(j = n - 1; j >= 0; j--) //将所有桶中记录依次收集到tmp中
        {
            k = (data[j] / radix) % 10;
            tmp[count[k] - 1] = data[j];
            count[k]--;
        }
        for(j = 0; j < n; j++) //将临时数组的内容复制到data中
            data[j] = tmp[j];
        radix = radix * 10;
    }
    delete []tmp;
    delete []count;
}
```
