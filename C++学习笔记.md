# C++学习笔记

## 实参和形参的区分

在编程语言中，形参（形式参数）和实参（实际参数）是两个重要的概念，它们分别表示函数定义过程中使用的变量名和调用函数时传递的真实参数。下面简单介绍一下二者之间的主要区别：

1. 定义。形参定义在函数声明或定义中，而实参是在函数调用时传递给函数的。

2. 类型。形参的类型是定义函数时声明的，而实参的类型是根据传递的值自动推断的。

3. 值传递。在函数调用期间，实参的值被复制到相应的形参中，因此，改变形参的值不会影响实参的值。

例如，下面是一个函数的定义：

```c
void calculateTotal(int a, int b)
{
  int total = a + b;
  printf("The total is %d", total);
}
```

在这个函数中，`a`和`b`是形参，表示整数类型的变量。在调用这个函数时，需要传递两个实参，如下所示：

```
calculateTotal(5, 10);
```

在这个调用中，5和10是实参，它们传递给`a`和`b`作为函数执行的参数。

## 头文件

```c++
#include<iostream.h>
#include<time.h>
using namespace std
```

## 强制类型转换

```c++
float a = 21.6f;
int x = 0;
x=(int)a;//把a的值强制转换为int类型，临时性的
cout << x;//21
```

```c++
float a = 23.5f;
float b = 21.6f;
int x = 0;
x = (int)(a + b);//如果是多个参数，后面一定也要打小括号；
cout << x;
```



## 进制问题

输入内容为二进制应以0b开头；八进制为0开头；十六进制为0x开头；输出默认全部自动转换为十进制;

```c++
	 cout << 0b1001 << endl;
	 cout << "输出为二进制" << bitset<8>(0b1001) << endl;
	 cout << 0123 << endl;
	 cout << "输出为八进制" << oct << 0123 << endl;
	 cout << 0x1a << endl;
	 cout << "输出为十六进制"hex << 0x1a << endl;


```

## 变量的声明

如果有先使用后声明的情况要在外部先进行全局声明;先使用 定义

```c++
extern int a; //先定义a的类型为int，如没有这个定义下面的a会报错（主要用于多文件编程）
void test03() {
	cout <<"a=" << a << endl;
}
int a=0;
```

## 键盘给变量赋值

```c++
int a = 0;
	cout << "请输入a的值:"; //cin根据a的类型输入，int类型只取整数部分
	cin  >> a;
	cout << "a=" << a<< endl;
```

### 同时输入多个值

```c++
float a ,b= 0;
cout << "请输入a和b的值:";//两个值之间用(空格\tab\回车\隔开)
cin  >> a>>b;
cout << "a=" << a<<endl<<"b=" <<b<< endl;
```

##  字符类型

### 1.字符常量

单引号只能作用于一个字符如   ‘a’,    'b'    类似于‘abc’这样的时错误的

```c++
cout << 'a' << endl;//输出字符‘a'
cout << (int)'a' << endl;//输出a的ascll码
```

### 2.字符变量

```c++
char ch1='a';
	cout << ch1 << endl;//输出为‘a'
	cout << (int)ch1 << endl;//输出为a的acsll码：97
```

一般初始化char类型时：char ch='\0';   这里的\0的ascll值为0

```c++
 char ch2 = '\0'; //初始化ch2的ascll码值为0
	cout << "请输入一个字符：";
	cin >> ch2;   //cin会自动判断ch2的类型，且一次只能输入一个字符
	cout << "ch2=" << (int)ch2;//输出a的ascll代码值
```

### 3.大小写转换

```c++
char ch1 = 'a';
	ch1 = ch1 - ('a' - 'A');//小写a转化为大写A 或者直接ch1=ch1-32
	cout << "ch1="<<ch1<<endl;
	char ch2 = 'A';
	ch2 = ch2 + ('a' - 'A');//大写A转化为小写a  ch2=ch2+32
	cout << "ch2="<<ch2<<endl;
```

```c++
char ch = '\0';
cout << "请输入一个字符：";
cin >> ch;
if (ch >='A' and ch <= 'Z')
{
	ch = ch + ('a' - 'A');//大写转小写
}
else if (ch >= 'a' and ch <='z')
{
	ch = ch - ('a' - 'A');//小写转大写
}
cout << "ch=" << ch;
```

### 4.有符号数和无符号数

有符号数和无符号数都占8位

#### 有符号数

有符号数最高位为1表示负数

最高位为0表示正数

一般来说把-0看成-128

取值范围是-128~127

```c++
定义有符号变量
int num;//num为有符号数
signed int num;//使用signed显示说明
```



#### 无符号数

所有二进制位都是数据

取值范围是0~255

```c++
定义无符号变量
unsigned int num;
```

### 对数据的存

负数再计算机中以补码的方式存储

非负数，八进制，十六进制一源码存储

```c++
short data1 = -10;
cout << bitset<16>(data1) << endl;//1111111111110110补码方式存储
short data2 = 10;
cout << bitset<16>(data2) << endl;//0000000000001010原码方式存储
short data3 = 03;
cout << bitset<16>(data3) << endl;//0000000000000011 0开头为8进制，原码存储
short data4 = 0x14;
cout << bitset<16>(data4) << endl;//0000000000010100 十六进制以原码方式存储
```

### 对数据的取

