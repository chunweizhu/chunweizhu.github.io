## 分析一个算法巩固函数指针的使用

```C
#include <stdio.h>
#include <stdlib.h>

#define MAX 10

typedef int ElemType;  //ElemType是int类型
typedef int Status;	   //Status也是int类型

typedef struct node
{
    ElemType elem[MAX];
    int length;
}SqList;			//定义了一个结构体变量SqList

SqList * init( SqList * L )    //初始化L
{
    L = (SqList*) malloc (sizeof(SqList));//初始是分配了44字节的内存，然后将44字节的内存首地址发送给L(4个字节)
    if( !L )
    {
        exit(0);
    }
    L->length = 0;

    int i = 1;
    while( i!=5 )
    {//elem 从第一号位置开始存储 (0无) 1 2 3 4
        L->length++;
        L->elem[L->length] = i; // [ ,1,2,3,4]
        ++i;
    }
    return L;
}

void show( SqList L )
{
    for( int i=1; i<=L.length; ++i )//从数组的第一个位置开始打印不是从第零号位置开始
    {
        printf( "%d ", L.elem[i] );
    }
    printf("\n");
}

Status compare( ElemType a, ElemType b )  //Status是int类型，所以可以返回整数 头文件中有标明 typedef int Status
{											//ElemTypey也是int类型头文件有标明
    if( a == b )						  //这个函数的功能是比较输入的两个int类型的参数值是否相等
    {
        return 1;
    }
    else
  
        return 0;
    }
}

int LocateElem_Sq( SqList L, ElemType e, Status(*compare)(ElemType, ElemType)) //传入了一个SqList类型的里面包含数组的L，和ElemType类型的e，以及把compare传入
																			   //了进来
{
    int i;
    ElemType *p;
    i = 1;
    p = L.elem+1;  //这里表示的是L这个变量中数组项中的地址再偏移一个指针的位置的地址 也就是数组中下标为  1  的数组地址，因为刚才赋值0号位置没有赋值从1开始的。
    while( i<=L.length && !(*compare)(*p++, e))
    {
        ++i;
    }
    if( i<=L.length)
    {
        return i;
    }
    else
    {
        return 0;
    }
}

int main()
{
    SqList *L = NULL;  //定义一个指向结构体变量的指针L
    L = init( L );     //初始化L，分配好一块 SqList类型变量 的内存区域，并将分配好的内存区域的首地址发送给L
    show( *L );
	Status(*compare)(ElemType, ElemType);
	compare = campare;   //将上面定义的  campare  函数的入口地址发送给函数式指针  compare  一个a一个o我也是醉了要我老命啊！臭程序员！
    printf("%d \n", LocateElem_Sq( *L, 3, compare) );

    return 0;
}
```

