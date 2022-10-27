# 資料結構與演算法 習題解答 奇數題
## 第一章 基本概念
---
---
### 5. Ackerman’s 函式 A(m, n) 的定義如下： <br> $\qquad \quad A(m,n)=\begin{cases}\qquad \quad n+1 \qquad \qquad \quad \ \ \text{ if } \ \ m=0 \\\qquad A(m-1,1) \qquad \quad \ \ \ \text{ if } \ \ n=0 \\A(m-1,A(m,n-1)) \quad	 \text{ otherwise }\end{cases}$ <br> 這個函式在 $m$ 和 $n$ 的值還不是很大的時候，就已成長的非常快。寫一個遞迴程序和非遞迴程序計算它。

:::spoiler 遞迴版本:

```javascript=
int Ackerman(int m,int n)
{   if (m==0) return n+1;
    else if (n==0) Ackerman(m-1,1);
    else Ackerman(m-1, Ackerman(m,n-1));
}

int main(void)
{   int m, n;
    while(cin>>m>>n)
    {   cout << "Answer = " << Ackerman(m,n);
        cout << endl << endl;
    } 
}
```

/* ---- 執行結果:

1 10
Answer = 12

3 10
Answer = 8189

2 15
Answer = 33

4 5
(等候逾時)

---- */

![](https://i.imgur.com/Y6ofI0k.png)

:::

::: spoiler 迴圈版本：

```javascript=
#include<iostream>
#include<time.h>
using namespace std;
int ack_nonrecursive(int m, int n)
{   int value[500000];
    int Size = 0;
    for (;;)
    {   if (m == 0)
        {   n++;
            if (Size-- == 0) break;
            m = value[Size];
            continue;
        }
        if (n == 0)
        {   m--;
            n = 1;
            continue;
        }
        int index = Size++;
        value[index] = m - 1;
        n--;
    }
    return n;
}

int main()
{   int m, n;
    cin>>m>>n;
    double START,END;
    for(int i=1; i<=m; i++)
        for(int j=1; j<=n; j++)
        {   cout<<"mi="<<i<<" nj="<<j<<"Value=";
            START = clock();
            cout<<ack_nonrecursive(i, j)<<endl;
            END = clock();
            cout<<"Execution Time: "<<(END - START) / 
            CLOCKS_PER_SEC<<"s"<<endl<<endl;
        }
}
```
/* ---- 執行結果:

![](https://i.imgur.com/na6KtK8.png) ![](https://i.imgur.com/k57YZJ8.png)

:::

:::spoiler 遞迴與迴圈執行時間比較 (CPU: RAM:  OS: Windows 10)：

| 遞迴版執行時間 | 迴圈版執行時間 | 
| -------- | -------- | 
| ![](https://i.imgur.com/VatBWLg.png)     | ![](https://i.imgur.com/15FH7n1.png)     | 
:::

---
### 7. 假如 S 是一個含有 $n$ 個元素的集合，則 S 的 power set 就是「所有 S 可能的子集合的集合」，例如：假如 S = {a, b, c} ，則 powerset(S) = {∅, {a}, {b}, {c}, {a, b}, {a, c}, {b, c}, {a, b, c}}，寫一個遞迴程式來計算 powerset(S)。

:::spoiler 請見下表：

&emsp;&emsp;&ensp;*S* &emsp; &emsp; &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp; powerset(*S*)中元素

|{a, b, c}|{}|{c}|{b}|{b, c}|{a}|{a, c}|{a, b}|{a, b, c}|
| -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- | -------- |
|0/1 Bit|000|001|010|011|100|101|110|111|

由上表可知：求 S = \{a, b, c} 的 power set 相當於求 000 \~ 111 的所有 bit patterns!
可設一 0/1 Bit 陣列 (\|BIT\| = \|S\|) 與 S 的字元相互對應，recursively 求 Bit 的所有0/1 patterns: powerset(Bit[k,n]) = {Bit[k]=0 \|\| powerest(Bit[k+1,n])} ∪ {Bit[k]=1 \|\| powerest(Bit[k+1,n])} 其中 || 表示 0/1 patterns 的串接。呼叫 powerset(Bit[n-1,n]) 時即為 boundary condition 可依 Bit[0:n-1] 的0/1 pattern 印出對應的 subset of S.

```cpp=
# include <stdio.h>
# include <stdlib.h>

char a[10] = { "abcdefgh" };
char b[10];

int Bit[10];
int cnt = 0;
int solCnt;

void printSubset(int Bit [], int n)
{   int i, index = 0;
    for (i=0; i<n; i++)
        if (Bit[i]) b[index++] = a[i];
    b[index] = '\0';
    printf("{%s}", b);
    if (++cnt<solCnt) printf(", ");
}

void powerSet(int k, int n)
{   if (k == n) printSubset(Bit, n);
    else
    {   Bit[k] = 0;
        powerSet(k+1, n);
        Bit[k] = 1;
        powerSet(k+1, n);
    }

}

int main()
{   int i, n;
    printf("Please input the size (n) of the set: n = ");
    while (scanf(" %d", &n), solCnt = pow(2, n),  n!=0)
    {   printf("{");
        powerSet(0, n);
        printf("}\nSize of Powerset = %d.\n\nContinue (0 for exit) n = ", cnt);
        cnt = 0;
    }
    return 1;
}
```

/* ---- 執行結果:


![](https://i.imgur.com/UVVyKcI.png)

---- */

:::

---
### 9. 比較 $n^3$ 和 $2^n$ 這兩個函式，計算出哪個 $n$ 值會使後者大於前者。

:::spoiler

當 $n$ = 10 時,  $n^3$ =1000 < $2^n$ = 1024; 當 $n$ > 10 時, $n^3$ < $2^n$。

:::