```c++
unsigned short data = -10;//由于定义的data是无符号数的，-10是有符号数，这里会把-10转为补码进行存储
cout << bitset<16>(data)<<endl;//1111111111110110
cout << "data=" << data;//data=65526  这里输出时又会把补码转为10进制输出(无符号数每一位都是数据)
short data5=0x8080;
cout<<data5;//  -32640  因为这里时4位16进制数，先转为2进制，首位为1就是负数，如果首位是0直接输出了
```

### 其他关键字

#### 1.const普通变量

const定义的变量是只可读的，只能被初始化，不可以通过后期赋值

如果是以常量初始化的，系统不会立马给这个关键字开辟空间：

```c++
const int data=10;  //以常量初始化data  不会立马给data开辟空间，直到需要取地址操作时；
int a=10;
const int data =a; //以变量方式初始化data 会立马给data开辟空间 
```



```c++
const int data=100; //如果这里是int a=10; const int data=a;以变量初始化data的话，下面两个输出的都是200；
int* p = (int*)&data;
*p = 200;
cout << "*p="<< * p<<endl;//*p=200  这里访问的是指针的地址所以为200
cout << "data=" << data;//data=100  通过变量名输出他访问的是事先存在常量表中的数据

```

#### 2.register修饰寄存器变量

如果变量高频繁使用，编译器会自动把变量存储在寄存器中 目的是为了提高访问效率

如果用户想将变量放入寄存器中

```c++
register int data=0;//data将存入寄存器中（cpu）
//放入寄存器中中用户无法对这个变量进行取地址操作
&data；//取地址操作会失败
```

#### 3.volatile强制访问内存

```c++
volatile int data=0;//对data的访问，必须从内存访问
```

#### 4.sizeof测量类型的大小

```c++
int a = 1;
	cout << sizeof(a)  <<endl;//4
	cout << sizeof('a') << endl;//1
	cout << sizeof(10) << endl;//4
	cout << sizeof(float) << endl;//4
	cout << sizeof(long) << endl;//4
	cout << sizeof(short) << endl;//2
```

#### 5.typedef为已有的类型取别名

```c++
	short int a = 18;
	typedef short int myi;//设定short int类型的别名为myi，下面用myi作用等于short int 同样是定义短整形常量；
	myi b = 1;
	cout << "a=" << a << endl;
	cout << "b=" << b << endl;
```

### 转义字符

#### 1.特殊字符

```c++
'\0'==ascll的值=0
'\n'回车
'\t'tab缩进符
'\r'回到行首
'\a'发出警报；
```

#### 2.八进制转义

'\ddd'每个d的取值范围是0~7,最多三位；

如'\123'

#### 3.十六进制转义

'\xhh'开头固定为x，每个h的取值范围是0~9,a~f,除了x最多能识别两位h；

### 类型转换

#### 1.自动类型转换

##### 1.无符号数和有符号数参加运算，需要向有符号数转化成无符号数

```c++
int data1 = -10;
unsigned int data2 = 3;
if (data1 + data2 > 0)
{
    cout << ">0" << endl;
    cout << bitset<16>(data1) << endl;
    cout<< "data1+data2=" << data1 + data2 << endl;
}
else
{
    cout << "<=0";
}
输出结果为
>0
1111111111110110  这里将-10转换成无符号数即-10的补码，下一步再和3的原码相加再转化成10进制输出
data1+data2=4294967289
```

##### 2.char和short类型，只要参加运算，都会将自己转化为int类型

```c++
char data1 = 'a';
short data2 = 0;
cout << sizeof(data1 + data2) << endl;//4
cout << sizeof(data1 + data1) << endl;//4
cout << sizeof(data2 + data2) << endl;//4
```

##### 3.int和double参加运算，会将int转为double类型

```c++
	int data1 = 10.2;
	double data2 = 10.246;
	cout << sizeof(data1 + data2)  <<endl;//8
	cout << sizeof(data1 + data1)  <<endl;//4
```

#### 2.强制类型转换

无论是自动转换还是强制转换，这种转换都是临时性的，只针对这一行代码有效

```c++
（int)p+1将p强制转化为int类型再加1
（int)(p+1)将p+1强制转化为int类型
```



```c++
float num1 = 4.13f;
int num2 = 0;
num2 = (int)num1;
cout << "num2=" << num2 << endl << "num1=" << num1<<endl;
输出结果为num1=4.13 num2=4
```

### 运算符

#### 1.算术运算符

##### 1.取整

如果/的所有运算对象都是证书 这里的'/'就是做取整运算

```c++
cout << 5 / 2;//2
cout << 5 / 3;//1
```

##### 2.除法运算

如果有一个数是实型，也就是带小数的 这里的'/'就是做除运算

```C++
5/2.0 //2.5
```

##### 3.取余运算符%

判断从键盘输入的数能否被3整除

```c++
int data = 0;
cout << "请输入一个数：";
cin >> data;
if (data % 3 == 0)
{
    cout << "yes";
}
else
{
    cout << "no";
}
}
```

## 随机数

只有rand生成的是***伪随机数***，真随机数需要构造种子，种子也随机

如果rand产生一个随机数>0,请使用rand()产生60-100的随机数

```c++
rand()%41+60 //60到100之间相差40，即取余40+1即可
```

如果rand产生一个随机数>0,请使用rand()产生'a'-'z'的随机字母

```c++
cout<<rand()%26+'a';//这里输出的是ascll码 字母一共26个，0-25所以取余是对25+1
cout<<(char)(rand()%26+'a')<<;这里将ascll码转化为字符
```

真随机数 要先引用time头文件

