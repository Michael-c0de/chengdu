---
layout: post
title: 链表基本操作
categories: LeetCode
description: 力扣刷题笔记1
keywords: 链表
---

# 1.链表逆转

## 源码

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */


struct ListNode* reverseList(struct ListNode* head){
    struct ListNode* node=head;
    struct ListNode* p1,*p2,*p3;
    p1=node;
    if(p1==NULL)return p1;
    p2=p1->next;
    if(p2!=NULL)    p3=p2->next;
    p1->next=NULL;
    while(p2!=NULL){
        p2->next=p1;
        p1=p2;
        p2=p3;
        if(p3!=NULL)    p3=p3->next;
    }
    return p1;
}
```

## 解题思路

将链表中每个节点的指针方向逐个逆转即可。

p1、p2、p3指针分别指向正向链表中连续的三个节点，然后让p2指向p1。整体向后移一位，如此循环。while循环边界判断条件是p2不为空。

## 运行截图

![test1]([test1.PNG (1010×182) (michael-c0de.github.io)](https://michael-c0de.github.io/chengdu/images/upload2/test1.PNG))

# 2.整数相加

## 源码

 

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
typedef struct ListNode* List;
List getPre( List list, List node );
List ReverseList(  );

struct ListNode* addTwoNumbers(List list1, List list2){
    list1=ReverseList(list1);
    list2=ReverseList(list2);
    List p=list1;
    List q=list2;
    while(p!=NULL && q!=NULL){
        p->val += q->val;
        p=p->next;
        q=q->next;
    }
    if(p==NULL){
        getPre(list1,p)->next=q;
    }
    // 进位
    p=list1;
    while (p!=NULL)
    {
        if(p->val>9){
            if(p->next==NULL){
                List node=(List)malloc(sizeof(struct ListNode));
                node->val=0;node->next=NULL;
                p->next=node;
            }
                p->val=p->val%10;
                p->next->val += 1;
            }
        p=p->next;
    }
    return ReverseList(list1);
}

// 返回list链表中node的前一个节点
List getPre( List list, List node ){
    while(list!=NULL && list->next!=node){
        list=list->next;
    }
    return list;
}

// 链表逆转
List ReverseList(struct ListNode* head){
    struct ListNode* node=head;
    struct ListNode* p1,*p2,*p3;
    p1=node;
    if(p1==NULL)return p1;
    p2=p1->next;
    if(p2!=NULL)    p3=p2->next;
    p1->next=NULL;
    while(p2!=NULL){
        p2->next=p1;
        p1=p2;
        p2=p3;
        if(p3!=NULL)    p3=p3->next;
    }
    return p1;
}
```



## 解题思路

①整数加法运算要求从低位加起，方便起见，先把两个链表逆转；

②从首元结点开始，逐个节点相加，将每次运算的结果存进其中一个链表的节点数据域中。

③从头开始遍历存有运算结果的链表，完成进位操作。

④逆转链表，打印在控制台上。

## 运行截图

