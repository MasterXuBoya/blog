---
title: Kickstart Round A 2020
toc: true
thumbnail: /gallery/thumbnails/painting1.jpg
widgets:
  - type: profile
    position: left
    author: 博雅
    author_title: zju小硕一枚
    location: 'Hangzhou,China'
    avatar: /images/Golden_Medal_-1_Icon.svg
    gravatar: null
    avatar_rounded: false
    follow_link: 'https://github.com/MasterXuBoya'
    social_links:
      Github:
        icon: fab fa-github
        url: 'https://github.com/MasterXuBoya'
      E-Mail:
        icon: fa fa-envelope
        url: xuboya@zju.edu.cn
      Facebook:
        icon: fab fa-facebook
        url: 'https://facebook.com'
      Twitter:
        icon: fab fa-twitter
        url: 'https://twitter.com'
      RSS:
        icon: fas fa-rss
        url: /
  - type: category
    position: left
  - type: toc
    position: left
date: 2020-03-27 21:56:49
tags:
    - Kickstart
    - OJ
categories:
    - 数据结构与算法
    - Kickstart
---

## Problem 1 Allocation

### Problem
There are N houses for sale. The i-th house costs Ai dollars to buy. You have a budget of B dollars to spend.

What is the maximum number of houses you can buy?
<!--more-->
### Input
The first line of the input gives the number of test cases, T. T test cases follow. Each test case begins with a single line containing the two integers N and B. The second line contains N integers. The i-th integer is Ai, the cost of the i-th house.

### Output
For each test case, output one line containing Case #x: y, where x is the test case number (starting from 1) and y is the maximum number of houses you can buy.

### Limits
Time limit: 15 seconds per test set.
Memory limit: 1GB.
1 ≤ T ≤ 100.
1 ≤ B ≤ 105.
1 ≤ Ai ≤ 1000, for all i.

Test set 1
1 ≤ N ≤ 100.

Test set 2
1 ≤ N ≤ 105.

### 分析

贪心算法，挑选最小的买

### 代码

```C++
#include<stdio.h>
#include<iostream>
#include<algorithm>
#include<string.h>

#define maxn 100010
using namespace std;

int T,n,b;
int a[maxn];

int main(){
    int i,j,t,ans;
    cin>>T;
    for(t=1;t<=T;t++){
        cin>>n>>b;
        for(i=0;i<n;i++)
            cin>>a[i];
        
        sort(a,a+n);
        ans=0;i=0;
        while(i<n&&b>=a[i]){
            ans++;
            b-=a[i];
            i++;
        }
        printf("Case #%d: %d\n",t,ans);
    }
}
```

## Problem 2 Plates

### Problem
Dr. Patel has N stacks of plates. Each stack contains K plates. Each plate has a positive beauty value, describing how beautiful it looks.

Dr. Patel would like to take **exactly** P plates to use for dinner tonight. If he would like to take a plate in a stack, he must also take all of the plates above it in that stack as well.

Help Dr. Patel pick the P plates that would maximize the total sum of beauty values.

### Input
The first line of the input gives the number of test cases, T. T test cases follow. Each test case begins with a line containing the three integers N, K and P. Then, N lines follow. The i-th line contains K integers, describing the beauty values of each stack of plates from top to bottom.

### Output
For each test case, output one line containing Case #x: y, where x is the test case number (starting from 1) and y is the maximum total sum of beauty values that Dr. Patel could pick.

### Limits
Time limit: 20 seconds per test set.
Memory limit: 1GB.
1 ≤ T ≤ 100.
1 ≤ K ≤ 30.
1 ≤ P ≤ N * K.
The beauty values are between 1 and 100, inclusive.

Test set 1
1 ≤ N ≤ 3.

Test set 2
1 ≤ N ≤ 50.

### 分析

典型的背包问题，假设原始数据为a[N][K]数组，第i堆前j个和为s[i][j](行的前缀和).从最后一堆分析起，最后一堆可以选0个、1个、2个……K个，然后前N-1个依然是最优子问题。

伪递推方程为：f(N,P)=f(N-1,i)+s[N][i]     0≤i≤min(K,P)。

边界条件：f(0,0)=0,f(0,i)=-MAXN    i>0,f(i,0)=0   i>0.

### 代码

```C++
#include<iostream>
#include<stdio.h>
#include<string.h>
#include<stdlib.h>
#include<cstring>
#define maxn 100000000
using namespace std;

int a[60][32],s[60][32];
int w[60][1600];
int main(){
    int i,j,k,t,T,n,m,p;
    cin>>T;
    for(t=1;t<=T;t++){
        memset(a,0,sizeof a);
        cin>>n>>m>>p;
        for(i=1;i<=n;i++)
            for(j=1;j<=m;j++)
                cin>>a[i][j];
        memset(s,0,sizeof s);
        for(i=1;i<=n;i++)
            for(j=1;j<=m;j++)
                s[i][j]=s[i][j-1]+a[i][j];
        memset(w,0,sizeof w);
        for(i=1;i<=p;i++)
            w[0][i]=-maxn;
        for(i=1;i<=n;i++)
            for(j=1;j<=p;j++){
                int up=min(m,j);
                for(k=0;k<=up;k++)
                    if(w[i-1][j-k]+s[i][k]>w[i][j])
                        w[i][j]=w[i-1][j-k]+s[i][k];
        }
        printf("Case #%d: %d\n",t,w[n][p]);
    }
}
```