```c++
#include<time.h>	
srand(time(NULL));//根据时间设置种子随机
cout << rand() << endl;//真正的随机数
```

## 位运算的综合应用

1.data为1字节，将data的第3，4位清零，其他位保持不变

```c++
data = data & 1110 0111; //从第0位开始数
data &= ~(0x01 << 4 | 0x01 << 3);
```

2.data为1字节，将data的第5，6位置为1，其他保持不变  ***0x01保持不变***

```c++
data = data | 0110 0000;
data |= (0x01 << 6 | 0x01 << 5);
```

## 自增自减运算符

### 1.自增自减作为一条独立语句

i++==++i;--i==i-- 加减在哪边并没有什么区别

```c++
int i=3;
i++;
cout<<i; //4
```

### 2.++在右边 先使用，后加减

```c++
int i=3;
int j=0;
j=i++;先使用后加减
cout<<i<<j; //输出i=4,j=3;
```

### 3.++在左边，先加减，后使用

```c++
int i=3;
int j=0;
j=++i;
cout<<i<<j;//输出i=4，j=4
```

## 控制语句

### 1.选择控制语句if

```c++
最简单的
if（条件语句）{
	执行语句
}
```

#### 2.if else

```c++
if(条件语句)
{
	执行语句
}
else
{
执行语句
}
```

#### 3.if 	else if

可以有多个else if语句，也可以不要最后一个else

```c++
if(条件语句)
{
	执行语句
}
else if
{
	执行语句
}
else
{
	执行语句
}
```

### 2.选择控制语句switch 

```c++
int data = 0;
cout << "请输入一个数：";
cin >> data;
if (data < 1 || data>7)
{
    cout << "输入错误，请重新输入！";
    cin >> data;
}
switch (data)
{
case 1:
    cout << "星期一";
    break;//break必须要写，不然会一直往下执行，直到碰到break才跳出
case 2:
    cout << "星期二";
}
}
```

### 3.循环控制for语句

基本语法

```c++
for(初始化语句;循环条件;步进语句)
{
	循环体
}
```

```c++
int s = 0;
int i = 0;
for (i ; i <= 100; i++)
{
    s += i;
}
cout << "s=" << s;//5050
}
```

#### for循环中的break

```c++
int s = 0;
int i = 0;
for (i ; i <= 100; i++)
{
    if (i == 50)
        break;//当i=50时，break语句的作用是跳出当前的整个for循环，break后面的s+=i也不会被执行
    s += i;
}
cout << "s=" << s;//s=1225 只从1加到49
```

#### for循环中的continue

```c++
int s = 0;
int i = 0;
for (i ; i <= 100; i++)
{
    if (i == 50)
        continue;//跳过当前的这次循环，直接进入下一次循环，这一次的s+=i没有被执行
    s += i;
}
cout << "s=" << s;//2=5000相当于少加了一个50；
```

#### for循环嵌套

输出乘法表

```c++
int i = 0;
int j = 0;
for (i=1; i < 10;i++)
{
    for (j=1; j < 10; j++)//(j=1;j<=i;j++)循环语句改成这个就不用后面的if break那两行
    {
        cout << i << "*" << j <<"="<<i*j << "	";
        if (j >=i)
            break;
    }
    cout << endl;
}
```

### 4.while循环控制语句

1-100的和

```c++
int i = 0;
int s = 0;
while (i <= 100)
{
    s += i;
    i++;
}
cout << s;
```

```c++
int i = 0;
int s = 0;
while (i <100)
{
    i++;
    if (i == 50)
        continue;
    s += i;
}
cout << s;
```

## 数组

### 1.一维数组

```c++
int arr[5];//定义一个数组
int i = 0;
for (i = 0; i < 5; i++)
{
    cout << arr[i] << endl ; //遍历输出数组的值，由于数组未初始化，所以是随机值
}
cout << sizeof(arr) << endl;//20（int占4字节，总共5个）输出数组总大小
cout << sizeof(arr[4]) << endl;//4 输出数组中第五个值的大小
```

#### 一维数组的初始化

```c++
int i = 0;
int arr[5] = { 1,2,3,4,5 };//全部初始化
int arr2[] = { 1,2,3,4,5 };//后面的值有多少个数组就有多少个
int arr3[5] = { 1,2,3 };//12300 未被初始化部分自动补0
int arr4[5] = {[1] = 10,[3] = 20};//第二个数组和第四个被初始化，其他为0  语法正确但是编译器会报错
int arr5[5]={0}//建议使用这个，全部初始化为0
```

#### 一维字符数组

```c++
char arr[10] = "hello";
cout << arr<<endl;//hello
char arr2[10] = "hel\0lo";//遇到\0就自动结束
cout << arr2;//hel
```

键盘获取字符串

```c++
char arr[10] = "";
cin >> arr;//这种方法碰到空格或者回车就会中断，无法写入下面的内容
cout << arr << endl;;
char arr2[20] = "";
cin.getline(arr2, sizeof arr2);//可以输入空格
cout << arr2;
```



```c++
//通过键盘输入
int arr[5] = { 0 };
int i = 0;
int n = (sizeof(arr) / sizeof(arr[0]));
cout << "请输入" << n << "个数组值：";
for (i = 0; i < n; i++)
{
    cin >> arr[i];
    cout << endl << "arr["<<i<<"]="<<arr[i] << endl;
}
```

#### 遍历找最大值最小值

