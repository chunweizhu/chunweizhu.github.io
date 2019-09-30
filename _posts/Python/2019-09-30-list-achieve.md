## 链表实现(创建、删除、增加、遍历、输出、计数)
---
categories:
  - Python   #注意一定要大写
published: false
---

```c
# include<stdio.h>
# include<malloc.h>
# include<stdlib.h>
typedef struct node
{	
	int date;
	struct node *pNext;

}NODE,*PNODE;

//前置声明
PNODE creat_list();
bool travel_list(PNODE pHead);
bool insert_list(PNODE pHead, int pos, int val);
bool delete_list(PNODE pHead, int pos, int*val);
void sort_list(PNODE);
int length_list(PNODE pHead);

//主程序
int main(void)
{
	PNODE pHead;
	pHead = NULL;
	pHead = creat_list();
	travel_list(pHead);
	//insert_list(pHead, 4, 100);
	//travel_list(pHead);
	//int val;
	//delete_list(pHead, -1, &val);
	//printf("删除的结点值为：%d\n",val);
	//travel_list(pHead);
	//int len = length_list(pHead);
	//printf("%d",len);
	//pHead =	sort_list(pHead);这种方法不可取！
	sort_list(pHead);
	travel_list(pHead);
	return 0;
}

//创建链表
PNODE creat_list()
{

	PNODE pHead = (PNODE)malloc(sizeof(NODE));
	
	if(pHead == NULL)
	{
		printf("动态内存分配失败!\n");
		exit(-1);
	}

	pHead->pNext = NULL;
	PNODE pTail = pHead;//创建一个指针好对后续结点进行操作因为要保证返回一个头结点，所以这个是头结点的副本，而返回的是头结点pHead.
	printf("请输入你需要几个结点：\n");
	int len;
	scanf("%d",&len);
	int val;//结点存储的值

	for(int i=1;i<=len;++i)
	{
		
		printf("请输入第%d结点放入何值：", i);
		scanf("%d",&val);
		PNODE pNew = (PNODE)malloc(sizeof(NODE));
		pNew->date = val;
		pTail->pNext = pNew;
		pNew->pNext = NULL;
		pTail = pNew;
		
	}
	return pHead;

}

//遍历链表
bool travel_list(PNODE pHead)
{
	PNODE p;
	p = pHead->pNext;//直接将指针从头结点移动到首节点位置。
	while( p != NULL)
	{
		printf("%d ",p->date);
		p = p->pNext;
	}
	printf("\n");
	return true;
}

//插入结点
bool insert_list(PNODE pHead, int pos, int val)
{
	//创建一个指针指向头结点，不是指向首结点，头结点中不包含有效数字，
    //只含有首节点地址信息，首节点中含有有效值val。
	PNODE p = pHead;
	//创建一个空结点
	PNODE pSert = (PNODE)malloc(sizeof(NODE));
	
	if(pSert == NULL)
	{
		printf("动态内存分配失败!\n");
		exit(-1);
	}
	
	pSert->date = val;
	pSert->pNext = NULL;

	//简单判断pos的范围错误
	if(pos<1)
	{
		printf("错误，请检查结点插入的位置!\n");
		exit(-1);
	}
	
	//移动指针至插入位置的前一个结点处，不用考虑是否超出插入范围，也不用管插入的位置处是否存在节点
	for(int i=1;i<=pos-1;i++)
	{
		p = p->pNext;
	}
	//判断是否超出插入界限，很好的思想，先不管是不是插入过界，先假设可以插入，
    //再来判断是否过界，往往这种事后的判断更容易！
	if (p == NULL)
	{
		printf("错误!插入位置超出链表范围!\n");
		exit(-1);
	}
	//把结点连接好
	pSert->pNext = p->pNext;
	p->pNext = pSert;

	return true;
}


//删除节点
bool delete_list(PNODE pHead, int pos, int* val)  
//该方法有一个不足就是没有释放malloc创建时分配给删除结点的内存，
//所以最好用一个指针指向删除的结点，然后free掉分配的内存。
{	
	//首先简单判断pos是否有明显错误 如 pos ==  -1 等
	if (pos < 1 )
	{
		printf("错误，请核对删除结点位置!\n");
		exit(-1);
	}

	PNODE p = pHead;

	//将指针移动至删除结点前一个节点的位置
	for(int i=1 ; i<=pos-1; i++)
	{
		p = p->pNext;

	}
	//判断删除结点是否在链表范围之内
	if (p == NULL || p->pNext == NULL)
	{
		printf("删除结点不存在!\n");
		exit(-1);
	}
	
	//将删除的链表中的date值放入main函数定义的val变量中
	*val = p->pNext->date;
	//跳过删除的结点，将链表连接起来
	PNODE q = p->pNext;
	p->pNext = p->pNext->pNext;
	free(q);
	return true;
} 

int length_list(PNODE pHead)
{
	int length = 0;
	PNODE p = pHead->pNext;
	while(p != NULL)
	{
		p =	p->pNext;
		length++;
	}
	return length;
}


/* //我开始是想将结点整体移动，也就是改变连接之间地址pNext的方式进行，然而这种方式很难将头结点锁定
   //在每一次拍完序后的首结点之前，暂时无办法进行
   //一般插入删除才去改变地址链接顺序，排序可以将数字进行排列即可，无须进行链表的地址链接的重新排列
PNODE sort_list(PNODE pHead)
{
	PNODE pRe = pHead->pNext;
	
	int len = length_list(pHead);

	for (int j=1; j<len;++j)
	{
		for(int i=1; i<len; ++i)
		{
			if (pRe->date > pRe->pNext->date)
			{
				PNODE pBck = pRe->pNext;
				pRe->pNext = pBck->pNext;
				pBck->pNext = pRe;
				//pHead->pNext = pBck;//错误，当pHead进行第二轮时将会跳过前面所指向的结点，会导致数据缺失，pHead并不是一直在指向头结点
				//pRe = pRe->pNext;
			
			}
			//pRe = pRe->pNext;

		}

		pRe = pHead->pNext;
	}
	return pHead;
		
}

*/

//下面这中方式就是直接将数字之间进行互换而未对链表的地址顺序进行重排，
//其实对内存地址的重排目前不知道有什么么特殊的用途
//不对链表的地址进行操作，那么头链接就固定下来，无须改变。

void sort_list(PNODE pHead)
{
	PNODE pRe = pHead->pNext;

	int len = length_list(pHead);

	for (int j=1; j<len;++j)
	{
		for(int i=1; i<len; ++i)
		{
			if (pRe->date > pRe->pNext->date)
			{
				PNODE pBck = pRe->pNext;
				int t = pRe->date;
				pRe->date = pBck->date;
				pBck->date = t;
				pRe = pRe->pNext;	
			}
		}

		pRe = pHead->pNext;
	}
	//free(pRe);//error 会将指向的内存空间释放，注意只用那种不在链表之内的指针可以释放
	return ;
}
```

