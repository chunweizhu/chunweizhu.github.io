## 字符串定长顺序存储结构
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

#define MAXSTRLEN 40
typedef unsigned char SString[MAXSTRLEN + 1];//unsigned什么意思？怎么用？
//函数声明
Status StrDelete(SString S, int pos, int len);
Status StrInsert(SString S, int pos, SString T);

//bo4-1串采用定长顺序存储结构的基本操作bo

//#1 初始条件：chars是字符串常量 操作结果：生成一个其值等于chars的串T

Status StrAssign(SString T, char * chars) //将chars指针指向的字符串转化为SString类型，并存入T中
{
	int StrLen = strlen(chars);//strlen()是内置的字符串求串长的函数 接收的形参就是字符串的首地址
	if ( StrLen > MAXSTRLEN )
	{
		return ERROR;
	}
	else
	{
		T[0] = StrLen;
		for(int i=1; i<=T[0]; ++i)  //这个地方用T[0]而不用StrLen 比较好，因为，T[0]所代表的就是串的长度，我们的模型中就是这样的用StrLen就不是那么容易看懂
		{
			T[i] = *(chars+i-1);//chars[i]也可以;
		}
		return OK;
	}
}//这就完成了对一个字符串的生成操作，与直接用数组存储字符串 类似这样 char chars[size] = "Hello World!"存储的区别在于，我们将数组的首地址T[0]用来存储字符的长度，所以这样形成的
 //T数组形式的字符串可以直接获取他的字符长度，所以当我们从输入端将字符输入进来，再调用StrAssign()函数时就将这个字符串生成为定长顺序存储结构了，这也就是定长顺序的存储结构。


//#2初始条件：定长顺序存储的串S存在；操作结果：由串S复制得串T(也为定长存储结构的字符串)
Status StrCopy(SString T, SString S)
{
	if(S == NULL)
	{
		return ERROR;
	}
	else
	{
		for(int i=0; i<=S[0]; ++i)
		{
			T[i] = S[i];
		}
		return OK;
	}

}

//#3 初始条件：串S存在，操作结果：若S为空串，则返回TRUE，否则返回FALSE。
Status StrEmpty(SString S)
{
	if(S[0] == 0)
	{
		return TRUE;
	}
	else
	{
		return FALSE;
	}
}

//#4 初始条件：串S和串T存在 操作结果：若S>T则返回值>0,若S == T则返回值 = 0;若 S < T,则返回值 < 0。
Status StrCompares(SString S, SString T)
{
	for(int i=1; i<=S[0] && i<=T[0]; ++i)
	{
		if(S[i]!= T[i])
		{
			return S[i]-T[i];
		}
	}
	return S[0]-T[0];//这里这样用非常妙，既完成了比较又返回了比较后的结果！减法就有这样的功能！
}


//#5 初始条件：串S存在 操作结果：返回S的元素个数，称为串的长度
Status Strlength(SString S)
{
	if(!S)
	{
		return FALSE;
	}
	else
	{
		return S[0];
	}
}

//#6 初始条件:串S存在  操作结果：将串S清空 
void CleaString(SString S)  //为什么不需要判断是否S存在呢？因为如果能够传参数进来，也就是一定是SString类型，而SString类型是 定长的 就一定S[0]不会是NULL了。可以为0
{
	if(S)
	{
		S[0] = 0;
	}
	else
	{
		printf("清空错误!\n");
	}
	
}

//#7 初始条件：串S1和S2存在  操作结果:用T返回由S1和S2联接而成的新串
Status StrConcat(SString T, SString S1, SString S2 )
{
	if(S1[0] + S2[0] <= MAXSTRLEN )
	{
		T[0] = S1[0] + S2[0];

		for(int i=1; i<=S1[0]; ++i)
		{
			T[i] = S1[i];
		}
		for(int j=1; j<= S2[0]; ++j)
		{
			T[S1[0] + j] = S2[j];
		}
		return TRUE;
	}
	else if(S1[0] == T[0])
	{
		T[0] = S1[0];
		for(int i=1; i<=S1[0]; ++i)
		{
			T[i] = S1[i];
		}
		return FALSE;
	}
	else
	{
		T[0] = MAXSTRLEN;

		for(int i=1; i<=S1[0]; ++i)
		{
			T[i] = S1[i];
		}
		for(int j=1; j<= MAXSTRLEN - S1[0]; ++j)
		{
			T[S1[0] + j] = S2[j];
		}
		return FALSE;
	}
	return 0;
}