```c++
int i = 0;
int arr[5] = { 0 };
int n = sizeof arr / sizeof arr[0];
cout << "请输入" << n << "个数：";
for (i = 0; i < n; i++) {
    cin >> arr[i];
}
int max = arr[0], min = arr[0];//这一步只是将arr[0]的值初始化给max和min，并不会冲突，他们是独立的变量；
for (i = 1; i < n; i++) {
    max = max < arr[i] ? arr[i] : max;
    min = min > arr[i] ? arr[i] : min;
}
cout << "max=" << max << endl << "min=" << min;
```

### 2.二维数组

数组的总大小：sizeof(arr)

```c++
int arr[3][4] = { 0 };//初始化数组
cout << sizeof arr << endl;//输出数组总大小 48字节
cout << sizeof arr / sizeof arr[0]<<endl;//行号
cout << sizeof arr[0] / sizeof arr[0][0];//列号
```



#### 初始化

```c++
int arr[3][4]={{1,2,3,4},{1,2,3,4},{1,2,3,4}};//完全初始化
int arr[][4]={{1,2,3,4},{1,2,3,4},{1,2,3,4}};//完全初始化只可以省略行号，因为系统要知道你有多少列
int arr[3][4]={{1,2},{1,2,3},{12}}//部分初始化，不足的自动补0
int arr[3][4]={1,2,3,4,5,6,7,8,9}//连续初始化，从第一行开始向下，不足的补0
```

```c++
int arr1[3][4]={{1,2},{5,6},{9,10,11}}//这里是部分初始化
int arr2[3][4]={1,2,5,6,9,10,11}//这里是连续初始化
arr1[1][2]+arr2[1][2]=11//索引从0开始[1][2]实际上是指第二行第三列；
```

![image-20230331171528623](C:\Users\15081\AppData\Roaming\Typora\typora-user-images\image-20230331171528623.png)

二维字符数组遍历

```c++
char arr[5][128] = { "hello","world","nihao","hhah" };
int row = 0,low=0;
int i = 0,j=0;
row = sizeof arr / sizeof arr[0];
low = sizeof arr[0] / sizeof arr[0][0];
for (i = 0; i < row; i++) {
    cout << arr[i]<<endl;
}
for (i = 0; i < row; i++) {
    for (j = 0; j < low; j++) {
        cout << arr[i][j] << "	";
    }
}
```



#### 键盘输入操作

```c++
int arr[3][4] = { 0 };
int h = sizeof arr / sizeof arr[0];//行号  
int l = sizeof arr[0] / sizeof arr[0][0];//列号 
int i = 0;
int j = 0;
cout << "请输入" << h*l << "个数：";
for (i = 0; i < h; i++) {
	for (j = 0; j < l; j++) {
		cin >> arr[i][j];
	}
}
for (i = 0; i < h; i++) {
	for (j = 0; j < l; j++) {
		cout<<"arr["<<i<<"]"<<"["<<j<<"]" << arr[i][j] << endl;
	}
}
```

```c++
int arr[5][4] = { {56,75,78,89},{89,98,76,67},{88,99,77,66},{67,78,89,90},{98,97,96,95} };
int h = sizeof arr / sizeof arr[0];
int l = sizeof arr[0] / sizeof arr[0][0];
int i = 0, j = 0;
float avg = 0;
for (i = 0; i < h; i++) {
    int sum = 0;
    for (j = 0; j < l; j++) {
        sum += arr[i][j];
        cout << sum << "	";
    }
    avg = sum / l;
    cout << avg<<endl;

}
```

从键盘输入多个字符串

```c++
char arr[5][128] = { "" };
int row = 0, low = 0, i = 0, j = 0;
row = sizeof arr / sizeof arr[0];
cout << "请输入" << row << "个字符串：";
for (i = 0; i < row; i++) {   //因为这是char类型的字符串，一个字符串就是一行，所以只用一个循环就行了；
    cin >> arr[i];
}
for (i = 0; i < row; i++) {
    cout <<arr[i];
}
```

## 函数

### 数字函数

```c++
void inputarr(int arr[], int n); //前三行都是函数声明，因为这里是先使用后定义，所以要在使用的上面先声明函数存在；
void sortarr(int arr[], int n); //这里的声明直接从定义函数操作那里复制过来
void printarr(int arr[], int n);
void test01() {
	int arr[5] = {0};
	int n = 0;
	n = sizeof arr / sizeof arr[0];
	inputarr(arr, n);
	sortarr(arr, n);
	printarr(arr, n);
}
//函数输入模块
void inputarr(int arr[], int n) {   //这里的(int arr[], int n) 是调用test01函数里的内容；是形参
	int i = 0;
	cout << "请输入" << n << "个数";
	for (i; i < n; i++) {
		cin >> arr[i];
	}
}
//函数排序模块
void sortarr(int arr[], int n) {
	int tmp = 0;
	int i = 0;
	for (i; i < n - 1; i++) {
		int j = 0;
		for (j; j < n - 1; j++) {
			if (arr[j]>arr[j+1]) {
				tmp = arr[j];
				arr[j] = arr[j + 1];
				arr[j + 1] = tmp;
			}
		}
	}
}
//函数输出模块
void printarr(int arr[], int n) {
	int i = 0;
	for (i; i < n; i++) {
		cout << arr[i] << endl;
	}
}
	
int main() {
	test01();
	return 0;
}
```

### 字符函数

