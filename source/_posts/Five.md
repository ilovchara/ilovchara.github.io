---
title: 数论 - 简单讲解数论
date: 2023-04-28 09:09:10
tags: 算法
categories: 算法
---

## 第五章 - 数论

```c++
1.质数的作用
2.各种知识
```

## 1.费马小定理

```c++
//前置消息
辗转相除法 gcd(a,b); //求取最小公倍数
#include <iostream> 
using namespace std; 
  
// Function to compute gcd of two numbers 
int gcd(int a, int b) 
{ 
    if (b == 0) 
        return a; 
    return gcd(b, a % b); 
} 
  
//main function
int main() 
{ 
    int a = 98, b = 56; 
    cout << "GCD of " << a << " and " << b << " is " << gcd(a, b); 
    return 0; 
}

```

```c++
//费马小定理

//它指出，如果一个自然数p是一个素数，并且a是一个不能被p整除的任意一个正整数，那么a的p次方一定模p等于a。

#include <iostream>
using namespace std;

int main(){
    int a, b, c;
    cout << "Enter two different numbers: ";
    cin >> a >> b;
    c = a*a + b*b;
    
    if(c % (a+b) == 0)
        cout << "弗尔马尔的小定理得到验证。";
    else 
        cout << "Felmar的小定理尚未得到验证";
    
    return 0;
}
```

## 2.欧拉函数(有啥用？)*

```c++
//欧拉函数的理论
 欧拉函数是被称为有限欧拉定理的函数，它在数论中广泛使用。欧拉函数的定义是：
   Φ(N) = (p1^k1)*(p2^k2)…*(pr^kr)，
   其中p1、p2…pr是大于1的所有不相等的质数，而k1、k2…kr是正整数。
   Φ(N)表示小于等于N的正整数中，与N互质的数之积。欧拉函数的意义就是计算满足特定限制条件的正整数值
```

```c++
//欧拉函数的代码生成
#include"iostream"

using namespace std;

//定义欧拉函数
long euler(long n)
{
    long ans=n,i,j;
    for ( i=2;i*i<=n;i++)
    {
        if(n%i==0)
        {
            ans=ans-ans/i;
            while(n%i==0) 
            {
                n/=i;
            }
        }
    }
    if(n>1) 
    {
        ans=ans-ans/n;
    }
    return ans;
}

int main()
{
    long n;
 
    cout << "请输入一个正整数：";
    cin >> n;
    cout << "此数的欧拉函数值为："<< euler(n) << endl;
    return 0;
}
```

## 3.扩展欧几里得算法

```c++
//前置知识 线性同余方程
 线性同余方程是一种数学模型，可以用来求解根据已知条件计算未知变量的问题。它是一个形如ax≡b (modn) 的方程，其中a、b和n是已知常数，x是待求解的未知变量。
  线性同余方程是用来解决模运算的一种数学方法，它可以解决大多数的组合数学问题，包括数论和计算机科学等，在特定情况下甚至可以用来分析给定的数学问题。线性同余方程在破解加密算法以及模数运算中也有所应用。
              ax ≡ b (mod n)
```

```c++
//扩展欧几里得算法作用
扩展欧几里得算法是一个整数线性规划算法，用于通过有限的整数操作来求解ax + by = gcd (a, b)的问题，其中a和b是两个正整数。它也可以用于计算多元最大公因子(gcd)，即求出多个整数的最大公约数。
```

```c++
//扩展欧几里得算法

#include<iostream>
using namespace std;
 
//扩展欧几里得算法
int exgcd(int a, int b, int &x, int &y)
{
    if (!b) { x = 1, y = 0; return a; }
    int d = exgcd(b, a%b, x, y);
    int t = x; y = x - a/b*y; x = t;
    return d;
}
 
int main()
{
    int a, b, x, y, ans;
    cin>>a>>b;
    ans = exgcd(a, b, x, y);
    cout<<"ax+by=d="<<ans<<endl;
    cout<<x<<" "<<y<<endl;
    return 0;
}
```