//#8 用sub返回串S的第pos个字符起长度为len的子串

Status Substring(SString sub, SString S, int pos, int len)
{
	if( pos<1 || S[0]<pos || len > S[0]-pos+1 || len<0 )
	{
		return FALSE;
	}
	else											//Q:如何使用两个变量同时在for中循环，好像郝斌用过这一方法!就是两个不一样的需要循环的部分，用一个for循环完成？
	{
		sub[0] = len;
		for(int i=1; i<=len; ++i)
		{
			sub[i] = S[ pos+i-1 ];
		}
		return OK;
	}
}


//#9若主串S中存在和串T值相同的子串，则返回它在主串S中第pos个字符之后第一次出现的位置，否则函数值为0
Status StrIndex(SString S, SString T, int pos)
{
	int i,j;
	i = pos;
	j = 1;
									
	while( i<= S[0] && j<= T[0])
	{
		if (i<1 || T[0]- j +1 > S[0]-i+1 || i>S[0])  	//程序排除了匹配过程中出现 串长超出范围 + 匹配位置超出范围的情况，所以后续的匹配过程中仅需要考量在范围之的匹配即可
		{
			return 0;
		}

		if (S[i] == T[j])					//匹配上就 +1 往后移动，反过来讲就是，能够往后移动说明匹配成功了！
		{
			++i;
			++j;
		}
		else
		{
			i = i-j+2;						//如果匹配不成功，则指针回朔到开始能匹配上的首位字符的下一个字符
			j = 1;
		}
	}										//while循环结束，且没有执行return 0;说明匹配没有超出范围

	if(i>S[0] && j<=T[0])    //退出循环是因为i溢出说明未匹配成功！注意这里要格外小心，因为我开始认为退出循环一定是因为j溢出，但是当T中只有一个字母时，①当未匹配上时，i会溢出，②匹配上则i和j同时溢出！
	{						//i溢出说明没有与T匹配的字符串，当ij同时溢出，说明有匹配的字符串，只要j溢出，那就说明匹配上了，因为j就代表最终在T中匹配到了第几个，j>T[0]说明匹配超过了T的字符串个数，所以一定匹配完全了！！！！！
		return 0;
	}
	return i - T[0];  //若单单只有这一步，没有上面的if语句，当T只有一个字符串时，那最后一个就一定匹配上了错误！！
}											


											//书上不是用的我这种方法，用的是另外一种，我把思考的重心放在了考率在匹配过程中后移的时候，T中的长度超过了S中(i指向的位置+之后的元素)的长度
											//当不匹配时我得考量将i回朔后，后面的元素个数是否   >  T中的元素个数,如果不是则不再需要继续比较，长度不够，直接退出。
										    //书中的考虑角度则与我不同，他只是在开始比较时考量了长度是否在范围之内，其余情况则没有，他会一直比较到最后，即使T的长度＞后续S中的元素，也会
											//进行比较，再通过循环结束后 通过判断j的大小来判断是否匹配完整，j>T[0]则匹配完整，否则j<0说明T中还有未匹配的元素，但程序就已经循环结束，
											//所以S中没有含T的子串!这两种方式，当T的串长和S的串长在同一个数量级时我的方式会更好，如果S >> T的串长，则书中的算法更好！





//#10 用V替换主串S中出现的所有与T相等的不重叠的子串  //这里的替换是相同长度之间的替换还是允许不同长度之间的替换呢？？//你这样考虑非常复杂，不如就直接用删除+插入操作，就不用考虑是不对等长

													//什么的了，如果还要考虑v的长度，那将是非常复杂的，直接就用删除插入操作，不用考虑V与删除的子串之间的关系了！
