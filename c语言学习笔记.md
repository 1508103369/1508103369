# c语言学习笔记

goto语句,实现语句的转跳，尽量减少使用，使用过多会导致程序的紊乱

```c
#include<stdio.h>
int main() {
	printf("1111\n");
	goto test;
	printf("2222\n");
test:
	printf("33333\n");
	printf("4444\n");
	printf("nihao\n");
}
```

![image-20230502105422878](C:\Users\15081\AppData\Roaming\Typora\typora-user-images\image-20230502105422878.png)

打字游戏

```c
void test03(void) {
	printf("*******************************************\n");
	printf("输入过程中无法退出！***********************\n");
	printf("请按所给字母敲击键盘！*********************\n");
	printf("请按任意字母开始测试，按下首字母开始计时！*\n");
	printf("请入出错请按_表示！************************\n");
	printf("*******************************************\n\n");
}
void test04() {
	char ch;
	char str[51] = { " " };
	int i = 0;
	int count;
	time_t strat_time, end_time;
	while (1)
	{
		test03();
		ch = _getch();

		srand(time(NULL));//设置种子
		for (i = 0; i < 50; i++) {
			str[i] = rand() % 26 + 'a';// 随机26个字母
		}
		str[50] = '\0';//最后一个手动赋值为\0
		printf("%s\n", str);
		count = 0;
		for (i = 0; i < 50; i++) {
			ch = _getch();
			if (i == 0) {
				strat_time = time(NULL);//判断是不是从这里开始，是的话设定为开始时间
			}
			if (ch == str[i]) {
				count++;
				printf("%c", ch);
			}
			else {
				printf("_");
			}
			end_time = time(NULL);//结束时间
		}
		printf("\n对了%d个\n\n", count);
		printf("正确率为:%ld%c\n", count*100 / 50,'%');
		printf("用时%ld秒\n",   end_time-strat_time);
		while (1) {
			ch = _getch();
			if (ch == ' ') {
				break;//判定为空格的话重新回到大的while函数中
			}
			if (ch == 27) { //判定为esc的话退出函数，esc的ascll码为27
				return 0;
			}
		}
	}
}
```

防止头文件重复

会在头文件中使用#idndef

code.h

```c
#ifndef __FUN_H
#define __FUN_H
extern int FUN(int x,int y);
#endif
```

头文件这样写之后，就算在你的源文件进行了多次调用code.h这个头文件，里面的这个FUN函数也不会因此被多次调用
