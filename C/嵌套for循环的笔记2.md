//求m,n的所有素数的和及其个数 
#include <stdio.h>
int main()
{
	int num1,num2;//定义两个数用于用户输入
	int i;
	int k;
	int o=0;
	int u=0;
	scanf("%d%d", &num1, &num2); 
	if( num1==1 )
	{
		num1=2;
	}
	for (i=num1;i<=num2;i++)
	{
		int l=1;
		for (k=2;k<i-1;k++)
		{
			if ( i % k== 0)
			{
				l = 0;
				break;
			}
			
		}
	if ( l==1 )
	{
		u++;
		o+=i;
	}
	}
	printf("%d，%d\n",u,o);
	return 0;
} 


```
使用判断语句时，要注意将判断的条件及时重置回原来的值
可以将判断的数字放于第一个for循环之后，第二个for循环之前
```
