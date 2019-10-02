---
categories:
  - Python   #注意一定要大写
published: true
---


## 队列的实现(c语言)

```c
# include<stdio.h>
# include<stdio.h>
# include<stdlib.h>
/*
这个程序的关键点是，如何设置queue结构体，因为数组的下标不好表示，而用front rare就直接代指下标
队列的作用是前出后进，内存连续，所以要保证程序封装后有前出后进的功能，那么对于数组来讲就是能够灵活的将数组的下标表示出来，
并且能够对下标进行四则运算，而运算是数字间的游戏，不能够影响对数组的操作，最好就是简单的用两个整型来标记前后，这种标记随时可以通过
数学关系进行转换，且这种转换只是在数组外部，与数组毫无牵连，由于数组坐标天然对应自然数，
所以反倒是可以用简单独立不想关的数字对于数组中元素进行高效索引，从而对数组中的元素进行标定，而队列就是标定好头和尾就行，头和尾比较灵活
都没有确定的特征进行定位，所以用两个灵活无关的数字进行标定头尾，加上指针的一个维度确定，就能根据需要来对头尾进行加工。
*/



typedef struct queue
{
	int *pBase;
	int front;
	int rare;
}QUEUE,*PQUEUE;

void init(PQUEUE pQ);
void en_queue(PQUEUE pQ,int val);
void out_queue(PQUEUE pQ ,int *pVal);
void travel_queue(PQUEUE pQ);
bool full(PQUEUE pQ);
bool empty(PQUEUE pQ);

int main(void)
{
	int val;
	QUEUE Q;
	init(&Q);
	en_queue(&Q,1);
	en_queue(&Q,2);
	en_queue(&Q,3);
	en_queue(&Q,4);
	en_queue(&Q,5);
	en_queue(&Q,4);
	en_queue(&Q,5);
	out_queue(&Q,&val);
	en_queue(&Q,11);
	en_queue(&Q,5);
	printf("出对的数字为:%d \n",val);
	travel_queue(&Q);

}

//初始化队列，并对队列中的参数进行初值处理
void init(PQUEUE pQ)
{
	//int len;
	//printf("请输入你需要的队列长度:");
	//scanf("%d",&len);
	pQ->pBase =(int*)malloc(sizeof(int)*6);
	pQ->front = pQ->rare = 0;
}

//判断队列是否已满
bool full(PQUEUE pQ)
{
	if((pQ->rare+1)%6 == pQ->front)//用下标来判断是否相等，就是当rare下标移动到下一个位置正好与front下标位置重合，则满！
	{
		return true;
	}
	else
	{
		return false;
	}
	
}

//将元素加入到队列末尾，队列是前进后出的
void en_queue(PQUEUE pQ,int val)
{
	if( full(pQ) )
	{
		printf("队列已满,入队失败!\n");
	}
	else
	{
		pQ->pBase[pQ->rare] = val;
		pQ->rare = (pQ->rare +1)%6;
		
	}
	return;
}

bool empty(PQUEUE pQ)
{
	if(pQ->front == pQ->rare)
	{
		return true;
	}
	else
	{
		return false;
	}
}


//将头元素从队列中取出
void out_queue(PQUEUE pQ ,int *pVal)
{
	if(empty(pQ))
	{
		printf("队列为空,出队失败!\n");
	}
	else
	{
		*pVal = pQ->pBase[pQ->front];
		pQ->front = (pQ->front +1)%6;
	}
	return;
}

//遍历队列
void travel_queue(PQUEUE pQ)
{
	if(pQ->front == pQ->rare)
	{
		printf("队列为空!\n");
	}
	else
	{
		while(pQ->front != pQ->rare)
		{
			printf("%d ",pQ->pBase[pQ->front]);
			pQ->front = (pQ->front+1)%6;
		}
		printf("\n");
	}
	return ;
}
```

