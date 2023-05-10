# test_5_10
#include <stdio.h>
//二叉树的顺序存储结构
#define MaxSize 100
struct TreeNode{
	ElemType value;
	bool IsEmpty;
};
void testTree(){
	TreeNode T[MaxSize];
	for(int i=0;i<MaxSize;i++)
	{
		T[i].IsEmpty = true;
	}
}
//链式存储结构
typedef struct BiTNode{
	ElemType data;
	struct BiTNode *lchild,*rchild;
}BiTNode,*BiTree;
void testTree(){
	//定义一个二叉树
	BiTree root = NULL;
	//插入根结点
	root = (BiTNode*)malloc(sizeof(BiTNode));
	root->data = 1;
	root->lchild = NULL;
	root->rchild = NULL;
	//插入新结点
	BiTNode *p = (BiTNode*)malloc(sizeof(BiTNode));
	p->data = 2;
	p->lchild = NULL;
	p->rchild = NULL;
	root->lchild = p;
}//二叉树的层序遍历
//链式队列结点
typedef struct LinkNode{
	BiTNode *data;
	struct LinkNode *next;
	struct LinkNode *front,*rear;
}LinkNode,*LinkQueue;
//二叉树结点
typedef struct BiTNode{
	char data;
	struct BiTNode *lchild,*rchild;
}BiTNode,*BiTree;
void LevelOrder(BiTree T){
	LinkQueue Q;
	InitQueue(Q);
	BiTree p;
	EnQueue(Q,T);
	while(!IsEmpty(Q))
	{
		DeQueue(Q,p);
		visit(p);
		if(p->lchild != NULL)
			EnQueue(Q,p->lchild);
		if(p->rchild != NULL)
			EnQueue(Q,p->rchild);
	}
}
//二叉树的先/中/后序遍历
void PreOrder(BiTree T){
	if(T != NULL)
	{
		visit(T);
		PreOrder(T->lchild);
		PreOrder(T->rchild);
	}
}
void InOrder(BiTree T){
	if(T != NULL)
	{
		InOrder(T->lchild);
		visit(T);
		InOrder(T->rchild);
	}
}
void PostOrder(BiTree T){
	if(T != NULL)
	{
		PostOrder(T->lchild);
	    PostOrder(T->rchild);
		visit(T);
	}
}
//中序线索化二叉树
typedef struct ThreadNode{
	ElemType data;
	struct ThreadNode *lchild,*rchild;
	int ltag,rtag;
}ThreadNode,*ThreadTree;
ThreadTree Pre = NULL;
void InThread(ThreadTree T){
	if(T != NULL)
	{
		InThread(T->lchild);
		visit(T);
		InThread(T->rchild);
	}
}
void visit(ThreadNode *p){
	if(p->lchild == NULL)
	{
		p->lchild = Pre;
		p->ltag = 1;
	}
	if(Pre != NULL && Pre->rchild == NULL)
	{
		Pre->rchild = p;
		Pre->rtag = 1;
	}
	Pre = p;
}
void InThread(ThreadTree p,ThreadTree &Pre){
	if(p != NULL)
	{
		InThread(p->lchild,Pre);
		if(p->lchild == NULL)
	    {
		    p->lchild = Pre;
		    p->ltag = 1;
	    }
	    if(Pre != NULL && Pre->rchild == NULL)
	    {
		    Pre->rchild = p;
		    Pre->rtag = 1;
	    }
	    Pre = p;
		InThread(p->rchild,Pre);
	}
}
void CreateInThread(ThreadTree T){
	ThreadTree Pre = NULL;
	if(T != NULL)
	{
		InThread(T);
		if(Pre->rchild = NULL)
			Pre->rtag = 1;
	}
}
//先序遍历二叉树
void PreThread(ThreadTree p,ThreadTree &Pre){
	if(p != NULL)
	{
		if(p->lchild == NULL)
	    {
		    p->lchild = Pre;
		    p->ltag = 1;
	    }
	    if(Pre != NULL && Pre->rchild == NULL)
	    {
		    Pre->rchild = p;
		    Pre->rtag = 1;
	    }
	    Pre = p;
		PreThread(p->lchild,Pre);
		PreThread(p->rchild,Pre);
	}
}
//找到以p为根子树中第一个被中序遍历的结点
ThreadNode *Firstnode(ThreadNode *p){
	while(p->ltag == 0)
		p = p->lchild;//左 根 右
	return p;
}
//找到p结点的后继结点
ThreadNode *Nextnode(ThreadNode *p){
	if(p->rtag == 0)
		return Firstnode(p->rchild);//右子树中最左下结点
	else
		return p->rchild;
}
//对中序二叉树进行中序遍历的非递归算法
void InThread(ThreadTree T){
	for(ThreadNode *p = Firstnode(T);p != NULL;p = Nextnode(p))
		visit(p);
}
//找中序前驱
ThreadNode *Lastnode(ThreadNode *p){
	if(p->rtag == 0)
		p = p->rchild;
	return p;
}
ThreadNode *Prenode(ThreadNode *p){
	if(p->ltag == 0)
		return Lastnode(p->lchild);
	else
		return p->lchild;
}
//对中序二叉树进行逆向中序遍历
void InThread(ThreadTree T){
	for(ThreadNode *p = Lastnode(T);p != NULL;p = Prenode(p))
		visit(p);
}
//先序线索二叉树找先序后继
ThreadNode *Nextnode(ThreadNode *p){
	if(p->ltag == 0)
		return p->lchild;
	else
		return p->rchild;
}
//后续线索化二叉树找前驱
ThreadNode *Lastnode(ThreadNode *p){
	if(p->rtag == 0)
		return p->rchild;
	else 
		return p->lchild;
}