```c++
//快速幂问题
  方法1：最朴素的想法，7*7=49，49*7=343，... 一步一步算，共进行了9次乘法。
  这样算无疑太慢了，尤其对计算机的CPU而言，每次运算只乘上一个个位数，无疑太屈才了。这时我们想到，也许可以拆分问题。
  方法2：先算7的5次方，即7*7*7*7*7，再算它的平方，共进行了5次乘法。但这并不是最优解，因为对于“7的5次方”，我们仍然可以拆分问题。
  方法3：先算7*7得49，则7的5次方为49*49*7，再算它的平方，共进行了4次乘法。模仿这样的过程，我们得到一个在 o(log n)时间内计算出幂的算法，也就是快速幂。
```

```c++
//扩展欧几里得算法实现快速幂


#include<iostream> 
using namespace std; 
long long quick_mod(long long a,long long b,long long c) 
{ 
    long long ans=1; 
    while(b) 
    { 
        if(b&1) 
            ans=(ans*a)%c; 
        b=b>>1; 
        a=(a*a)%c; 
    } 
    return ans; 
} 
int main() 
{ 
    long long a,b,c; 
    cin>>a>>b>>c; 
    cout<<quick_mod(a,b,c); 
    return 0; 
}
```

## 4.乘法逆元

```c++
//乘法逆元
乘法逆元是乘法运算在模意义下的一个概念，即存在一个数a，使得a*b≡1（mod n），其中a就叫做b的乘法逆元，记为b^(-1) (mod n)。它的用处是在模意义下实现除法的目的，可以用来求模意义下的乘法对角线，解方程组等。
```

```c++
//扩展欧几里得算法实现逆元
int exgcd(int a, int b, int &x, int &y) 
{
    if (b == 0) 
    {
        x = 1;
        y = 0;
        return a;
    }
    int r = exgcd(b, a % b, x, y);
    int t = x;
    x = y;
    y = t - a / b * y;
    return r;
}

int inverse(int a, int m) 
{
    int x, y;
    int gcd = exgcd(a, m, x, y);
    return x >= 0 ? x : x + m;
}
```

## 5.贝祖定理

```c++
//贝祖定理 
  贝祖定理是一个重要的概率论原理，用于求解含有多个独立随机变量的复合分布概率。它可以用来解决一些关于概率分布的问题，比如计算多个独立事件的概率之和，计算某个事件出现的概率，计算两个事件概率的乘积以及计算某个条件发生的概率等问题。
```

``` c++
#include <iostream> 
using namespace std;
 
int main()
{
    int n, k;
    cout<<"Please input two integers (n, k): ";
    cin>>n>>k;
 
    // Compute C(n, k) using the formula
    long c = 1;
    for(int i = 1; i <= k; i++)
        c = c * (n - i + 1) / i;
 
    cout<<"C("<<n<<", "<<k<<") = "<<c<<endl;
 
    return 0;
}
```

## 6.中国剩余定理

```c++
中国剩余定理的用途有： 
      1、在数论中，它可用于解决同余方程； 
      2、在密码学中，它可用于将私钥分成多份存放，并能通过合并多份共享私钥来解密信息；       3、在密码学中，它还可以用于生成曲线密码，能够抗攻击； 
    4、在数值分析中，它可用于解决系统方程组； 
    5、在计算机科学中，它可用于解决复杂图形计算问题； 
    6、它还可以用于解决一系列复杂的模式识别问题； 
    7、在数据库领域，它可用于解决复杂的查询优化问题
```

```c++
/*
* 中国剩余定理：求解一组数的方程组 x = ai (mod ni) 所有a_i, n_i都是正整数 
* 并且互质，即gcd(n_i, n_j) = 1 (1 ≤ i ≤ k, 1 ≤ j ≤ k, i ≠j)则存在这样一个整数x，
* 使得x≡ai(modni)成立，且0≤x<n1 * n2 *...nk。
*/
int chinese_remainder(int *n, int *a, int len) {  // 传入ni和ai的数组，len表示数组的长度
 int p, i, j, m, n_i, x; //定义变量
 x = 0;
 p = 1;
 for (i = 0; i < len; i++) { // 求出n1 x n2 x ... nk
  p *= n[i];
 }

 // 中国剩余定理算法 
 // 之所以不影响x的值，是因为m == (p / n[i]) * ai mod n[i]
 for (i = 0; i < len; i++) {
  n_i = p / n[i];
  m = extended_Euclid(n_i, n[i]);  //  扩展欧几里得算法计算出逆元
  x += a[i] * m * n_i;
 }

 return x % p;
}

// 扩展欧几里得算法
int extended_Euclid(int a, int b) {
 int x, y, d;
 if (b == 0) {
  x = 1;
  y = 0;
  d = a;
  return d;
 }
 int x1, y1, d1;
 d1 = extended_Euclid(b, a % b);
 x1 = y1;  // 记录上一次的结果 
 y1 = x1 - (a / b)* y1;
 d = d1;
 x = x1;
 y = y1;

 // 由模线性方程性质可知 
 // 如果a, b互质， 那么对于模线性方程 ax + by = c 有唯一解， 
 // 而该解恰好为 d = gcd(a, b) 的一组特解
 if (d == 1) 
  return (x + b) % b; // 返回模逆元 
}
```