```c++
void input(char arr[], int n);
void myarrlen(char arr[]);
void test02() {
	char arr[128];
	int n = sizeof arr;
	input(arr, n);
	myarrlen(arr);
}
void input(char arr[], int n) {
	cout << "请输入一串字符：";
	cin.getline(arr, n);
}
void myarrlen(char arr[]) {
	int i = 0;
	while (arr[i] != '\0') {
		i++;
	}
	cout << i;
}
```

## 预处理

### 1变量的存储

#### 1.1普通局部变量

只作用在最相邻的大括号内

```c++
void test01(){
int a=10;
    {
        int a=20;
    }
}
```

#### 1.2静态局部变量

不初始化内容为0；

作用范围依然是最近的{}内

但是他在整个进程内都有效，只有第一次定义会生效，后面在进行调用都不会再重新定义numl

```c++
void test03() {
	static int num = 10;
	num++;
	cout << num << endl;
}
int main() {
	test03();//11
	test03();//12
	test03();//13
	test03();//14
	test03();//15
	return 0;
}
```



#### 1.3普通全局变量

不初始化内容为0；

作用在整个进程中，全局变量和局部变量同名，优先使用局部变量（就近原则）

其他源文件调用全部变量，必须在开头加进行extern声明

#### 1.4静态全局变量

加static声明；

只能在当前进程使用，不能被调用；

### 2.全局函数和静态函数

全局函数在当前文件或者其他文件都可以使用，在其他文件使用要加extern声明;

静态函数只能在当前文件使用

### 3.头文件包含

```c++
#include<head.h> //包含系统头文件
#include "head.h"//先从当前目录寻找head.h头文件，找不到再去系统中找
```

### 4.define宏

undefine可以取消宏

```c++
#define PI 3.14  宏的名称尽量用大写，结尾不要加分号；
#define MYSTR "helle,world" 
```

带参数的宏

```c++
#define MYNUM(a,b) a*b//这里定义的时候a和b不能定义类型，宏是没有类型的
cout<<MYNUM(5,6)//输出为30
MYNUM（10+20，10+20）//这里的运算过程是10+20*10+20结果是230，因为宏不能保证参数的完整性
#define MYNUM（a,b) ((a)*(b))//这样定义就可以保证参数的完整性了
```

## 指针

在32位系统中，任何指针变量都是4字节大小，而在64位系统中，都是8字节

```c++
int num=10;
int *p;
p=&num;
cout<<*p;
```

指针必须初始化为NULL，不然编译器会报错

```c++
int *p=NULL;
cout << *p;
```

取地址 他这里是倒着存的，从前面开始取但是是从后面开始输出

![image-20230402102327129](C:\Users\15081\AppData\Roaming\Typora\typora-user-images\image-20230402102327129.png)

```c++
int num = 0x01020304;
short* p = (short*)&num;
cout << hex << *(p + 1) << endl;//取出0102的值，以为这里是short类型，占两个字节，从前面开始取，p+1取的就是0102，int是四个字节，char一个
cout << *p;
```

```c++
int num = 0x01020304;
char* p = (char*)&num;
cout <<(int) *(p+1 );//这里取的是03
```

```c++
int num = 0x01020304;
char* p = (char*)&num;
cout <<hex<< *(short*)(p + 1);//这里取的是0203，由于p的类型是char*，跨度是一个字节， 但是要输出两个字节， 下面就强制转换一下成short
```

### 指针注意事项

void不能定义普通变量

```c++
void num;//会报错，因为普通变量的定义需要系统为这个变量开辟空间，而void系统不知道他的大小。所以无法开辟空间
```

```c++
void *p//这是正常的，因为在32位平台上，所有类型的指针类型都为4个字节
但是输出的时候不能直接用*p输出，因为输出的时候编译器无法知道void的长度，可以强制转换一下再输出
```

```c++
void *p=NULL;指针变量初始化为null时，不要去取他的地址
```

```c++
int *p;指针变量未初始化，也不要去进行*p操作
```

```c++
char ch='a';
int *p=&ch;
*P//会报错，因为这里定义的*p是int类型，占有四个字节，而ch为一个字节的内容，属于越界访问；
```

```c++
int num=10;
int *p=&num;
p++;
*p;//这种行为也属于越界操作，会取到下一个四字节的内容
```

### 1.数字指针

```c++
int arr[]={10,20,30,40,50};
int *p;
p=arr;//取的第0个元素地址;等同于：p=&arr[0]
p=&arr[0];
p=&arr[3];
```

#### 数字指针的遍历

```c++
int arr[] = { 1,2,3,4,5 };
int n = sizeof arr / sizeof arr[0];
int* p = arr;
int i = 0;
for (i; i < n; i++)
{
    cout << *(p+i)<<"	"; //三种方式都可以遍历指针
    cout << *(arr + i);
    cout << arr[i] << "  ";
}
```

### 2.字符串指针

#### 1.字符数组

```c++
char str1[128]="hello world";
```

str1是数组，开辟128字节，存放在栈区/全局区 ，可读可写

2.字符串指针变量

```c++
const char *str2="hello world";
sizeof (str2);//输出结果时候4，也就是指针的大小，并不是字符串的大小
cout<<str2;//hello world
cout<<str2[3];//l
str2[3]='a'//error 会报错，文字常量区不可写入
```

str2存放的是"hello world"的首元素地址，并不是整个字符串的内容

开辟的指针变量占有四个字节，在栈区/全局区而后面的字符串存放在文字常量区，只可读不可写

### 3.指针数组