## Problem 3 Workout

### Problem
Tambourine has prepared a fitness program so that she can become more fit! The program is made of N sessions. During the i-th session, Tambourine will exercise for Mi minutes. The number of minutes she exercises in each session are strictly increasing.

The difficulty of her fitness program is equal to the maximum difference in the number of minutes between any two consecutive training sessions.

To make her program less difficult, Tambourine has decided to add up to K additional training sessions to her fitness program. She can add these sessions anywhere in her fitness program, and exercise any positive integer number of minutes in each of them. After the additional training session are added, the number of minutes she exercises in each session must still be strictly increasing. What is the minimum difficulty possible?

### Input
The first line of the input gives the number of test cases, T. T test cases follow. Each test case begins with a line containing the two integers N and K. The second line contains N integers, the i-th of these is Mi, the number of minutes she will exercise in the i-th session.

### Output
For each test case, output one line containing Case #x: y, where x is the test case number (starting from 1) and y is the minimum difficulty possible after up to K additional training sessions are added.

### Limits
Time limit: 20 seconds per test set.
Memory limit: 1GB.
1 ≤ T ≤ 100.
For at most 10 test cases, 2 ≤ N ≤ 105.
For all other test cases, 2 ≤ N ≤ 300.
1 ≤ Mi ≤ 109.
Mi < Mi+1 for all i.

Test set 1
K = 1.

Test set 2
1 ≤ K ≤ 105.

### 分析

求间隔最大值的最小，典型的最大最小问题，二分思想。主要想法是二分枚举间隔x，通过check函数判断x是否能实现。假设最终的答案是t，那么x< t时，checkout函数返回值为false，表示间隔到不了那么小，需要更多的K；x≥t时，check返回值为true，表示容忍的了那么大的间隔。因此该函数具有单调性，可以二分解决

### 代码

```C++
#include<stdio.h>
#include<stdlib.h>
#include<math.h>
#include<cstring>
#include<iostream>
#include<algorithm>
#include<vector>
#include<list>
#include<map>
#include<set>
#include<string>
#include<stack>
#include<queue>
#include<climits>
#include<sstream>
#define maxn 100010

using namespace std;

int n,k;
int a[maxn],d[maxn];

bool check(int x){
    int i,s=0;
    for(i=0;i<n-1;i++)
        if(d[i]%x==0) s+=d[i]/x-1;
        else s+=d[i]/x;
    if(s<=k) return true;
    return false;
}
int main(){
    int i,j,T,t;
    cin>>T;
    for(t=1;t<=T;t++){
        memset(a,0,sizeof a);
        memset(d,0,sizeof d);
        cin>>n>>k;
        for(i=0;i<n;i++)
            cin>>a[i];
        for(i=0;i<n-1;i++)
            d[i]=a[i+1]-a[i];

        int l=1,r=1e9,mid,dl=0,dr=0;
        while(l<r){
            if(dl==l&&dr==r) break;
            dl=l;dr=r;
            mid=(l+r)/2;
            bool status=check(mid);
            if(status) r=mid;
            else l=mid+1;
        }
        printf("Case #%d: %d\n",t,(l+r)/2);
    }
}

```

## Problem 4 Bundling

### Problem
Pip has N strings. Each string consists only of letters from A to Z. Pip would like to bundle their strings into groups of size K. Each string must belong to exactly one group.

The score of a group is equal to the length of the longest prefix shared by all the strings in that group. For example:
The group {RAINBOW, RANK, RANDOM, RANK} has a score of 2 (the longest prefix is 'RA').
The group {FIRE, FIREBALL, FIREFIGHTER} has a score of 4 (the longest prefix is 'FIRE').
The group {ALLOCATION, PLATE, WORKOUT, BUNDLING} has a score of 0 (the longest prefix is '').

Please help Pip bundle their strings into groups of size K, such that the sum of scores of the groups is maximized.

### Input
The first line of the input gives the number of test cases, T. T test cases follow. Each test case begins with a line containing the two integers N and K. Then, N lines follow, each containing one of Pip's strings.

### Output
For each test case, output one line containing Case #x: y, where x is the test case number (starting from 1) and y is the maximum sum of scores possible.

### Limits
Time limit: 20 seconds per test set.
Memory limit: 1GB.
1 ≤ T ≤ 100.
2 ≤ N ≤ 105.
2 ≤ K ≤ N.
K divides N.
Each of Pip's strings contain at least one character.
Each string consists only of letters from A to Z.

Test set 1
Each of Pip's strings contain at most 5 characters.

Test set 2
The total number of characters in Pip's strings across all test cases is at most 2 × 106.

### 分析

据说是转化成字典树(Trie树)，暂时不会……

---
