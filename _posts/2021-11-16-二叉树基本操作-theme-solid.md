---
layout: post
title: 二叉树操作设计和实现实验
categories: Data Structure
description: 二叉树基本操作
keywords: DFS, BFS, 递归
---

##### 二叉树操作设计和实现

###### 实验要求：

采用二叉树链表作为存储结构，完成二叉树的建立，先序、中序和后序以及按层次遍历的操作，求所有叶子及结点总数的操作。

 

###### 实验主要步骤：

1、分析、理解程序。

2、调试程序，设计一棵二叉树，输入完全二叉树的先序序列，用#代表虚结点（空指针），如ABD###CE##F##，建立二叉树，求出先序、中序和后序以及按层次遍历序列，求所有叶子及结点总数。

###### code

```c
#include"stdio.h"
#include"stdlib.h"
#include"string.h"
#define Max 20         //结点的最大个数
typedef struct node{
    char data;
    struct node *lchild,*rchild;
}BinTNode; //自定义二叉树的结点类型
typedef BinTNode *BinTree; 
int NodeNum,leaf;    
//NodeNum为结点数，leaf为叶子数
//==基于先序遍历算法创建二叉树====
//=====要求输入先序序列，其中加入虚结点"#"以示空指针的位置==========
BinTree CreatBinTree(void){
	BinTree T;
	char ch;
	if((ch=getchar())=='#')return NULL;
	else{
	T = (BinTNode*)malloc(sizeof(BinTNode));
	T->data=ch;
	T->lchild=CreatBinTree();
	T->rchild=CreatBinTree();
	return T;
	}
}

//========NLR 先序遍历=======
void Preorder(BinTree T)
{
	if(T!=NULL){
	printf("%c",T->data);
	Preorder(T->lchild);
	Preorder(T->rchild);
	}
}
//========LNR 中序遍历===========
 void Inorder(BinTree T)
{
 	if(T!=NULL){
	Inorder(T->lchild);
	printf("%c",T->data);
	Inorder(T->rchild);
	}
}
//=======LRN 后序遍历============
void Postorder(BinTree T)
{
	if(T!=NULL){
	Postorder(T->lchild);
	Postorder(T->rchild);
	printf("%c",T->data);
	}
}
//=====采用后序遍历求二叉树的深度、结点数及叶子数的递归算法========
int TreeDepth(BinTree T)
{
    int hl,hr,max;
	if(T!=NULL){
		hl=TreeDepth(T->lchild);
		hr=TreeDepth(T->rchild);
		max=hl>hr? hl:hr;
		NodeNum++;
		if(hl==0&&hr==0) leaf=leaf+1;
		
		return max+1;
	}
	else return 0;
}
//====利用"先进先出"（FIFO）队列，按层次遍历二叉树(广优)==========
void Levelorder(BinTree T)
{
    int front=0,rear=1;
    BinTNode *cq[Max],*p; 
    cq[1]=T;
    while(front!=rear)      
    {
		front=(front+1)%NodeNum;
		p=cq[front];
		printf("%c",p->data);
		if(p->lchild!=NULL){
	    rear=(rear+1)%NodeNum;
		    cq[rear]=p->lchild;
		}
		if(p->rchild!=NULL){
	    rear=(rear+1)%NodeNum;
		    cq[rear]=p->rchild;
	}
    }
}
//====数叶子节点个数==========
int countleaf(BinTree T)
{
	int hl,hr;
	if(T!=NULL){
		hl=countleaf(T->lchild);
		hr=countleaf(T->rchild);
	if(hl==0&&hr==0)return 1;
	else return hl+hr;
	}
	else return 0;
}

//=========主函数=================
void main()
{
    BinTree root;
	char i;
    int depth;
    printf("\n");
printf("Creat Bin_Tree； Input preorder:"); 
//输入完全二叉树的先序序列，
// 用#代表虚结点，如ABD###CE##F##
root=CreatBinTree();      
//创建二叉树，返回根结点
	Preorder(root);
    do {//从菜单中选择遍历方式，输入序号。
		printf("\t********select *****\n");
		printf("\t1: Preorder Traversal\n");    
		printf("\t2: Iorder Traversal\n");
		printf("\t3: Postorder traversal\n");
		printf("\t4: PostTreeDepth,Node number,Leaf number\n");
		printf("\t5: Level Depth\n"); 
//按层次遍历之前，先选择4，求出该树的结点数。
		printf("\t0: Exit\n");
printf("\t**************\n");
		fflush(stdin);
		scanf("%c",&i);  
//输入菜单序号（0-5）
		switch (i-'0'){
	case 1: printf("Print Bin_tree Preorder: ");
		Preorder(root);break;//先序遍历
case 2: printf("Print Bin_Tree Inorder: ");
		Inorder(root);break;//中序遍历
case 3:printf("Print Bin_Tree Postorder: ");
Postorder(root); break;//后序遍历
	case 4:depth=TreeDepth(root);
 //求树的深度及叶子数
printf("BinTree Depth=%d  BinTree Node number=%d",depth,NodeNum);
printf("  BinTree Leaf number=%d",countleaf(root));
		break;
	case 5: printf("LevePrint Bin_Tree: ");
	Levelorder(root); break;//按层次遍历
		default: exit(1);
	}
printf("\n");
    } while(i!=0);
}  

```

###### 心得体会

**①**二叉树的前序，中序，后序遍历算法都由递归访问左右子树实现，区别只在于访问节点数据的时机不同。

**②**后序遍历树求二叉树深度由递归算法实现，只需层层返回当前所在高度即可。出栈时，借全局变量的递增可以实现计数。此处注意栈的思想。

**③**按层次遍历二叉树时，使用指针队列，按层次填入地址，再按层次遍历输出即可。