本质是数组，只是数组的每个元素为指针

```c++
char *arr1[4];
short *arr2[4];
int *arr3[4];
sizeof arr1;//32  因为存的是指针，32位平台上一个指针的大小是4字节，而64位平台是8字节；
sizeof arr2;//32
sizeof arr3;//32
```

```c++
int num1 = 10, num2 = 20, num3 = 30;
int* arr[] = { &num1,&num2,&num3 };
cout << *arr[0];//10
```

### 4.字符指针数组

原理还是存的字符串的地址，字符串存放在文字常量区，可读不可写

```c++
const char* arr[4] = { "hehheheh","lalallala","xxxxxiixiisi","nanannanaa" };
int n = sizeof arr / sizeof arr[0];
int i = 0;
for (i; i < n; i++)
{
    cout << arr[i] << "  ";//这里不加*号输出的是整个字符串的内容，加了*号是输出每个字符串的首元素内容
    cout<<*(arr[i]+2);//这样输出的就是每个字符串的第三个元素
    cout
}  
```

### 5.二维字符数组

一维字符数组保存的是字符串的首元素地址，而二维字符数组保存的是字符串本身，也就是都存在全局区/栈区里，可读可写

### 6.指针的指针

一级指针可以保存0级指针的地址，二级指针可以保存1级指针的地址

![image-20230404102651179](C:\Users\15081\AppData\Roaming\Typora\typora-user-images\image-20230404102651179.png)

### 7.数组指针

```c++
int arr[5] = { 10,20,30,40,50 };
cout << arr<<endl;
cout << arr + 1;
结果为：000000BFB26FF498  相差四个字节
        000000BFB26FF49C
```

```c++
int arr[5] = { 10,20,30,40,50 };
cout << &arr<<endl;
cout << &arr + 1;
00000093CF2FF508 //进行取地址操作后，跳过的是整个数组，也就是4*5字节
00000093CF2FF51C
```

###      8.二维数组与数组指针的关系

![image-20230405104735250](C:\Users\15081\AppData\Roaming\Typora\typora-user-images\image-20230405104735250.png)

二维数组用一维去遍历

```c++
int arr[3][4] = { {1,2,3},{4,5,6},{7,8,9} };
int row = sizeof arr / sizeof arr[0];
int low = sizeof arr[0] / sizeof arr[0][0];
int i = 0;
int* p = &arr[0][0];
for (i; i < row*low; i++)
{
    cout << (p[i]) << "  ";
}
```

### 9.指针与函数

一维数组作为函数的形参会被优化成指针

```c++
void test10() {
	int arr[5] = { 10,20,30,40,50 };
	int n = sizeof arr / sizeof arr[0]; 
	cout << "外部变量：" << sizeof arr<<endl;//8   64位平台上输出8，32位输出为4  这里数组作为函数的形参被编译器自动优化为了指针，所有类型的指针在32位平台都是4位，64位平台占8位
	inputarr(arr,n);//这是实参
}
void inputarr(int* arr, int n) { //这是形参
	cout << "内部变量：" << sizeof arr<<endl;//20 这里输出的是实际数字的大小
	int i = 0;
	for (i; i < n; i++)
	{
		cout << arr[i]<<"  ";
	}
	cout << endl;
```

### 10.函数返回值为指针类型

```c++
int *getaddr(void) {
	static int num = 100;//修饰为静态变量，声明周期为整个进程，否则num变量在这个大括号运行结束就被释放了，无法被获取到
	return &num;
}
void test11() {
	int* p = NULL;
	p = getaddr();//函数返回值为指针类型，返回的地址不能是普通局部变量的地址，会报错，可以在变量前加static修饰
	cout << "*p=" << *p;
}
```

### 11.函数指针

本质是一个指针变量，存放的是函数的入口地址

```c++
int myadd(int x, int y)
{
	return x + y;
}
void test12() {
	int (*p)(int x, int y) = NULL;
	p = myadd;
	cout << sizeof p<< endl ;
	cout << p(10, 20);
}
```

函数指针作为函数参数

```c++
int myadd(int x, int y) {
	return x + y;
}
int mysub(int x, int y) {
	return x - y;
}
int mymul (int x, int y) {
	return x * y;
}
int mydiv(int x, int y) {
	return x / y;
}
int mycale(int x, int y, int(*func)(int, int)) {
	return func(x, y);
}
void test12() {
	cout << mycale(10, 20, myadd) << endl;
	cout << mycale(10, 20, mysub) << endl;
	cout << mycale(10, 20, mymul) << endl;
	cout << mycale(10, 20, mydiv) << endl;
```

## 动态空间申请

### new和delete

#### new和delete操作基本类型空间

new 申请空间，delete释放空间，这两个函数一般成双出现

```c++
void test01() {
	int* p = NULL;
	p = new int;//从堆区申请int类型大小的空间
	*p = 100;
	cout << "*p=" << *p << endl;
	delete p;
}
void test02() {
	int* p = NULL;
	p = new int(100);//申请int大小空间的同时进行初始化；
	cout << "*p=" << *p << endl;
	delete p;
}
```

#### new和delete操作数组空间

如果new后面有[ ]那么delete后也要跟[ ]

```c++
int* arr = NULL;
arr = new int[5] {10, 20, 30, 40, 50};//申请空间的同时进行数组初始化
int i = 0;
for (i; i < 5; i++) {
    cout << arr[i] << "  ";
}
delete[] arr;
```

## 字符串处理函数