Status StrReplace(SString S, SString T, SString V)
{
	
	//如果T为空串则返回错误，S中不可能有空串的子串
	if(StrEmpty(T))
	{
		return ERROR;
	}
	//不为空字符串那就可以与S进行匹配操作了，那就开始进行索引，看S在什么地方与T相匹配

	int pos = 1;					//从S的开头进行匹配，从头开始匹配!	
	int Pos = StrIndex(S, T, pos);  //在S中的什么位置与T匹配了

	Status K;
	while( Pos != 0 )
	{
		//匹配之后就从这个位置起删除长度为T[0]的字符串，然后再从这个位置起插入V字符串
		StrDelete(S, Pos, T[0]);
		K = StrInsert(S, Pos, V); //在Pos位置插入V，返回插入是否成功给K
		//检测一下是否完全插入，没有完全插入那就是超出了长度范围返回ERROR
		if(!K)      //!!插入不成功，插入益处
		{
			return ERROR;
		}
		pos = Pos +V[0];			//从下一个位置继续索引看看S中还有没有与T匹配的字符串
		Pos = StrIndex(S, T, pos);  //索引下一个可能的插入的位置，没有时Pos = 0;有则不为0;
	};
	return OK;
	
}


//#11 从串S中删除第pos个字符起长度为len的子串,将之后的字符串迁移len的长度
Status StrDelete(SString S, int pos, int len)
{
	if(len <0 || pos<1 || len > S[0]-pos+1)
	{
		return FALSE;
	}
	else
	{
		for(int i=1; i<=S[0]-len-pos+1; ++i)
		{
			 S[pos+i-1] = S[len+pos+i-1];
		}
		S[0] = S[0] - len;

		return OK;
	}
}


//#12 在串S中的第pos个字符之前插入串T
Status StrInsert(SString S, int pos, SString T)
{
	if(pos<1 || pos > S[0]+1)
		return ERROR;


	//#完全插入S中
	if(S[0] + T[0] <= MAXSTRLEN)
	{
		int len = S[0]+ T[0];


		for(int i=S[0]; i >=pos; --i)
		{
			S[i+T[0]] = S[i];
		}
		for(int j=1; j<=T[0]; ++j)
		{
			S[pos+j-1] = T[j];	
		}
		S[0] = len;
		return TRUE;
	}
	//#部分插入S中
	else
	{
		 for(int i = MAXSTRLEN-T[0]; i>=pos; --i)
		 {
			S[ i+T[0] ] = S[i];
		 }
		 for(int j=1; j<T[0];++j)
		 {
			 S[pos+j-1] = T[j];
		 }
		 S[0] = MAXSTRLEN;
		 return FALSE;
	}
}


//打印输出SString中有效长度的数值，也就是长度等于S[0]处数字的值，多（溢出）的少的都不打印
void StrPrint(SString S)
{
	int i;
	for(i=1; i<=S[0]; ++i)
	{
		printf("%c",S[i]);  //%c 字符型。可以把输入的数字按照ASCII码相应转换为对应的字符
	}
	printf("\n");  
}


//调试
int main(void)
{
	SString S1,S2,T,sub;  //这样的形式定义T有多层含义，SString定义的是一个数组，而且这个数组是有特定大小的为[MAXSTRLEN + 1]，T既是数组的首地址，也是数组的名称所以他可以当做地址传入函数而不需要&符号
	
	char chars1[MAXSTRLEN +1]; // = "Hello World!"; +1是要包括串结束符 \n
	char chars2[MAXSTRLEN +1];//不用在乎是否是正好填满 MAXSTRLEN+1 因为后面是要用这个传统的字符串转换成定长字符串的，所以会在开头[0]处加一个表示具体有效长度数字 的字符串
	char chars3[MAXSTRLEN +1];

	printf("请输入chars1中的字符串:\n");
	gets(chars1);										//从键盘获得输入，并存入chars1变量中
	printf("请输入chars2中的字符串:\n");
	gets(chars2);										//从键盘获得输入，并存入chars2变量中
	printf("请输入chars3中的字符串:\n");
	gets(chars3);



	
	StrAssign(S1,&chars1[0]); //将chars1    数组中存放的字符串转化为SString类型，并存入S1 (SString类型) 中
	StrAssign(S2,&chars2[0]); //将chars2    数组中存放的字符串转化为SString类型，并存入S2 (SString类型) 中
	StrAssign(T, &chars3[0]);  //将chars3    数组中存放的字符串转化为SString类型，并存入T (SString类型) 中

	StrReplace(S1, S2, T);



	//Substring(sub, S1, 3, 4);

	//StrDelete(S1, 2, 3);
	//StrInsert(S1, 3, S2);

	//StrConcat(T, S1, S2);
	//StrPrint(T);
	//StrPrint(sub);
	//int pos = StrIndex(S1, S2, 1);
	//printf("%d\n", pos);
	StrPrint(S1);


	return 0;
}





