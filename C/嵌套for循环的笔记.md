#include <stdio.h>
int main()
{
	int i = 10;
	int a = 2;
	int count = 0;
	int count2 = 0;
	for( i =10; i<100; i++)
	{
		for ( a=2; a <i; a++)
		{	
			if ( i % a == 0)
			{
				count++;
				break;	
			}
			else 
			{
				count = 0;	
			}
		}
		if ( count == 0)
		{	
			count2++;
			printf("%d ", i);
			if( count2 ==2)
			{
				count2 = 0;
				printf("\n");
			}
		}
	}
	return 0;
} 

```
利用嵌套for循环来筛选出素数的值是，第二个for循环里面的if不能直接接上printf，否则会让循环不断重复
可以定义一个count放于第二个for循环中用于判断
在第一个for循环的结尾里再进行count的值的判断用于提取素数
```