### 1.字符串操作函数

需要写入头文件#include<string.h>  遇到'\0'自动结束操作

#### 1.测量字符串长度strlen

```c++
char str[128] = "hello";
cout << strlen(str);//  5
char str2[128] = "he\0llo";
cout << strlen(str2);//  2
```

#### 2.拷贝字符串ctrcpy

```c++
#include<string.h>
char *strcpy(char *dest，const char *src);
char *strncpy(char *dest，const char *src，size_t n);//这里的n表示拷贝的字符长度
src:原字符串的首元素地址
dest:目的空间地址  //这里的目的空间地址一定要足够字符串的写入，否则会造成内存溢出（内存污染）
```

```c++
char str1[128] = "hello world";
char str2[128] = "";
char str3[128] = "";
strcpy_s(str2, str1);
strncpy_s(str3, str1, 4);
cout << str2<<endl;//hello world
cout << str3;//hell
```

#### 3.字符串追加strcat

追加到字符串尾部

```c++
char str1[128]="";
char str2[] = "hello";
strcat_s(str1, "nihao");
strcat_s(str1, str2);
cout << str1;// nihaohello
```

#### 4.字符串比较strcmp

**逐字符串比较ascll大小**除非字符一直一样，否则在第一个不一样的上就能决定谁大谁小，与字符串长度无关

如输出>0则str1>str2

如输出<0则str1<str2

如输出=0则str1=str2

```c++
char str1[128] = "hello";
char str2[128] = "hellosw";
cout << strcmp(str1, str2);
```

## 结构体 struct

### 1.结构体的定义

#### struct定义结构体

```c++
struct  student //这里的student就是结构体名称
{
    int num;//定义结构体过程中不能给变量初始化
    char name[128];
    int age;
};//结尾有分号
student lucy;//	定义一个lucy变量，结构体基础是student
student tom;//定义一个tom变量
```

```c++
struct student
{
	int num;
	char name[128];
};
void test01() {
	student lucy = { 110,"chen" };//初始化结构体的值
	cout << lucy.num << lucy.name;
}
```

#### memset清空结构体

```c++
student lucy;
memset (&lucy, 0, sizeof(lucy)); //将从lucy地址开始，长度为lucy字节全部赋值为0
cout << lucy.num << lucy.name;
```

#### 用键盘给结构体变量赋值

```c++
student lucy ;
cin >> lucy.num >> lucy.name;
cout << lucy.num << endl << lucy.name << endl;
```

#### 单独操作结构体中的成员

```c++
student lucy = { 001,"tom" };
lucy.num += 100;
lucy.name = "tom";//这一行代码会报错，因为name成员是一个数组，该字符串字面值被翻译器作为常量处理并储存于只读内存区域，修改方法可以使用 C 库函数 strcpy() 复制一份可读可写字符串，并让结构体成员指向它。
strcpy_s(lucy.name, "tom");
cout << lucy.num << lucy.name;// 101 tom
```

#### 相同类型结构体变量的赋值

```c++
student lucy = { 001,"tomjack" };
strcpy_s(lucy.name, "tom");//通过复制的方式赋值
student bob = lucy; //直接赋值
memcpy(&bob,&lucy,sizeof(student));//通过内存拷贝进行赋值
cout << bob.num << endl << bob.name;
```

#### 结构体嵌套结构体

```c++
struct ob
{
	int age;
	int score;
};
struct student
{
	int num;
	char name[128];
	ob data;
};
student lucy = { 001,"lucy.tom" ,{18,99} };
cout << lucy.num << endl << lucy.name << endl << lucy.data.age << endl << lucy.data.score << endl;
}
```

### 2.结构体数组

#### 结构体数组的定义

```c++
student arr[5] = { {100,"tom"},{101,"jack"},{102,"lucy"},{103,"xiaoming"},{104,"jerry"} };
int n = sizeof arr / sizeof arr[0];
int i = 0;
for (i; i < n; i++) {
    cout << arr[i].num << "  " << arr[i].name;
    cout << endl;
}
```

#### 结构体数组从键盘输入

```c++
student arr[3] = {};
memset(arr, 0, sizeof(arr));
int n = sizeof arr / sizeof arr[0];
int i = 0;
for (i; i < n; i++) {
	cout << "请输入第" << i << "组数据：";
	cin >> arr[i].num >> arr[i].name;
}
for (i=0; i < n; i++) {//这里很关键，一定要重新初始化i=0，否则上面的遍历结束i的值已经为4了，就不会执行下面的循环了
	cout << arr[i].num << "  " << arr[i].name;
	cout << endl;
}
```

### 3.结构体指针变量

```c++
student lucy = { 101,"jerry" };
student* p = &lucy;
//*p==lucy
cout << lucy.num << "  " << lucy.name<<endl;// 如果是结构体变量只能用.去访问，如果是地址则可以用->去访问
cout << (*p).num << "  " << (*p).name << endl;
cout << p->num << "  " << p->name << endl;
cout << (&lucy)->num << "  " << (&lucy)->name << endl;
```

