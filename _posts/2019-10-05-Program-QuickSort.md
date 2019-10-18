##  快速排序
**title:**  QuickSort
**time:**  5 Oct 2019
**location:** Library


**NO COMMENT SOURCE CODE**
```c
# include<stdio.h>

int FindPos(int *, int low, int high);
void QuickSort(int * ,int low ,int high);


int main(void)
{
		int a[5] = {6,5,4,3,2};
		for(int i=0;i<5;++i)
		{
			printf("%d ",a[i]);
		}
		printf("\n");
		return 0;

}
void QuickSort(int *a ,int low ,int high )
{	
	int pos;
	if(low < high)
	{
		pos = FindPos(a,low,high); 
		QuickSort(a,low,pos-1);         
		QuickSort(a,pos+1,high);
	}
}

int FindPos(int * a, int low, int high)
{
	int PointValue = a[low]; 
	while(low < high)
	{
		while(a[high] > PointValue && low < high) 
		{
			--high;
		}
		a[low] = a[high];  
		
		while(a[low] < PointValue  && low < high) 
		{
			low++;
		}
		a[high] = a[low];
	}   
	a[low] = PointValue;
	return low;
}

```

**SOURCE CODE**
```C
# include<stdio.h>

int FindPos(int *, int low, int high);
void QuickSort(int * ,int low ,int high);


int main(void)
{
		int a[5] = {6,5,4,3,2};

		QuickSort(a, 0, 5);
    //0-5代表排序数据段,函数执行完成则表示数据段排序完成 //0代表从哪个位置开始才对「其后数字 -到5代表		位//置结束位置」排序。

		for(int i=0;i<5;++i)
		{
			printf("%d ",a[i]);
		}
		printf("\n");

		return 0;

}

void QuickSort(int *a ,int low ,int high )
{	
	int pos;

	if(low < high)
	{
		pos = FindPos(a,low,high); //将找到的位置发送个pos让他进一步递归剩下的数组中的数字
		QuickSort(a,low,pos-1);         //递归 PointValue之前之后的 数组中的数据段 那么什么时候终止呢，只要low < high就继续，否则就终止
		QuickSort(a,pos+1,high);
	}
	
	

}



//找到「PointValue」数字应该在的正确位置，次程序说的是升序是所应该在的位置,一次运行只能找到一个位置哦，并且已经将其移动到数组中的正确位置了

int FindPos(int * a, int low, int high)
{
	
	/*
	从某个位置拿出一个数 PointValue「是否是必须从第一个位置取」，然后从数组的两边进行遍历，两边递进逼		近，将两边的逐个元素分别与PointValue值进行对比，比PointValue数大的往后丢，『理论上讲可以不用去管丢	   在什么地方，只要是比PointValue大的都从后往前丢，比PointValue小的都从前往后丢，直到当所有的数字都遍     历了一遍之后，在二者之间插入的数字即为PointValue，也就找到了相应PointValue所应该在的正确位置』但是     考虑到流程的连贯和遍历的简便，所以在丢数字时按照 「从前往后，从后往前」依次按照顺序进行丢放，使得计算机     做出高效的定位行为，而不会因为随意丢放数字而做一些重复无效的操作。
	
	有两个位置：low位置 high位置,先是从high位置起，「如果high处的位置对应的数值比PointValue值小」，此     时他的位置在最后，所以以PointValue为参考点此时他应该往前放，而这里的往前放的本意是我们得放在           PointValue实际正确位置之前，而我们却不知道PointValue的具体位置在	哪，但是我们可以把它放在最前端，     这样的极端条件不论PointValue在哪个位置都是满足之前这个苛刻条件的，你似乎也可以做一个这样的思想实验，     那就是我假定PointValue的位置在某个地方，我就把他放在这个位置之前，当不满足假定时，就修正假定，假定满     足时的移动也可以保留，因为PointValue的具体位置本就是根据一步一步的移动数据得出来的，但我觉得这种理解     方式太过于抽象，最好还是 认识到，数组就那么长，最前就是0位置，最末为就是数组长度-1的位置，我以两个极端     放数字一定不会有问题，这是为了方便理解这样说。	「如果high处的位置对应的数值比PointValue值大」，因     为他本身就在最后，那就让他呆在最后就好了，因为自然就满足比PointValue的条件不需要改动。
	
	当最终可以想见，从两边逼近的结果是low == high 那个位置就是
	PointValue的位置。然后将数组分成前后两个部分再次使用同样的方式确定下相应PointValue的值
	*/

	int PointValue = a[low]; //将最左边的值设为要找位置的数


	/*
	while( a[high] < PointValue && low <= high )  //如果在后面的值比PointValue小那就往前丢，由于low 处的值已经被PointValue保留且low又在最前面，那么我们就可以
												  //将后面的值赋值给low处的值达到移动的目的  注意这里有一个隐含的前提，那就是low确实是比high要前才行，不然会适得其反
												  //违反本意了
	{ 
		a[low] = a[high];						  //将靠后的数值赋值给靠前的数值
		++low;                                    //表示low的位置向前移动一个位置，因为low原来的位置已经保存了一个相对正确的值「比PointValue小在前---正确」
												  //那么 low代表的前面就可以相应的推广到后一位的位置。可供选择插入的前的范围扩大了。high位置不变
	}

	while(a[low] > PointValue)                  
	{
		a[high] = a[low];
		--high;
	}
	*/   //我这种思路是错误的 我设计的是当满足条件时继续，然而实际是需要当满足条件是终止，所以条件应该取反才行，满足条件继续是不需要移动，需要移动时是要停止，并赋值操作



	while(low < high)
	{
		while(a[high] > PointValue && low < high)  //while 具有判断和循环的功能，对需要 「条件终止」 的程序使用，而条件一般是动态的，没有清晰的终止预知
		{
			--high;
		}

		a[low] = a[high];  //while执行完后执行，说明退出循环是 操作开始发生变化了，这才是关键点到了，发生了转折了
		
		while(a[low] < PointValue  && low < high)  //low < high不是多余的，不可以省略！
		{
			low++;
		}
		a[high] = a[low];
	}   //执行完此时的high 与 low在同一个位置，即low == high 数值相等 也就是PointValue应该在的正确位置

	a[low] = PointValue;

	return low;


}




```