![test2]([test1.PNG (1010×182) (michael-c0de.github.io)](https://michael-c0de.github.io/chengdu/images/upload2/test2.PNG))

# 3.删除倒序节点

## 源码

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */

typedef struct ListNode* List;
List getPre();
struct ListNode* removeNthFromEnd(List list, int i){
    List p=list;
    List q=list;
    int j=i;
    while(p!=NULL && j--){
        p=p->next;
    }

    while(p!=NULL){
        p=p->next;
        q=q->next;
    }
    List pre=getPre(list,q);
    if (pre==NULL)return q->next;
    pre->next=q->next;
    return list;
}

List getPre( List list, List node ){
    while(list!=NULL && list->next!=node){
        list=list->next;
    }
    return list;
}
```

## 解题思路

思路1：逆转链表，删除正序第i个节点，再次逆转链表并打印。

思路2：使用双指针，前后分别为p，q。使p，q间距保持为i，同步后移p，q，直到q为空。p即为倒序第i个节点，删除后打印链表。

使用思路2。

## 运行截图

![test3]([test1.PNG (1010×182) (michael-c0de.github.io)](https://michael-c0de.github.io/chengdu/images/upload2/test3.PNG))

# 4.删除重复单元

## 源码

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
typedef struct ListNode* List;

struct ListNode* deleteDuplicates(struct ListNode* list){
    
    List node=list;
    while(node!=NULL && node->next!=NULL){
    	// last节点指向相同数据的最末节点 
        List last=node; 
        while(last!=NULL && last->next!=NULL && last->val==last->next->val){
            last=last->next;
        }
        // 节点node指向与其数值相异的第一个节点，相当于它“跨越”了重复单元 
        node->next=last->next;
        node=node->next;
    }
    return list;
}
```

## 解题思路

链表升序排列，重复元素必然相邻，只要让每个节点“跨越”紧邻其后的重复元素，指向和自身不同的元素即可。

初始化last指针指向每一个节点node自身，利用last指针的后移找到相邻重复元素的最后一个元素，while循环截至条件为：

```c
last->val != last->next->val
```

最后令node指向last所指的元素即可。

## 运行截图

![test4]([test1.PNG (1010×182) (michael-c0de.github.io)](https://michael-c0de.github.io/chengdu/images/upload2/test4.PNG))

# 5.合并链表数组

## 源码

```c
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     struct ListNode *next;
 * };
 */
struct ListNode* mergeKLists(struct ListNode** lists, int listsSize){
    int i, Min, iMin, izcnt = 0,iflag = 1;
    struct ListNode* head = NULL;
    struct ListNode* node = NULL;
    

    iflag = 0;
    for(i=0;i<listsSize;i++){
        if(lists[i] != NULL){
            iflag = 1;
        }else{
            izcnt++;
        }
    }
    while(iflag){

        for(i=0;i<listsSize;i++){
            if(lists[i] != NULL){
                Min = lists[i]->val;
                iMin = i;
                break;
            }
        } 
       
        for(i=i+1;i<listsSize;i++){
            if(lists[i] != NULL){
                iMin = Min > lists[i]->val ? i : iMin;
                Min = Min > lists[i]->val ? lists[i]->val : Min;
            }
        } 
        if(head == NULL){
            head = lists[iMin];
            node = head;
        }else{
            node->next = lists[iMin];
            node = node->next;
        }
   
        lists[iMin] = lists[iMin]->next;
        if(lists[iMin] == NULL){
            izcnt++;
            if(izcnt == listsSize){
                iflag = 0;
            }
        }
    }
    return head;
}
```

## 解题思路

定义和初始化新链表头节点res，定义指针p，q分别指向两个链表的首元节点。在循环结构中逐个比较p，q节点数据域大小，使链表res的尾指针总是指向数据域较小的节点，同时使链表对应指针（p or q）后移一位。

多个链表的合并用循环结构操作即可。

## 运行截图

![test5]([test1.PNG (1010×182) (michael-c0de.github.io)](https://michael-c0de.github.io/chengdu/images/upload2/test5.PNG))

# 6.实现跳表

## 源码

```c
#include <stdio.h>
#include <stdlib.h>
#include <limits.h>

#define bool int
#define TRUE 1
#define FALSE 0

#define SKIPLIST_MAXLEVEL 32        

#define max(a, b) ((a) > (b) ? (a) : (b))
#define min(a, b) ((a) < (b) ? (a) : (b))

typedef struct SkiplistNode {
    int value;                      // 值
    struct SkiplistLevel {
        struct SkiplistNode *next;
    } level[];                      // 跳表每个节点有N层索引, 柔性数组
} SkiplistNode;

typedef struct Skiplist {
    struct SkiplistNode *head;      // 表头节点
    int length;                     // 记录跳跃表长度
    int level;                      // 记录跳跃表当前层数
} Skiplist;


// helper func
SkiplistNode* skiplistNodeCreate(int level, int value)
{
    SkiplistNode *p = (SkiplistNode *)malloc(sizeof(*p) + sizeof(struct SkiplistLevel) * level);
    p->value = value;
    return p;
}

// 跳表的创建
Skiplist* skiplistCreate() {
    Skiplist *sl = (Skiplist *)malloc(sizeof(*sl));
    sl->length = 0;
    sl->level = 1;
    sl->head = skiplistNodeCreate(SKIPLIST_MAXLEVEL, INT_MIN);
    for (int i = 0; i < SKIPLIST_MAXLEVEL; ++i) {
        sl->head->level[i].next = NULL;
    }
    return sl;
}

// 跳表的查找, O(logN)
bool skiplistSearch(Skiplist* obj, int target) {
    SkiplistNode *p = obj->head;
    int levelIdx = obj->level - 1;
    
    for (int i = levelIdx; i >= 0; --i) {
        // 如果第i层节点值小于target, 沿当前层继续查找
        while (p->level[i].next && p->level[i].next->value < target) {
            p = p->level[i].next;
        }
        // 第i层未找到该节点, 或者节点值已大于target, 沿着下一层查找
        if (p->level[i].next == NULL || p->level[i].next->value > target) {
            continue;
        }
        return TRUE;
    }
    return FALSE;
}

// P(层数为n) = (1/2) ^ n
int GetSkipNodeRandomLevel()
{
    int level = 1;
    while (rand() & 0x1) {
        ++level;
    }
    return min(level,SKIPLIST_MAXLEVEL); 
}

// 跳表的插入 O(logN)
// 要点: 记忆待插入节点的所有前继节点的值，并更新最大层数
void skiplistAdd(Skiplist* obj, int num) {
    SkiplistNode *p = obj->head;
    int levelIdx = obj->level - 1;
    struct SkiplistNode *preNodes[SKIPLIST_MAXLEVEL]; // 记忆待插入节点的所有前继节点的值
    for (int i = obj->level; i < SKIPLIST_MAXLEVEL; ++i) {
        preNodes[i] = obj->head;
    }

    // 记忆待插入节点的所有前继节点的值，便于后续插入操作
    for (int i = levelIdx; i >= 0; --i) {
        // 如果第i层节点值小于target, 沿当前层继续查找插入的位置
        while( p->level[i].next && p->level[i].next->value < num) {
            p = p->level[i].next;
        }
        preNodes[i] = p;
    }

    int newLevel = GetSkipNodeRandomLevel();
    struct SkiplistNode *newNode = skiplistNodeCreate(newLevel, num);
    for (int i = 0; i < newLevel; ++i) {
        newNode->level[i].next = preNodes[i]->level[i].next;
        preNodes[i]->level[i].next = newNode;
    }
    obj->level = max(obj->level, newLevel);         // 注意更新跳跃表当前层数
    ++obj->length;
}

void skiplistNodeDelete(Skiplist *obj, SkiplistNode *cur, SkiplistNode **preNodes)
{
    for (int i = 0; i < obj->level; ++i) {
        if (preNodes[i]->level[i].next == cur) { // 只有相等，才能从当前层的链表中删除节点!
            preNodes[i]->level[i].next = cur->level[i].next;
        }
    }
    // 如果删除的节点是层数最大的，那么可能需要更新跳表长度
    for (int i = obj->level - 1; i >= 1; --i) {
        if (obj->head->level[i].next != NULL) {
            break;
        }
        --obj->level;
    }
    --obj->length;
    // 释放被删除节点空间
    free(cur);
}

// 跳跃表删除操作 O(logN)
// 要点: 找到待删除节点的前一个节点, 注意更新最大层数
bool skiplistErase(Skiplist* obj, int num) {
    SkiplistNode *p = obj->head;
    int levelIdx = obj->level - 1;
    struct SkiplistNode *preNodes[SKIPLIST_MAXLEVEL]; // 记忆待删除节点的所有前继节点的值
    for (int i = levelIdx; i >= 0; --i) {
        // 如果第i层节点值小于num, 沿当前层继续查找
        while (p->level[i].next && p->level[i].next->value < num) {
            p = p->level[i].next;
        }
        preNodes[i] = p;
    }

    p = p->level[0].next;
    if (p && p->value == num) {
        skiplistNodeDelete(obj, p, preNodes);
        return TRUE;
    }
    return FALSE;
}

void skiplistFree(Skiplist* obj) {
    SkiplistNode *cur = obj->head->level[0].next;
    SkiplistNode *d;
    while(cur) {
        d = cur;
        cur = cur->level[0].next;
        free(d);
    }
    free(obj->head);   
    free(obj);
}

/**
 * Your Skiplist struct will be instantiated and called as such:
 * Skiplist* obj = skiplistCreate();
 * bool param_1 = skiplistSearch(obj, target);
 
 * skiplistAdd(obj, num);
 
 * bool param_3 = skiplistErase(obj, num);
 
 * skiplistFree(obj);
*/
```

## 运行截图

 ![test6]([test1.PNG (1010×182) (michael-c0de.github.io)](https://michael-c0de.github.io/chengdu/images/upload2/test6.PNG))

下载：

 [code.rar](https://michael-c0de.github.io/chengdu/images/upload2/code.rar) 