```c++
void inputarr(student* arr, int n) {
	cout << "请输入" << n << "组数据：";
	int i = 0;
	for (i = 0; i < n; i++) {
		cin >> arr[i].num >> arr[i].name;
	}
}
void sortarr(student* arr, int n) {
	int i = 0;
	for (i = 0; i < n; i++) {
		int j = 0;
		for (j = 0; j < n-1; j++) {//这里j<n-1是因为最后一次执行内部循环时 j 的值等于 n -1，则将尝试访问超出数组上限的第 n 个元素：arr[j+1] 会导致程序运行错误。所以应该将内部循环结束条件更改为 j<n-1。
			if (arr[j].num > arr[j + 1].num) {
				student tmp;
				tmp = arr[j];
				arr[j ] = arr[j + 1];
				arr[j + 1] = tmp;
			}
		}
	}
}
void output(student* arr, int n) {
	int i = 0;
	for (i = 0; i < n; i++) {
		cout << arr[i].num << "  " << arr[i].name<<endl;
	}
}
void test09() {
	student arr[3];
	memset(arr, 0, sizeof arr);
	int n = sizeof(arr) / sizeof(arr[0]);
	inputarr(arr,n);//输入
	sortarr(arr,n);//排序
	output(arr, n);//输出
```

#### 结构体指针成员的定义

```c++ 
struct project
{
	int num;
	const char* name;
};
void test11() {
	project lucy = { 101,"hello,world" }; //这里的helloworld存放在文字常量区，lucy.name存放的只是字符串的首地址
}
```

#### 结构体指针成员指向堆区

```c++
struct project
{
	int num;
	char* name;
};
void test11() {
	project lucy;
	lucy.num = 101;
	lucy.name = new char[32];
	strcpy(lucy.name, "hello");//通过拷贝的方式给lucy.name赋值，这样值就不会存在文字常量区了
    delete [] lucy.name;
}
```



### 4.结构体的深拷贝和浅拷贝

#### 浅拷贝

将结构体空间变量内容赋值一份到相同类型的结构体变量空间中，如果结构体中没用指针成员，那么就不会有什么问题，一旦结构体中存在指针成员，就会导致堆区空间重复释放的问题 

```c++
struct project
{
	int num;
	char* name;
};
void test11() {
	project lucy;
	lucy.num = 101;
	lucy.name = new char[32];
	strcpy(lucy.name, "hello");//通过拷贝的方式给lucy.name赋值，这样值就不会存在文字常量区了
    project bob;
    bob=lucy;  //bob.num=100;bob.name==lucy.name的地址，浅拷贝只是把地址拷贝过去了，并没有把真正的内容复制过去
    delete [] lucy.name;
    delete [] bob.name;//这里的浅拷贝就会导致堆区空间的重复释放
}
```

![image-20230412184037985](C:\Users\15081\AppData\Roaming\Typora\typora-user-images\image-20230412184037985.png)

#### 深拷贝

如果结构体中存在指针成员，尽量使用深拷贝，深拷贝就是为指针成员分配独立空间，然后再内容拷贝

```c++
struct project
{
	int num;
	char* name;
};
void test11() {
	project lucy;
	lucy.num = 101;
	lucy.name = new char[32];
	strcpy(lucy.name, "hello");//通过拷贝的方式给lucy.name赋值，这样值就不会存在文字常量区了
    project bob;
	bob.num=lucy.num;
    bob.name=new char[32];//为bob.name开辟独立的空间
    strcpy(bob.name,lucy.name)//这样就是深拷贝   
    delete [] lucy.name;
    delete [] bob.name;//这里的浅拷贝就不会导致堆区空间的重复释放
}
```

### 5.结构体变量在堆区，结构体指针成员也指向堆区

```c++
struct project
{
	int num;
	char* name;
};
void test12() {
	project* p = new project;//为p在堆区中开辟一个结构体
	p->num = 100;//因为这里的p.num是一个地址所以要那样写，或者写成(*p).num
	p->name = new char[32];
	strcpy(p->name, "hello");
	cout << p->num << "  " << p->name;
	delete [] p->name;
	delete p;
}
```

### 6.结构体的对齐规则

#### 自动对齐规则

两个成员之间空出来的位置不能用，只能用空的

![image-20230412192523828](C:\Users\15081\AppData\Roaming\Typora\typora-user-images\image-20230412192523828.png)

![image-20230412192549682](C:\Users\15081\AppData\Roaming\Typora\typora-user-images\image-20230412192549682.png)

#### 强制对齐规则

![image-20230412192930336](C:\Users\15081\AppData\Roaming\Typora\typora-user-images\image-20230412192930336.png) 	![image-20230412193020146](C:\Users\15081\AppData\Roaming\Typora\typora-user-images\image-20230412193020146.png)

### 7.结构体的位域

## 共用体 union

结构体：所有成员拥有独立空间

共用体：所有成员公用同一块空间

共用体空间大小由最大的成员类型决定

```c++
union mydata
{
	char name;
	int num;
	short age;
};
void test01() {
	mydata lucy;
	cout << sizeof(lucy);//4 在这个共用体中，int类型最大，那么这个共用体的大小就为4字节；
}
```

虽然共用体的空间是最大的类型决定的，但是成员还是只能操作他自己类型大小的空间；倒着存倒着取

![image-20230412202517902](C:\Users\15081\AppData\Roaming\Typora\typora-user-images\image-20230412202517902.png)

## 枚举enum

```c++
enum phonecolor
{
	pink,red,black,blue
};
void test02() {
	phonecolor ob = pink;//这里的赋值只能写上面已经枚举了的数据
	ob = 100//赋值以枚举以外的数据会报错
```

修改枚举的值

```c++
enum phonecolor
{
	pink,red=10,black,blue
};
void test02() {
	phonecolor ob = pink;
    cout << pink << red << black << blue;//0 11 12 13
```

## 链表