## 7.高斯消元（不理解）

```c++
//高斯消元是一种数学计算方法，用于快速求解系数表示的线性方程组的解，它也被称为高斯-赛洛加算法，以著名的普林斯顿大学数学家高斯命名。该方法把方程组分为上三角形组和下三角形组，通过变换方程中的系数值，最终获得方程的解。
//高斯消元法用于解决线性方程组的系数矩阵形式，即通过将系数矩阵通过合并和分解，从而求解出给定线性方程组的参数值解
//解决多项式方程的算法 - 是线性代数的知识
```

```c++
//高斯消元的证明
#include<iostream>
#include<iomanip>
using namespace std;
public void gauss_jordan(double a[][],double b[],int n)//a为系数矩阵，b为常数项，n为未知数个数
{
    int i,j,k,r;
    double m,s;

    for(i=0;i<n-1;i++)
    {
        r = i;
        for(j = i+1; j < n; j++)
            if(fabs(a[j][i]) > fabs(a[r][i])) 
                r=j;
        //将第r行与第i行交换
        if(r != i) 
        {
            for(k = 0; k < n; k++) 
            {
                m = a[i][k];
                a[i][k] = a[r][k];
                a[r][k] = m;
            }
            s = b[i];
            b[i] = b[r];
            b[r] = s;
        }
        //用第i行的元素除以第i列第一个元素
        m = a[i][i];
        b[i] = b[i]/m;
        for(k = 0; k < n; k++)
            a[i][k] = a[i][k]/m;
        //化为型
        for(j=0;j<n;j++)
        {
            if(j == i) continue;
            s = a[j][i];
            for(k = 0;k<n;k++)
                a[j][k] = a[j][k] - a[i][k]*s;
            b[j] = b[j] - b[i]*s;
        }
    }
    //输出解
    for(i=0;i<n;i++)
    {
        printf("x%d = %f\n",i,b[i]);
    }
return ;
}
```

## 筛质数

```c++
//计算质数的代码(暴力根源)
#include <iostream>  
using namespace std;  
int main()  
{  
    int i, j, count;  
    cout << " 它的2到100间的质数有: \n";  
    for(i=2;i<100;i++)  
    {  
        count = 0;  
        for(j=2;j<i;j++)//判断i是否为质数，若count==0则为质数  
        {  
            if(i%j == 0)  
            {  
                count++;  
                break;  
            }  
        }  
        if(count==0)  
        {  
            cout<<i<<" ";  
        }  
    }  
    cout << "\n";  
}
```

```c++
//加上合数优化
#include<iostream>
using namespace std;

bool i_p(int x)
{
    if(x<2) return false;
    //如果一个数是合数（不是素数），那么它的最小因数一定不会超过它的平方根
    //这个属性可以用反陈述来证明。设 a 和 b 是 n 的两个因子，使得 a*b = n。如果两者都大于 √n，则 a.b > √n， * √n，这与表达式 “a * b = n” 相矛盾。（可以理解为 a 和 b 最大就 √n）
    for(int i = 2;i<=x/i;i++)
        if(x%i == 0) return false;
    return true;   
    
}


int main()
{
    int n;
    cin>>n;
    
    while(n--){
        int x;
        cin>>x;
        if(i_p(x)) puts("Yes");
        else puts("No");
    }
    return 0;
    
    
}
```

### 试除法

