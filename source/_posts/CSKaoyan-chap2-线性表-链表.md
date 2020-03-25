---
title: CSKaoyan-chap2-线性表-链表
author: bellick
copyright: true
top: 0
date: 2019-04-23 14:19:55
categories:
- Notebook
- CSKaoyan
tags:
- CSKaoyan
mathjax:
---
# 链表
### 1. 递归算法，删除不带头节点的单链表中所有值为x的点
```
void recursion_delete_X(LinkList L,int x){
    LinkNode * p;//指向待删除节点
    if(L==NULL){
        return;
    }
    if(L->data==x){//如果L指向的节点需要删除，则删除，并将L向后移动，重复执行
        p==L;
        L = L->next;
        free(p);
        recursion_delete_X(L,x);
    }else{
        recursion_delete_X(L->next,x);//L不需要删除，重复执行后面的过程
    }
}
```
### 2 . 带有头结点的单链表，删除所有值满足特定条件的节点（eg.等于x），并释放空间
1. 依次扫描，符合条件则删除
2. 依次扫描，用尾插法将需要保留的节点放入新链表，
```
void delect_x(LinkList L, int x){
    LinkNode* pre = L;
    LinkNode* p = L->next;
    LinkNode* q;//需要删除的点
    while(p!=NULL){
        if(p->data==x){
            q = p;
            p = p->next;
            pre->next = p;
            free(q);
        }else{
            q = p;
            p = p->next;
        }
    }
}
```
###3.带头结点单链表，反向输出每个节点的值
```
void reverse_print(LinkList L){
    if(L->next!=NULL){
        reverse_print(L->next);
    }
    printf("%d",L->data); 
}
```
### 4.带头节点单链表，删除一个最小值节点（最小值唯一）
```
void delete_Min(LinkList L){
    LinkNode *p = L->next,*prev = L;
    LinkNode *prevMin = prev,*Min = p;
    while(p!=NULL){
        if(p->data<Min->data){
            prevMin = prev;
            Min = p;
        }
        prev = p;
        p = p->next;
    }
    prevMin->next = Min->next;
    free(Min);
}
```
### 5 链表翻转
```
void reverse(LinkList L){
    LinkNode* a,*b,*c;//c用来暂存b的后继节点
    a = L->next;
    b = a->next;
    L->next->next = NULL//这个很关键
    while(b!=NULL){
        c = b->next;
        b->next = b;
        b = c;
        a = b;
    }
}
```
### 6. 带头结点的单链表，使之递增有序
```
//直接插入法
void Sort(LinkList L){
    LinkNode *p;//指向当前节点
    p = L->next;
    LinkNode *pre;//指向需要插入位置的前一个节点
    LinkNode *r;//保存p后的节点指针，以保证不断链
    while(!p){
        r = p->next;
        pre = L;
        while(pre->next!=NULL&&pre->next->data< p->data){
            pre = pre->next;
        }
        p->next = pre->next;
        pre->next = p;
        p = r;
    }
}
```

### 8. 找两个链表的公共节点
先比较两个表长，计算出长度差dist
从长表的第dist个节点和短表的第一个节点同时开始遍历，直到找到第一个相同节点，返回这个节点及之后的所有节点构成的链表

```
LinkList search_1st_commmon(LinkList L1,LinkList L2){
    int len1 = Length(L1);
    int len2 = Length(L2);
    LinkList longList , shortList;
    int dist;
    if(len1>len2){
        longList = L1->next;
        shortList = L2->next;
        dist = len1-len2;
    }else{
        longList = L2->next;
        shortList = L1->next;
        dist = len2-len1;
    }
    while(dist--){
        longList = longList->next;//表长的链表先便利到第dist个节点，然后同步
    }
    while(longList!=NULL){
        if(longList == shortList){//节点相同
            return longList;
        }else{
            longList = longList->next;
            shortList = shortList->next;
        }
    }
    return NULL;
}
```
### 10. 将一个带头节点的单链表A分解为两个带头节点大单链表A，B，是的A中含有原表中序号为奇的元素，B含有序号为偶的元素。
```
//扫描La，将偶数节点加入到Lb中并从La删除
void separate_odd_even(LinkList La,LinkList Lb){
    LinkNode *p = La->next;
    LinkNode *q = Lb ;
    while(!p){
        if(p->next!=NULL){//当前节点有后继
            q->next = p->next;
            q = q->next;
            p->next = p->next->next;
            p = p->next;
        }
    }
    p->next = NULL;
} 
```
### 12 递增有序，删除相同
```
//递增有序，删除相同
void delect_same(LinkList L){
    LinkNode * p;//工作节点
    LinkNode * prev;//前一节点
    p = L->next->next;
    prev = L->next;
    while(p!=NULL){
        if(p->data==prev->data){
  
            p = p->next->next ;
        }else{
            prev = p;
            p = p->next;
        }
    }
}
```
### 21 查找倒数第k个节点
设置两个指针，现将p移动到第k个节点
再同时移动p,q，p到表尾时，q的位置便是倒数第k个。
```
//查找倒数第k个节点
int find_last_kth(LinkList L,int k){
    //p，q指向第一个节点
    LinkNode *p = L->next;
    LinkNode *q = L->next;
    int count = 0;
    while(p!=NULL){
        if(count<k)count++;
        else{//count >= k 时，q和p同步移动
            q = q->next;
        }
        p = p->next;
        if(count<k){
            return 0;
        }else{
            printf("%d",q->data);
            return 1;
        }
    }
}
```
