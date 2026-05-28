---
title: C++提高编程
date: 2022-03-08 14:15:28
categories:
  - C++
tags:
  - C++
---

[](#C-八股文的一些个人总结)C++八股文的一些个人总结
## 一、内存相关
C++程序在执行时，将内存大方向划分为**4个区域**



- **代码区**：存放函数体的二进制代码，由操作系统进行管理的

- **全局区**：存放全局变量和静态变量以及常量

- **栈区**：由编译器自动分配释放, 存放函数的参数值,局部变量等

- **堆区**：由程序员分配和释放,若程序员不释放,程序结束时由操作系统回收



C++中在**程序运行前**分为全局区和代码区



- 代码区特点是共享和只读

全局区中存放全局变量、静态变量(static)、常量
常量中存放 const修饰的全局变量(全局常量) 和 字符串常量






在**程序运行后**分为栈区和堆区



- 栈区由编译器自动分配释放, 存放函数的参数值,局部变量等，🧐注意事项：不要返回局部变量的地址，栈区开辟的数据由编译器自动释放

堆区数据由程序员管理开辟和释放，堆区数据利用new关键字进行开辟内存，析构代码将堆区开辟数据做释放操作
[](#深拷贝与浅拷贝)深拷贝与浅拷贝
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
class Person&#123;
public:
Person()&#123;
cout"Person的默认构造函数调用"
&#125;
Person(int age,int height)&#123;

m_Age = age;
m_Height = new int(height);
cout"Person的有参构造函数调用"
&#125;
//要利用深拷贝来解决：自己实现拷贝构造函数，解决浅拷贝带来的问题
Person(const Person &p)&#123;
cout"Person拷贝构造函数的调用"
m_Age = p.m_Age;
//m_Height = p.m_Height;编译器默认实现（浅拷贝）
//深拷贝操作
m_Height =  new int(*p.m_Height);//在堆区重新创建一块内存
&#125;
~Person()&#123;
if(m_Height!=NULL)&#123;
delete m_Height;
m_Height = NULL;
&#125;
cout"Person的析构函数调用"
&#125;

int m_Age;
int *m_Height;
&#125;;

void test01()&#123;
Person p1(18,180);
cout"p1的年龄为：""身高为："
Person p2(p1);
cout"p2的年龄为：""身高为："
//栈的后进先出原则，先析构p2再析构p1，导致同一内存地址释放两次
//浅拷贝带来的问题：堆区的内存重复释放
//要利用深拷贝来解决：自己实现拷贝构造函数，解决浅拷贝带来的问题
&#125;

int main() &#123;

test01();

return 0;
&#125;





[](#内存对齐)内存对齐
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
#include 
using namespace std;

#pragma pack(show)//对齐模数默认是8，编译后查看
//#pragma pack(1)// 对齐模数可以修改为2的n次方

//自定义数据类型 内存对齐规则如下
//1.第一个属性开始，从偏移量0位置开始存储
//2.第二个属性开始，放在min(该类型大小，对齐模数「默认是8」）的整数倍上
//3.整体计算完毕后，做二次对齐，结构体大小必须是min(该结构体中最大数据类型，对齐模数)的整数倍上，不足补齐
struct Student&#123;
int a;  //0~3
char b; //4~7
double c;//8~15
float d; //16~19
&#125;;

//2、结构体嵌套结构体
struct Student2&#123;
char a;     //0~7
struct Student b;//8~31
double c;   //32~39
&#125;;
void test01()&#123;
cout"sizeof = "sizeof(struct Student)//24
cout"sizeof = "sizeof(struct Student2)//40
&#125;
int main() &#123;

test01();

return 0;
&#125;





## 二、代码相关
[](#关于static关键字)关于static关键字

- static修饰普通变量



- 该静态变量存储在全局区，在程序运行前就生成了，如果没有初始值，默认为0




- static修饰普通函数



- 该函数作用范围仅限当前文件内，防止多文件环境下与其他文件内函数重名




- static修饰成员变量



- 该成员变量全局唯一一份，与实例化对象创建的个数无关。该成员变量无需实例化对象就可以进行访问




- static修饰成员函数



- 该函数无需实例化对象就可以进行访问，函数不能访问非静态变量




[](#关于const关键字)关于const关键字

const修饰指针 常量指针 
1
2
3
4
5
6
7
int a = 10;
int b = 10;

const int * p1 = &a;
//指针指向的值不可以修改，指针的指向可以修改
//*p1 = 20;错误
p1 = &b;//正确





const修饰常量 指针常量
1
2
3
int * const p2 = &a;
*p2 = 100;//正确，指针指向的值可以修改
//p2 = &b;错误，指针的指向不可以修改





const修饰指针和常量
1
2
3
4
const int * const p3 = &a;
//指针指向的值和指针的指向都不可以修改
//*p3 = 100;错误
//p3 = &b;错误



*技巧：关注const后面紧跟的事指针还是常量，是指针就是常量指针，是常量就是指针常量。*






- 在函数形参列表中，可以加==**const**修饰形参==，防止形参改变实参



1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
//引用使用的场景，通常用来修饰形参
void showValue(const int& v) &#123;
//v += 10;
cout 
&#125;

int main() &#123;

//int& ref = 10;  引用本身需要一个合法的内存空间，因此这行错误
//加入const就可以了，编译器优化代码，int temp = 10; const int& ref = temp;
const int& ref = 10;

//ref = 100;  //加入const后不可以修改变量
cout 

//函数中利用常量引用防止误操作修改实参
int a = 10;
showValue(a);

system("pause");

return 0;
&#125;




**const修饰成员函数**



常函数：



- 成员函数后加const后我们称为这个函数为常函数

- 常函数内不可以修改成员属性

- 成员属性声明时加关键字mutable后，在常函数中依然可以修改



常对象：



- 声明对象前加const称该对象为常对象

- 常对象只能调用常函数







1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
class Person &#123;
public:
Person() &#123;
m_A = 0;
m_B = 0;
&#125;

//this指针的本质是一个指针常量(Person * const this)指针的指向不可修改
//如果想让指针指向的值也不可以修改，需要声明常函数,即添加const
//相当于变成const Person * const this
void ShowPerson() const &#123;
//const Type* const pointer;
//this = NULL; //不能修改指针的指向 Person* const this;
//this->mA = 100; //但是this指针指向的对象的数据是可以修改的

//const修饰成员函数，表示指针指向的内存空间的数据不能修改，除了mutable修饰的变量
this->m_B = 100;
&#125;

void MyFunc() const &#123;
//mA = 10000;
&#125;

public:
int m_A;
mutable int m_B; //可修改 可变的
&#125;;

//const修饰对象  常对象
void test01() &#123;

const Person person; //常量对象
cout 
//person.mA = 100; //常对象不能修改成员变量的值,但是可以访问
person.m_B = 100; //但常对象可以修改mutable修饰成员变量

//常对象访问成员函数
person.MyFunc(); //常对象能调用常函数

&#125;

int main() &#123;

test01();

return 0;



- 因为this指针的本质是一个**指针常量**（Person * const this)指针的指向不可修改

- 所以加const后相当于变成**常量指针常量**(const Person * const this)值也不可以修改了


[](#构造函数调用规则如下：)**构造函数**调用规则如下：   如果用户定义有参构造函数，c++不在提供默认无参构造，但是会提供默认拷贝构造


如果用户定义拷贝构造函数，c++不会再提供其他构造函数


示例：



1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
52
53
54
55
class Person &#123;
public:
//无参（默认）构造函数
Person() &#123;
cout "无参构造函数!" 
&#125;
//有参构造函数
Person(int a) &#123;
age = a;
cout "有参构造函数!" 
&#125;
//拷贝构造函数
Person(const Person& p) &#123;
age = p.age;
cout "拷贝构造函数!" 
&#125;
//析构函数
~Person() &#123;
cout "析构函数!" 
&#125;
public:
int age;
&#125;;

void test01()
&#123;
Person p1(18);
//如果不写拷贝构造，编译器会自动添加拷贝构造，并且做浅拷贝操作
Person p2(p1);

cout "p2的年龄为： " 
&#125;

void test02()
&#123;
//如果用户提供有参构造，编译器不会提供默认构造，会提供拷贝构造
Person p1; //此时如果用户自己没有提供默认构造，会出错
Person p2(10); //用户提供的有参
Person p3(p2); //此时如果用户没有提供拷贝构造，编译器会提供

//如果用户提供拷贝构造，编译器不会提供其他构造函数
Person p4; //此时如果用户自己没有提供默认构造，会出错
Person p5(10); //此时如果用户自己没有提供有参，会出错
Person p6(p5); //用户自己提供拷贝构造
&#125;

int main() &#123;

test01();

system("pause");

return 0;
&#125;




## 拷贝构造函数调用时机
C++中拷贝构造函数调用时机通常有三种情况



- 使用一个已经创建完毕的对象来初始化一个新对象

- 值传递的方式给函数参数传值

- 以值方式返回局部对象



1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
42
43
44
45
46
47
48
49
50
51
52
53
54
55
56
57
58
59
60
61
62
63
64
65
66
class Person &#123;
public:
Person() &#123;
cout "无参构造函数!" 
mAge = 0;
&#125;
Person(int age) &#123;
cout "有参构造函数!" 
mAge = age;
&#125;
Person(const Person& p) &#123;
cout "拷贝构造函数!" 
mAge = p.mAge;
&#125;
//析构函数在释放内存之前调用
~Person() &#123;
cout "析构函数!" 
&#125;
public:
int mAge;
&#125;;

//1. 使用一个已经创建完毕的对象来初始化一个新对象
void test01() &#123;

Person man(100); //p对象已经创建完毕
Person newman(man); //调用拷贝构造函数
Person newman2 = man; //拷贝构造

//Person newman3;
//newman3 = man; //不是调用拷贝构造函数，赋值操作
&#125;

//2. 值传递的方式给函数参数传值
//相当于Person p1 = p;
void doWork(Person p1) &#123;&#125;
void test02() &#123;
Person p; //无参构造函数
doWork(p);
&#125;

//3. 以值方式返回局部对象
Person doWork2()
&#123;
Person p1;
cout int *)&p1 
return p1;
&#125;

void test03()
&#123;
Person p = doWork2();
cout int *)&p 
&#125;

int main() &#123;

//test01();
//test02();
test03();

system("pause");

return 0;
&#125;




## 从this指针理解拷贝构造函数

this指针指向被调用的成员函数所属的对象



this指针是隐含每一个非静态成员**函数**内的一种指针



this指针不需要定义，直接使用即可



this指针的用途：


当形参和成员变量同名时，可用this指针来区分


在类的非静态成员函数中返回对象本身，可使用return *this





1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
23
24
25
26
27
28
29
30
31
32
33
34
35
36
37
38
39
40
41
class Person
&#123;
public:

Person(int age)
&#123;
//1、当形参和成员变量同名时，可用this指针来区分
this->age = age;
&#125;

Person& PersonAddPerson(Person p)
//若不加&(返回引用改为返回值)
// 相当于 值方式返回局部对象 不会返回传进来的对象，而是调用拷贝构造函数，拷贝一个新的对象返回
// 这样链式编程后的结果p2的值为20而不是40，因为每次返回都是一个新的p2
&#123;
this->age += p.age;
//返回对象本身
return *this;
&#125;

int age;
&#125;;

void test01()
&#123;
Person p1(10);
cout "p1.age = " 

Person p2(10);
//链式编程
p2.PersonAddPerson(p1).PersonAddPerson(p1).PersonAddPerson(p1);
cout "p2.age = " 
&#125;

int main() &#123;

test01();

return 0;
&#125;























赏




好活当赏！




![](/assets/paywechat.jpg)
微信















**本文作者：**
Alituofo


**本文链接：**
[http://alituofo.github.io/2022/03/08/C++八股文/](http://alituofo.github.io/2022/03/08/C++八股文/)



**版权声明： **

本博客所有文章除特别声明外，均采用 [MIT](https://github.com/alituofo/hexo-theme-yilia-plus/blob/master/LICENSE) 许可协议。转载请注明出处！