```c++
//代码模版
#include <iostream>
#include <vector>
using namespace std;

vector<pair<int, int>> trial_division(int n) {
    // n是要分解的整数
    // 返回一个向量，包含n的所有质因数和它们的次数
    vector<pair<int, int>> factors; // 存储质因数和次数的向量
    int i = 2; // 从2开始试除
    while ( i <= n/i) { // 只需要试到根号n就可以了
        int s = 0; // 记录i出现的次数
        while (n % i == 0) { // 如果n能被i整除，就更新n和s
            n /= i; // 用i约分n
            s++; // 增加i的次数（当前质数的指数）
        }
        if (s > 0) { // 如果s大于0，说明i是n的一个质因数，把它加入factors向量
            factors.push_back(make_pair(i, s));
        }
        i++; // 增加i，继续试除下一个数
    }
    if (n > 1) { // 如果n最后大于1，说明它本身是一个质数，也要加入factors向量(理解)
        factors.push_back(make_pair(n, 1));
    }
    return factors; // 返回factors向量
}
```

### 试除法求约数

```c++
#include<iostream>
#include<algorithm>
#include<vector>

using namespace std;

vector<int> get_divisors(int x){
    vector<int> res;
    for(int i = 1;i<=x/i;i++)
    //整除流入
        if(x%i == 0){
            //如果i是质因数 - 塞入res数组中
            res.push_back(i); 
            //除数相等的情况 就只有i了嘛
            if(i!=x/i) res.push_back(x/i);
        }
        //排个序 (可不可以用 set存储啊)
        sort(res.begin(),res.end());
        return res;
}


int main()
{
    int n;
    cin>>n;
    
    while (n -- ){
        int x;
        cin>>x;
        auto res = get_divisors(x);
        for( auto x:res) cout<<x<<" ";
        cout<<endl;
    }
    return 0;
    
}
```

```c++
#include<bits/stdc++.h>
using namespace std;

typedef long long LL;
const int N = 110, mod = 1e9+7;//题目的意思就是和mod取模的得数

//这个代码求约数是用约数个数定理12的方法。约数个数定理是：
//如果一个数可以分解为质因数的形式，如M = x^a * y^b * z^c * …，则M的约数个数 = (a+1) (b+1) (c+1)…。
//这个代码就是先把每个输入的数分解成质因数，然后用哈希表记录每个质因数的指数，最后用公式计算出所有输入的数的最小公倍数的约数个数。
int main()
{
    int n;
    cin>>n;
    //哈希表的键是质数，值是质数对应的指数
    //primes[i], i是键，primes[i]是值
    unordered_map<int,int> primes; 
    //unordered_map的迭代器是一个指针，指向这个元素，通过迭代器来取得它的值。它的键值分别是迭代器的first和second属性12。
    //例如，it->first就是键，it->second就是值。
    //当然也不一定存储键值对 - 也可以存储二者
    while (n -- ){
        int x;
        cin>>x;
        for(int i = 2;i<=x/i;i++)
            while(x%i == 0){
                x/=i;
                primes[i]++; //对应位置的质数记录 （算是记录指数）
            }
        if(x>1) primes[x]++; //只剩下一个数据 这个数据必然是质数
    }
    LL res = 1;
    for(auto p:primes) res = res*(p.second+1)%mod;
    cout<<res<<endl;
    return 0;
}
```

### 筛法

