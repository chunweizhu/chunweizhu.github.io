```C
# include<stdio.h>
typedef struct len
{
	int length;
	//int cnt;
}LEN,*PLEN;

int main(void)
{
	//len test;
	//test.length = 0;
	//test.length++;

	//int a = test.length;
	PLEN test_2;
	test_2->length++;
	int a = test_2->length;
	printf("%d\n",a);//ERROR为什么这个打印不出来呢？最后没有结果显示，不能这样赋值给a的							//吗？  用 . 的形式就可以为什么呀！
    					// 不是 test->length 等价于(*test).length
	return 0;
}
```

![1570677700598](F:\my_blog\chunweizhu.github.io\assets\images\1570677700598.png)