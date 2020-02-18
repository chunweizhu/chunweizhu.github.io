---
categories:
  - Python   #注意一定要大写
published: true
---



## 静态链表SLinkList

```C
# include <string.h>//字符串函数头文件
# include <ctype.h>//字符函数头文件
# include <malloc.h>//malloc()等
# include <limits.h>//INT_MAX等
# include <stdio.h>//标准输入输出头文件，包括EOF（=^z或F6),NULL等
# include <stdlib.h>//atio(),exit()
# include <io.h>//eof()
# include <math.h>//数学函数头文件，包括floor（），ceil（），abs（）等
# include <sys/timeb.h>//ftime()
# include <stdarg.h>//提供宏va_start,va_arg和va_end，用于存取变长参数表
//函数结果状态码
#define TRUE 1
#define FALSE 0
#define OK 1
#define ERROR 0
//#define INFEASIBLE -1 没有使用
//#define OVERFLOW -2 因为在math.h中已定义OVERFLOW 的值为3，故去掉此行
typedef int Status; //Status是函数的类型，其值是函数结果状态代码，如OK等
typedef int Boolean;//Boolean 是布尔类型，其值是 TRUE 或 FALSE
typedef int ElemType;

#define MAX_SIZE 100  //define 后面没有  ;  号

//静态链表的存储结构
typedef struct
{
	ElemType data;
	int cur;
}component, SLinkList[MAX_SIZE];

//初始化建立一个静态链表

void InitList(SLinkList L)
{
	
	L[MAX_SIZE-1].cur = 0;

	for(int i=0; i<MAX_SIZE-2; ++i)
	{
		L[i].cur = i+1;
	}
	L[MAX_SIZE-2].cur = 0;

}

//向有效区分配一个结点,相当于malloc函数
//1.我开始是认为将结点分配，并将备用链表和有有效链表都做好，但是这里只需要分配不需要将有效链表连接，返回分配的区域的数组下标即可
/*
void Malloc(SLinkList L)
{
	int i = L[0].cur;
	if(NULL != i)
	{
		L[MAX_SIZE-1].cur = i;
		L[0].cur = L[i].cur;
		L[i].cur = 0;
	}	
}
*/

int Malloc(SLinkList L)
{
	int i = L[0].cur;
	if( i!=NULL )
	{
		L[0].cur = L[i].cur;
	}
	return i;//返回数组中下标为i的单元区域被返回，可以被（有效区）链表使用;还有另外一层含义，如果i==NULL，则返回NULL

}



//将有效区不用的元素回收回备用区
void Free(SLinkList L , int k)
{//将下标为k的空闲结点回收到备用链表中，成为备用链表的第一个结点
	 L[k].cur = L[0].cur;
	 L[0].cur = k;
}

//判断链表是否为空，当为空时返回TRUE，否则返回FALSE
bool ListEmpty(SLinkList L)
{
	if (L[MAX_SIZE-1].cur == 0)
		return TRUE;
	else
		return FALSE;
}


//返回L中元素个数，求（有效）链表中元素个数
int LengthList(SLinkList L)
{
	int length = 0;
	int i = L[MAX_SIZE-1].cur;
	while(NULL != i)
	{
		i = L[i].cur;
		++length;
	}
	return length;
}


//用e返回L中第i个元素的值
bool GetElem(SLinkList L,int i,int *e)
{
	if(i > LengthList(L) || i<1)
	{
		printf("超出链表范围!\n");
		return FALSE;
	}
	
	
	int pos = L[MAX_SIZE-1].cur;  //最好用pos == MAX_SIZE-1;这样就是同步的移动了

	for(int j =1; j<i ;++j)
	{
		pos = L[pos].cur;
		
	}
	*e = L[pos].data;
}

//在静态单链表L中查找第一个值为e的元素，若找到，则返回它在L中的位序，否则返回0//是要返回数组的下标呀！

/*Status LocateElem(SLinkList L, int e,)
{
	int i = L[MAX_SIZE-1].cur;
	int pos = 1;

	for(pos; pos<=LengthLink(L); ++pos)
	{
		if(L[i].data == e)
		{
			return pos;
		}
	}
	else
	{
		return 0;
	}

}*/

//改写上面的代码，返回数组的下标而不是第几个
//在静态单链表L中查找第一个值为e的元素，若找到，则返回它在L中的位序，否则返回0
int LocateElem(SLinkList L, int e )
{
	int i = L[0].cur; //静态链表中第一个有效元素的下标在数组中是 i
	while(i && L[i].data != e) //当i下标对应的数组中保存的数据不是i时我们就继续顺着有效链表的连接 继续搜素 搜索到最后的标志是 cur中保存的为0.
	{
		i = L[i].cur;
	}
	return i;//这一句不论e存不存在数组中，我们都可以返回i，不存在时说明i已经变为0了，while因为i==0而退出，如果存在，while则是因为L[i].data == e退出，此时i就是e所在数组中位置中的下标大小
}

//清空链表中(使用了的空间)的元素，也就是把使用了的数组空间归还给备用区去管理
//我要找到备用链表中的最后一个空间的位置，然后我将其存储的cur = 0换成使用了的链表的首结点(就是第一个存放有效数字的结点，不是头结点，但是他的地址得由头结点找出)
void CLearList(SLinkList L)
{
	//遍历找到备用区的最后一块区域
	int i = L[0].cur;
	while(L[i].cur != 0)
	{
		i = L[i].cur;
	}
	L[i].cur = L[MAX_SIZE-1].cur;//L[MAX_SIZE-1].cur代表的是有效区的第一个结点在数组中的位置，也就是首结点的位置。将有效区全部归并到备用去
	L[MAX_SIZE-1].cur = 0;      //将头结点的cur指向备用去的头结点,也就说明有效区没有元素了.只有一个头结点,其余都是备用区
}


//如果cur_e是L中的数据元素，且不是第一个，则用pre_e返回它的前驱元素数据；否则操作失败，pre_e无定义

Status PriorElem(SLinkList L, int cur_e, int * pre_e)
{
	int i = L[MAX_SIZE - 1].cur; //第一个元素的位置下标
	if(L[i].data == cur_e && i == 0)
	{
		return FALSE;
	}
	int j = L[i].cur;

	while( L[j].data == cur_e && j != 0 )
	{
		i = L[i].cur;
		j = L[j].cur;
	}
	if(j == 0)
	{
		return FALSE;
	}
	else
	{
		*pre_e =  L[i].data;
		return OK;
	}
}

//若cur_e 是L的数据元素，且不是最后一个，则用next_e 返回它的后继元素，否则操作失败，next_e无定义
Status NextElem( SLinkList L, int cur_e, int * next_e)
{
	int i,j = L[MAX_SIZE - 1].cur;
	do
	{
		i = j;
		j = L[j].cur;
	
	}while(L[i].data == cur_e && j!=0);
		if(i)
		{
			*next_e = L[j].data;
			return OK;
		}
		else
		{
			return FALSE;
		}

}


/*
//在L中第i个元素之前插入新的数据元素e
Status ListInsert(SLinkList L, int i, int e)
{
	int j = L[MAZ_SIZE -1].cur;
	if( i> LengthList(L) && i<0)
	{
		return FALSE;
	}
	else
	{
		for(int x=0; x<i; ++x)
		{
			j = L[j].cur;
			
		}

	}
}*///这是我的第一种想法但是这种想法一开始就错误，首先我得从备用链表中取出一块给(有效)链表，进而才来改变插入位置的cur前后的值

//在L中第i个元素之前插入新的数据元素e
Status ListInsert(SLinkList L, int pos, int val)
{
	//分配得到的区域的地址下标为 sub
	int sub = Malloc( L );   
	int Pcur = MAX_SIZE-1;  //将静态链表的游标放在有效链表的头结点处，未放在有效节点处。
	
	if (sub == NULL)
	{
		printf("分配失败!\n");
	}
	else
	{	
		
		if( pos<0 || pos>LengthList(L) + 1) //插入范围出界，我们规定是在 pos个元素之前插入+ 在有效链表的尾部插入也可以，除此之外就是出界
		{
			return FALSE;
		}
		else
		{
			L[sub].data = val;
			for(int i=0; i<pos-1; ++i)
			{
				Pcur = L[Pcur].cur;
			}	//循环完成说明Pcur游标指向的是插入位置的前一个结点
			L[sub].cur = L[Pcur].cur;
			L[Pcur].cur = sub;
			return TRUE;
		}
	}
}




//删除在L中第pos个数据元素e，并返回其值
Status ListDelete(SLinkList L, int pos, int *e)
{
	//首先考虑出界的情况 当pos<1 和 pos>LengthList(L)时 出界
	//将要删除的部分归还给备用链表free()
	//归还后还要将原先的链表 接上
	int	Pcur = MAX_SIZE -1;
	if(pos<1 || pos >LengthList(L) )
	{
		return ERROR;
	}
	else
	{
		for(int i=0; i<pos-1; ++i)
		{
			Pcur = L[Pcur].cur;
		}	//循环完成说明Pcur游标指向的是删除位置的前一个结点
		int k = L[Pcur].cur;  //要删除结点的下标为k，也就是在有效链表的第pos个位置是 数组中相应的下标为 k的位置
		L[Pcur].cur = L[k].cur;
		*e = L[k].data; 
		Free(L,k);
		return OK;
	}
}


/*
void ListTraverse(SLinkList L)
{
	for(int i=0; i<MAX_SIZE; ++i)
	{
		printf("%d ",L[i].data);
	}
	printf("\n");
}
*/


void visit(int i)
{
	printf("%d\n",i);
}

//依次对L中的每个元素调用visit()
void ListTraverse(SLinkList L , void(*visit)(ElemType))
{
	int i = L[MAX_SIZE -1].cur;
	while(i)
	{
		visit(L[i].data);
		i = L[i].cur;
	}
	printf("\n");
}
/*
void visit(int i)
{
	printf("%d\n",i);
}*/


//调试bo基本操作
int main(void)
{
	SLinkList space;
	InitList(space);
	ListInsert(space, 1, 1);
	ListInsert(space, 2, 2);
	ListInsert(space, 3, 3);
	ListInsert(space, 4, 4);
	ListInsert(space, 5, 5);
	ListInsert(space, 6, 6);
	ListInsert(space, 7, 7);

	int e;//存放删除结点的值
	ListDelete(space, 3, &e);
	void(*p)(ElemType);//创建一个叫做p的 指向（1.无返回值 2.只有一个形参 3. 形参类型为ElemType）的函数的指针
	p = visit;
	ListTraverse(space,p);
	printf("删除的元素为:%d\n",e);
	return 0;
}
```


![SLinkList](assets\images\SLinkList.jpg)