![定义](https://upload.wikimedia.org/wikipedia/commons/b/b9/Sieve_of_Eratosthenes_animation.gif)

```c++
//1.埃拉托色尼筛
埃拉托色尼筛的原理是这样的：
    首先，把所有小于等于n的自然数都列出来，从2开始，把1排除掉。
    然后，从2开始，把它的所有倍数都标记为合数，也就是不是素数的数。
    接着，找到下一个没有被标记的数，它一定是素数，然后把它的所有倍数都标记为合数。
    重复这个过程，直到没有更多的没有被标记的数为止。
    最后，所有没有被标记的数就是素数

 /*质数筛法是一种经典的求解质数的方法，它利用筛过程来获得所有质数。这个算法强调效率，通常使用比较基本的操作，来求解比较大的质数。 筛法的原理是：从2开始，把2的倍数筛掉，然后3，4，5，把3的倍数筛掉，以此类推。最后剩下的就是质数。 如何实现质数筛法：  1. 首先设置一个长度为n的数组，并将数组初始化，其数字依次为2, 3, …, n。   
2. 从2开始遍历数组中的元素，依次筛除其2、3、4、… 倍数的数，即将数组中对应元素置0。  3. 筛完后，把数组中非0元素存入一个新数组中，这些非0元素即为质数。 
```

```c++
#include<iostream>
using namespace std;

const int N = 1e6+10;

int primes[N],cnt;
bool st[N];

void get_primes(int n){
    for(int i = 2;i<=n;i++){
        if(st[i]) continue;
        //1.primes[cnt++] = i;的作用是把i这个素数存入primes数组中
        //  并且把cnt加一，表示素数的个数增加了一个。
        primes[cnt++] = i;
        //2.;这个循环的作用是把i的所有倍数都标记为合数，也就是非素数。
        //  这样，当i增加时，就可以跳过已经被标记为合数的数字，只考虑没有被标记的数字，因为它们可能是素数。
        for(int j = i+i;j<=n;j+=i) st[j] = true;
    }
}


int main()
{
    int n;
    cin>>n;
    get_primes(n);
    cout<<cnt<<endl;
    return 0;
    
}
```

```c++
//2.线性筛法（埃拉托色尼筛优化版）
#include <iostream>
#include <algorithm>

using namespace std;

const int N= 1000010;

int primes[N], cnt;
bool st[N];

void get_primes(int n)
{
    for (int i = 2; i <= n; i ++ )
    {
        //如果i没有被标记为合数，就把它加入到primes数组中，并且把cnt加一。cnt是用来记录素数的个数的变量。这样可以把所有的素数都存储起来，方便后面的使用。
        if (!st[i]) primes[cnt ++ ] = i;
        for (int j = 0; primes[j] <= n / i; j ++ )
        {
            //primes[j] * i这个数标记为合数
            st[primes[j] * i] = true;
            //如果i能被primes[j]整除，就跳出循环，这样可以避免重复地标记一些合数。
            if (i % primes[j] == 0) break;
        }
    }
}

int main()
{
    int n;
    cin >> n;

    get_primes(n);

    cout << cnt << endl;

    return 0;
}

```

## 约数

```c++
//约数
约数是一个数据的因子，可以用这个约数凑成我们的这个数据
//试除法
  测试除法是一种数学操作，用来衡量和检查两个数字的除法运算结果是否正确。它通常包括算术问题、实数求余以及其他形式的除法运算。
//代码
#include <iostream>
#include <math.h>
using namespace std;

//试除法，求约数
void FindDivisor(int n)
{
    for (int i = 2; i <= sqrt(n); i++)
    {
        while (n % i == 0)
        {
            cout << i << "  ";
            n /= i;
        }
    }
    if (n > 1) 
    {
        cout << n;
    }
    cout << endl;
}

int main()
{
    int n;
    cout << "Please enter an integer:";
    cin >> n;

    cout << n << "的约数有：" ;
    FindDivisor(n);
    
    system("pause");
    return 0;
}
```

## 组合数(不完整)

```c++
#include <iostream> 
using namespace std;
int f[20][20];//定义二维数组，用来中间存储计算结果 
int cmn(int m,int n)  
{     
    int i,j;
    for(i=0;i<=m;++i)  //循环m步，从0开始 
       for(j=0;j<=n;++j)  //循环n步，也是从0开始  
        if(j==0||i==j)    //当i等于0或者i等于j时，直接赋值为1 
           f[i][j]=1;
        else 
           f[i][j]=f[i-1][j-1]+f[i-1][j];//其他的都是在上一步的计算结果上加+1，依次类推
     return f[m][n];  //返回结果 
}
int main()
{
    int m,n;
    cout<<"请输入任意m.n来得到组合数：";
    cin>>m>>n;
    int result=cmn(m,n);
    cout<<m<<"C"<<n<<"="<<result<<endl; //输出结果
    return 0;
}
```

## 欧几里得算法（求最大公约数）

![image-20230316143057971](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230316143057971.png)

```c++
/*辗转相除法，又称欧几里得算法，是一种求两个非负整数的最大公约数的算法1。最大公约数是能够同时整除两个整数的最大的正整数2。辗转相除法的基本思想是：如果a和b都能被c整除，那么a和b的余数也能被c整除。因此，可以用a和b的余数代替b，重复这个过程，直到余数为0为止。此时，a就是最大公约数3。

例如，要求18和30的最大公约数，可以这样做：

18 = 0 × 30 + 18

30 = 1 × 18 + 12

18 = 1 × 12 + 6

12 = 2 × 6 + 0

此时，余数为0，所以最大公约数是6。*/

//大除小，小除大除小的余数，这样反复直到一方变为0是这样吗
#include<iostream>
using namespace std;

int gcd(int a,int b){
    return b?gcd(b,a%b):a;
}

int main()
{
    int n;
    cin>>n;
    while(n--){
        int a,b;
        cin>>a>>b;
        cout<<gcd(a,b)<<endl;
    }
    
    return 0;
    
}
```

### 拓展：裴蜀定理

```c++
1.裴蜀定理（Bézout’s identity）又称裴蜀引理，是数论中的一个重要定理。它指出，对于任意整数 a 和 b，如果它们的最大公约数为 d，那么一定存在整数 x 和 y，使得不定方程 ax + by = d 有解。

2.换句话说，对于任意整数 a 和 b，它们的线性组合（即形如 ax + by 的整数）一定包含它们的最大公约数。此外，根据裴蜀定理还可以推出：如果不定方程 ax + by = c 有整数解，则当且仅当 c 是 a 和 b 的最大公约数的倍数时成立。

裴蜀定理在求解不定方程、计算模逆元等问题中都有重要应用。

//简单来说：就是构造一个这样的 ax + by = gcd(a,b); 只要gcd(a,b)能被我们题目的数据整除，就说明有整数解
```

```c++
假设我们要求解不定方程 3x + 5y = 11。首先，我们可以使用扩展欧几里得算法求出 3 和 5 的最大公约数以及不定方程 3x + 5y = gcd(3,5) 的一组整数解。运算结果显示，gcd(3,5) = 1，且不定方程 3x + 5y = 1 的一组整数解为 (x0,y0) = (2,-1)。//就是只要 3x + 5y = 1 的解也就是1 -  能够被原来的 11 整除 那么就有整数解

由于 11 是 gcd(3,5) 的倍数，因此原不定方程有整数解。它的一组特殊解为 (x0 * c/d, y0 * c/d) = (2 * 11/1,-1 * 11/1) = (22,-11)。此外，根据裴蜀定理，原不定方程的通解为 (22 + k * b/d,-11 - k * a/d) = (22 + k * 5,-11 - k * 3)（其中 k 为任意整数）。

因此，当 k=0 时，(x,y)=(22,-11) 是原不定方程的一组整数解；当 k=-1 时，(x,y)=(17,-8) 是原不定方程的另一组整数解；当 k=1时，(x,y)=(27,-14) 是原不定方程的第三组整数解。
   
```

### 扩展欧几里得算法(求不定方程)

![image-20230316150108768](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230316150108768.png)

```c++
//求不定方程 - 判断有无整数解是这个样子吗
//根据裴蜀定理，当且仅当 c 是 a 和 b 的最大公约数的倍数时，不定方程才有整数解。
int exgcd(int a,int b,int &x,int &y){
    if(b==0){
        x = 1,y = 0;
        return a;
    }
    int x1,y1,d;
    d = exgcd(b,a%b,x1,y1);
    x = y1,y = x1-a/b*y1; //用欧几里得构造
    return d; //这个就是被除的
}

int main()
{
    int a,b,c,x,y;
    cin>>a>>b>>c;
    int d = exgcd(a,b,x,y);
    if(c%d == 0) printf("%d %d",c/d*x,c/d*y); //能整除就是有整数解 - 无就是无
    else puts("none");
    return 0;    
}
```

## 快速幂算法

![image-20230317191357823](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230317191357823.png)

```c++
/*快速幂：
可以这样理解，将指数转化为二进制，有1就乘，没1就不乘。 是通过二进制 一次执行多次乘法来缩减算法复杂度的
快速幂算法就是通过二进制来判断哪些位需要乘，哪些位不需要乘，从而减少乘法的次数。

例如，如果要计算 7 的 13 次方，可以将 13 写成二进制的 1101，然后有：

7^13 = 7(23 + 2^2 + 2^0) = 7(23) * 7(22) * 7(20)

= (7 * 7)^4 * (7 * 7)^2 * (7 * 7)^1

= (49)^4 * (49)^2 * (49)^1

这样就只需要做三次乘法，而不是十二次。
*/
#include<iostream>
using namespace std;

typedef long long LL;

LL qmi(int a,int b,int p){
    LL res = 1%p;
    //把次数转化成二进制 - 有1就代表有一次
    while(b){
        if(b&1) res = res*a%p;
        a = a*(LL)a%p;
        b>>=1;
    }
    return res;
}


int main()
{
    int n;
    cin>>n;
    while(n--)
    {
        int a,b,c;
        cin>>a>>b>>c;
        cout<<qmi(a,b,c)<<endl;
    }
    return 0;
    
    
    
}
```

### 快速幂求逆元

```c++
//根据费马小定理
a^(p-1) ≡ 1 (mod p)，所以a * a^(p-2) ≡ 1 (mod p)，也就是说x = a^(p-2)就是a的逆元。
//就是要构造 a^(p-1) ≡ 1 (mod p) 那么有 a* a^(p-2) ≡ 1 (mod p)  a^(p-2)这个就是我们的目标值（用逆元是为了减少运算次数）


#include <iostream>
#include <algorithm>

using namespace std;

typedef long long LL;


LL qmi(int a, int b, int p)
{
    LL res = 1;
    while (b)
    {
        //mod质数 这里就是快速幂啦
        if (b & 1) res = res * a % p;
        a = a * (LL)a % p;
        b >>= 1;
    }
    return res;
}


int main()
{
    int n;
    scanf("%d", &n);
    while (n -- )
    {
        
        int a, p;
        scanf("%d%d", &a, &p);
        if (a % p == 0) puts("impossible");
        else printf("%lld\n", qmi(a, p - 2, p));
        //如果p是一个质数，那么根据费马小定理，我们有a^(p-1) ≡ 1 (mod p)
        //所以a * a^(p-2) ≡ 1 (mod p)，也就是说x = a^(p-2)就是a的逆元。
    }

    return 0;
}
```

## 组合计数*

  本质上，求组合数就是按照公式就可以求出来的。

### 线性筛法 - 求组合数

![image-20230317103427722](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230317103427722.png)

```c++
//线性筛法板子
#include <iostream>
#include <algorithm>

using namespace std;

const int N= 1000010;

int primes[N], cnt;
bool st[N];

void get_primes(int n)
{
    for (int i = 2; i <= n; i ++ )
    {
        //如果i没有被标记为合数，就把它加入到primes数组中，并且把cnt加一。cnt是用来记录素数的个数的变量。这样可以把所有的素数都存储起来，方便后面的使用。
        if (!st[i]) primes[cnt ++ ] = i;
        for (int j = 0; primes[j] <= n / i; j ++ )
        {
            //primes[j] * i这个数标记为合数
            st[primes[j] * i] = true;
            //如果i能被primes[j]整除，就跳出循环，这样可以避免重复地标记一些合数。
            if (i % primes[j] == 0) break;
        }
    }
}

int main()
{
    int n;
    cin >> n;

    get_primes(n);

    cout << cnt << endl;

    return 0;
}
```

```c++
//用这个方法 - 就是减少阶层的计算次数
//同时，组合数太大了只能用高精度的数据存储
//n！（阶层）中的p的个数 ，p是质数
int get(int n,int p){
    //n容纳p的个数有上限的
    int s = 0;
    while(n) s+=n/p,n/=p;
    return s;
}
//c中的p的个数(算质数的个数) - 算组合数中的质数个数
int getC(int n,int m,int p){
    return get(n,p) - get(m,p) - get(n-m,p);
}

//组合数公式 就是 - get/(get(n,p) - get(m,p) - get(n-m,p))

//数组乘质数？
void mul(int C[],int p,int &len){
    //高精度
    int t = 0;
    for(int i = 0;i<len;i++){
        t+=C[i]*p;
        C[i] = t%10;
        t/=10;
    }
    while(t){
        C[len++] = t%10;
        t/=10;
    }
}

int main()
{
    int C[N],len = 1,C[0]= 1;
    for(int i = 0;i<cnt;i++){
        int p = prim[i];
        int s = getC(n,m,p);
        while(s--) mul(C,p,len); //这里算的结果是吗
    }
    
}
```

### 快速幂 - 求组合数  - 乘法逆元(不太会)

![image-20230317190202981](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230317190202981.png)

```c++
//快速幂板子
#include<iostream>
using namespace std;

typedef long long LL;

LL qmi(int a,int b,int p){
    LL res = 1%p;
    //把次数转化成二进制 - 有1就代表有一次
    while(b){
        if(b&1) res = res*a%p;
        a = a*(LL)a%p;
        b>>=1;
    }
    return res;
}


int main()
{
    int n;
    cin>>n;
    while(n--)
    {
        int a,b,c;
        cin>>a>>b>>c;
        cout<<qmi(a,b,c)<<endl;
    }
    return 0;
    
    
    
}
```

```c++
//快速幂求组合数
LL qpow(LL a,int b)
{
    LL res = 1;
    while(b){
        if(b&1) res = res*a%p;
        a = a*a%p;
        b >>= 1;
    }
    return res;
}

void init()
{
    f[0] = g[0] = 1;
    for(int i = 1;i<N;i++){
        f[i] = f[i-1]*i%P;
        g[i] = g[i-1]*qpow(i,P-2)%P;
    }
    LL getC(LL n,LL m){
        return f[n]*g[m]%P*g[n-m]%P;
    }
}
```

```c++
#include<bits/stdc++.h>
using namespace std;
const int mod=1e9+7;
int n,m;
//求出对应的次方 也就是a^b - 也可以用来求乘法逆元 a^(p-2)
//乘法逆元原理： a*a^(p-2) = 1(modp) 这样就行 我们要求a的乘法逆元就是a^(p-2)
int qpow(int a,int b)
{
    int ans=1;
    while(b)
    {
        if(b&1) ans=1ll*ans*a%mod;
        a=1ll*a*a%mod;
        b>>=1;
    }
    return ans;
}
int inv(int x)
{
    return qpow(x,mod-2);
}
int main()
{
    cin>>n>>m;
    int ans=1;
    for(int i=1;i<=m;i++)
    {
        ans=1ll*ans*(n-i+1)%mod;
        ans=1ll*ans*inv(i)%mod;
    }
    cout<<ans<<endl;
    return 0;
}
```

### 递推法 - 求组合数 - 杨辉三角形

![image-20230317191900241](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230317191900241.png)

![image-20230317191910081](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230317191910081.png)

![杨辉三角形](C:\Users\Administrator\Pictures\杨辉三角形.jpg)

```c++
//杨辉三角形求解组合数
//就是 当前这个数据 等于上面的左右数据的加和 - 那么就用动态规划解决这个问题就ok了
//但是怎么构造杨辉三角形呢 - 初始化三角形的两条边把
void getC()
{
    for(int i = 0;i<N;i++) //N是上限 - 底的吗
        for(int j = 0;j<=i;j++)
            if(j == 0) C[i][j] = 1;//j是尽头(把三角形当成是二分之一的正方形)
      else C[i][j] = (C[i-1][j]+C[i-1][j-1])%mod;
}
```

```c++
#include<iostream>
using namespace std;
int main()
{
    int n;
    cin>>n;
    int a[100][100]={0};
    for(int i=1;i<=n;i++)
    {
        a[i][1]=1;
        a[i][i]=1;
    }
    for(int i=3;i<=n;i++)
    {
        for(int j=2;j<=i-1;j++)
        {
            a[i][j]=a[i-1][j-1]+a[i-1][j];
        }
    }
    for(int i=1;i<=n;i++)
    {
        for(int j=1;j<=i;j++)
        {
            cout<<a[i][j]<<" ";
        }
        cout<<endl;
    }
    return 0;
}
```

### 卢卡斯定理

![image-20230317192018240](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230317192018240.png)

![image-20230317192035378](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230317192035378.png)

```c++

```

## 容斥原理

![image-20230317192341534](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230317192341534.png)

## 高斯消元

### 求解线性方程组

![image-20230317192423405](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230317192423405.png)

## 中国剩余定理

![image-20230317193339194](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230317193339194.png)

![image-20230317193528005](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230317193528005.png)

![image-20230317193536881](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230317193536881.png)

### 中国剩余定理拓展

![image-20230317193608034](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230317193608034.png)

![image-20230317193627036](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230317193627036.png)

![image-20230317193636294](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20230317193636294.png)