/*
___________________________________________________

  后面是书中代码,上面是我自己的想法
	
  时间：2019-10-22 19：01
_________________________________________________

*/

/*
Status StrCompare(SString S, SString T)
{
	if(!S || !T)
	{
		return FALSE;
	}
	else
	{
		int i = 1;
		while(i <= S[0] && i<=T[0])
		{
			if(S[i] < T[i])
			{
				return -1;
			}
			else if(S[i] == T[i])
			{
				++i;
			}
			else
			{
				return 1;
			}
		}
		if(S[0] > T[0])
		{
			return 1;
		}
		else if(S[0]<T[0])
		{
			return -1;
		}
		else
		{
			return 0;
		}
	}
}

*/




//下面是书中的关于模式串匹配的代码
/*
int Index(SString S, SString T,int pos)
{
	int i,j;
	if(1<pos, S[0]>pos)
	{
		i = pos;
		j = 1;
		while(i<=S[0] && j<=T[0])
		{
			if(S[i]==T[j])
			{
				++i;
				++j;
			}
			else
			{
				i = i-j+2;
				j = 1;
			}
		}
		if(j>T[0])
			return i-T[0];
		else
			return 0;
	}
	else 
		return 0;
}
*/



/*
Status StrReplace(SString S, SString T, SString V)
{
	int i=1;
	Status k;
	if(StrEmpty(T))
		return ERROR;
	while(i)
	{
		i = StrIndex(S, T, i);
		if(i)
		{
			StrDelete(S, i, T[0]);
			k= StrInsert(S, i, V);
			if(!k)
			return ERROR;
			i+=V[0];
		};
	}
	return OK;
} 
*/



/*
//#11 从串S中删除第pos个字符起长度为len的子串,将之后的字符串迁移len的长度
Status StrDelete(SString S, int pos, int len)
{
	if(len <0 || pos<1 || pos>S[0]-len +1)
	{
		return FALSE;
	}
	else
	{
		for(int i=pos+len; i<=S[0]; i++)
		{
			 S[i-len] = S[i];
		}
		S[0] = S[0] - len;     //这种S的长度变化是更随插入删除操作一起的，只有这样的操作才会改变长度！

		return OK;
	}
}
*/

/*
//#12 在串S中的第pos个字符之前插入串T
Status StrInsert(SString S, int pos, SString T)
{
	int i;
	if(pos<1 || pos > S[0]+1)
		return ERROR;


	//#完全插入S中
	if(S[0] + T[0] <= MAXSTRLEN )
	{
		for(i=S[0]; i >=pos; i--)
		{
			S[i+T[0]] = S[i];
		}
		for(int i=pos; i<pos+T[0]; i++)
		{
			S[i] = T[i-pos+1];	
		}
		S[0] +=T[0];
		return TRUE;
	}
	//#部分插入S中
	else
	{
		 for(int i = MAXSTRLEN; i>=pos+T[0]; i--)
		 {
			S[ i ] = S[i - T[0]];
		 }
		 for(int j=pos; j<pos + T[0] && i<=MAXSTRLEN; j++)
		 {
			 S[j] = T[j-pos+1];
		 }
		 S[0] = MAXSTRLEN;
		 return FALSE;
	}
}	
*/
```