---
title: CSKaoyan-chap2线性表-顺序表
author: bellick
copyright: true
top: 0
date: 2019-04-23 14:15:01
categories:
- Notebook
- CSKaoyan
tags:
- CSKaoyan
mathjax:
---
# 顺序表
###1. 顺序表删除所有值为x的元素
##### 基本思想
1. 用k记录要删除的元素的个数，并将扫描到的不要删除的元素向前移动k
2. 用k记录不要删除的元素的个数，并将不要删除的元素放到k位置上

```
//1
void delete_x(Sqlist *L,int x){
  int k = 0;
  for (int i = 0;i<L.length;i++){
    if (L.data[i]==k){//要删除
      k++;
    }else{
      L.data[i-k] = L.data[i];
    }
  }
  L.length = L.length-k;
} 
//2
void delete_x_2(Sqlist *L,int x){
  int k = 0;
  for (int i= 0;i<L.length;i++){
    if(L.data[i]!=k){//不要删除
       L.data[k] = L.data[i];
    } 
  }
}

```
### 2.删除在给定值s与t之间的值
方法同上，注意错误处理
```
bool delete_s_t(Sqlist *L,int s,int t){
  int i,k = 0;
  if(L.length==0||s>=t)return false;
  for(i = 0;i<L.length;i++){
    if(L.data[i]>=s&& L.data[i]<=t){
        k++;
    }else{
        L.data[i-k] = L.data[i];
    }
  }
  L.length -= k;
  return true;
}
```
###3. 有序表中删除所有重复元素
1. 插入排序的原理，如果和前面非重复有序表的最后一个元素L.data[i]不同，则将此元素插入到L.data[i]后,并将i++
2. 用k记录所有需要删除的元素的个数，用temp 记录前面非重复有序表的最后一个元素。
```
//1
bool delete_same1(Sqlist *L){
    if(L.length == 0){
        reutrn false;
    }
    int i ,j;
    for (i = 0;j<length;j++){
        if(L.data[i]!= L.data[j]){
            L.data[++i] = L.data[j];
        }
    }
    L.length = i+1;
    return true;
}
//2

bool delete_same2(Sqlist *L){
    if(L.length ==0 ){
        return false;
    }
    int temp = 0;
    int k = 0;
    for (int i = 0;i<L.length;i++){
        if(L.data[i]==L.data[temp]){//和最后一位不等
            L.data[i-k] = L.data[i];
            temp++;
        }else{
            k++;
        }
    }
    L.length -=k;
    return true;
}
```
如果这个表示无序表，复杂度就不是O(n)了
要想让复杂度为O(n)，就要考虑使用hash表
### 4 合并有序顺序表
```
void MergeList(SqList La, SqList Lb, SqList &Lc){
    int *pa = La.elem;
    int *pb = Lb.elem;
    int *pc = Lc->elem;
    Lc.listsize = Lc.length = La.length + Lb.length;
    if(!Lc->elem)return 0 ;
    int *pa_last = La.elem + La.length - 1;
    int *pb_last = Lb.elem + Lb.length - 1;
    while((pa<=pa_last && pb<=pb_last)){
        if(*pa<*pb) *pc++ = *pa++;
        else *pc++ = *pb++;
    }
    while(pa<=pa_last)*pc++ = *pa++;
    while(pb<=pb_last)*pc++ = *pb++;
}
```
### 5.相邻数组互换 (a1,,,,am,b1,,,bn）-->（b1,,,bn,a1,,,am）
首先对全部元素原地逆置，再对前n个元素和后m个元素分别逆置
```
void Reverse(DataType A[],int left,int right,int size){
    if(left>=right||right>=size){
        return;
    }
    int mid = (left+right)/2;
    for (int i = 0;i<=mid;i++){
        DataType temp = A[left+i];
        A[left+i] = A[right-i];
        A[right-i] = temp;
    }
}
void Exchange(DataType A[],DataType B[],int m,int n,int size){
    Reverse(A,0,m+n-1,size);
    Reverse(A,0,n-1,size);
    Reverse(A,n,m+n-1,size);
}
```
### 6. 递增有序表，查找某元素，找到则和后继元素互换，找不到则将此元素插入使仍然有序，时间最少
用折半查找
```
void SearchExchangeInsert(ElemType A[],ElemType x){
    int low = 0;
    int high = n-1;
    int mid;
    while(low<=high){//让high最后停留在待插入位置的前驱
        mid = (low+high)/2;
        if(A[mid]==x)break;
        else if(A[mid]<x)low = mid+1;
        else high = mid -1;
    }
    if(A[mid]==x&&mid!=n){//找到了
        DataType t = A[mid];
        A[mid] = A[mid+1];
        A[mid+1] = t;
    }
    if(low>high){//没找到，此时high停留在待插入位置的前面
        int i;
        for (i=n-1;i>high;i--){
            A[i+1] = A[i];
        }
        A[i+1] = x;
    }
}
```
### 7. 数组循环左移p个位置
(a0,a1,,,ap,,,,an)-->(ap,ap+1,,,,an,a0,a1,,,ap-1)
相当于第5 题
(a0,,,ap-1),(ap,,,,an)互换
```
void Reverse(int R[],int from,int to){
    //将from 到to之间的元素倒置
    int i,temp;
    for (i = 0;i<(to-from)/2;i++){
        temp = R[from+i];
        R[from+i] = R[to-i];
        R[to-i] = temp;
    }
}
void conReverse(int R[],int n,int p){
    Reverse(R,0,p-1);
    Reverse(R,p,n-1);
    Reverse(R,0,n-1);
}
```
### 8 .求两个等长升序序列的中位数
(此题中中位数的定义为第L/2向上取整个位置的元素。即中间或中间偏左位置)
基本设计思想：
找出各自的中位数 a ，b
1. 若a=b，则a或b即为要求中位数。
2. 若a<b，则舍弃A中较小的一半，同时舍弃B中较大的一半，要求两此舍弃的长度相等
3. 若a>b，则舍弃A中较大的一般，同时舍弃B中较小的一半，要求两次舍弃的长度相等
在保留的两个升序序列中，重复1），2），3），直到两个序列中均只含有一个元素为止，较小着为所求中位数。
```
int M_search(int A[],int B[],int n){
    int s1 = 0,d1 = n-1; //A的首位，末尾
    int s2 = 0,d2 = n-1; //B的首位，末位
    int m1,m2; // A,B的中位数
    while(s1!=d1||s2!=d2){
        m1 = (s1+d1)/2;
        m2 = (s2+d2)/2;
        if(A[m1]==B[m2])return A[m1];
        if(A[m1]<B[m2]){
            if((s1+d1)%2){//长度为奇数
                s1 = m1;//舍弃A中间点以前的数
                d2 = m2;//舍弃B中间点以后的书数
            }else{
                s1 = m1+1;
                d2 = m2;
            }
        }else{
            if((s2+d2)%2==0){
                d1 = m1;
                s2 = m2;
            }else{
                d1 = m1;
                s2 = m2+1;
            }
        }
    }
    return A[s1]<B[s1]?A[s1]:B[s2];
}
```
### 找出主元素（某元素的个数超过数组长度的一半）
主元素的特点： 如果一个元素的个数一定超过后面不等于它的元素的个数
将第一个遇到的整数num保存在c中，记录num出现的次数为1， 若遇到的下一个元素仍是num，则计数加1，若不等于num，则计数减1。当计数等于0时，将遇到的下一个元素保存到c中，计数重新记为1，开始新一轮计数，直到扫描完所有的元素
```
int majority(int A[],int n){
    int count = 1;//count初始化为1
    int c = A[0];
    int i;
    for (i = 1;i<n;i++){//从第二个元素开始操作
        if(A[i]==c){//遇到的等于c
            count++;
        }else{//遇到的不等于c
            if(count==0){
                c = A[i];
                count = 1;
            }else{
                count--;
            }
        }
    }
    if(count>0){
        for(i = count = 0;i<n;i++){
            if(A[i]==c){
                count++;
            }
        }
        if(count>n/2){
            return c;
        }else{
            return -1;
        }
    }
}
```
