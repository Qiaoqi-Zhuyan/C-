# C++语法

## 1、利用 `const` 替换 `#define`

替换 `define` 为 `const` 

~~~~~C++
const double pi = 3.1416;
~~~~~

只读这个变量，定义的时候必须初始化

跨文件的时候

~~~~~C++
extern const double PI;
~~~~~

~~~~C++
const int *p//这是个常量指针
p = &J;//指向的内容是常量，指向的类型严格一致。
~~~~



~~~~~C++
void f(const int x){
  
}
//函数体不能对这个参数做修改
~~~~~

## 2、inline 内联函数

相当于宏，为了避免函数体被调用的时候重复的进行展开，用inline 定义函数体相当于用`#define` 定义了一个函数体

~~~~C++
inline int max(int a,int b){
  return a>b?a:b;
}

int main(){
	int a = 10;
  for(int i = 0;i<=20;i++){
    cout<<max(i,a)<<endl;//这里的max就不会被重复调用而是会替换成inline 后面的这个max函数体
  }  
}
~~~~

目的是为了节省内存的开销

## 3、函数重载

同一个函数名，但是输入的参数不同，执行的函数也不同

**多个函数拥有相同的名字，只要它们的参数列表不同**

~~~~C++
#include <iostream>
using namespace std;
//交换 int 变量的值
void Swap(int *a, int *b){
    int temp = *a;
    *a = *b;
    *b = temp;
}
//交换 float 变量的值
void Swap(float *a, float *b){
    float temp = *a;
    *a = *b;
    *b = temp;
}
//交换 char 变量的值
void Swap(char *a, char *b){
    char temp = *a;
    *a = *b;
    *b = temp;
}
//交换 bool 变量的值
void Swap(bool *a, bool *b){
    char temp = *a;
    *a = *b;
    *b = temp;
}
int main(){
    //交换 int 变量的值
    int n1 = 100, n2 = 200;
    Swap(&n1, &n2);
    cout<<n1<<", "<<n2<<endl;
   
    //交换 float 变量的值
    float f1 = 12.5, f2 = 56.93;
    Swap(&f1, &f2);
    cout<<f1<<", "<<f2<<endl;
   
    //交换 char 变量的值
    char c1 = 'A', c2 = 'B';
    Swap(&c1, &c2);
    cout<<c1<<", "<<c2<<endl;
   
    //交换 bool 变量的值
    bool b1 = false, b2 = true;
    Swap(&b1, &b2);
    cout<<b1<<", "<<b2<<endl;
    return 0;
}
~~~~

匹配的原则

- 1.精确匹配，包括实参类型和形参类型相同，实参从数组或函数转换成对应的指针类型，向实参添加顶层const或从实参删除顶层const
- 2.通过const转换实现的匹配
- 3.通过类型提升实现的匹配
- 4.通过算数类型转换实现的匹配
- 5.通过类类型转换实现的匹配

## 4、默认参数

定义函数时可以给形参指定一个默认的值，这样调用函数时如果没有给这个形参赋值，那么就使用这个默认的值。

~~~~C++
void func(int n, float b=1.2, char c='@'){
    cout<<n<<", "<<b<<", "<<c<<endl;
}
int main(){
    //为所有参数传值
    func(10, 3.5, '#');//输出：10 3.5 #
    //为n、b传值，相当于调用func(20, 9.8, '@')
    func(20, 9.8);//输出：20 9.8 @
    //只为n传值，相当于调用func(30, 1.2, '@')
    func(30);//输出：30 1.2 @
    return 0;
}
~~~~

C++规定，默认参数只能放在形参列表的最后，而且一旦为某个形参指定了默认值，那么它后面的所有形参都必须有默认值。实参和形参的传值是从左到右依次匹配的，默认参数的连续性是保证正确传参的前提

也就是说，默认参数必须是在形参的右边，到时候匹配的时候是从左到右的顺序匹配的。

这两个是不被允许的

~~~~C++
void func(int a, int b=10, int c=20, int d){ }
void func(int a, int b=10, int c, int d=20){ }
~~~~

## 5 、引用

引用可以看做是数据的一个别名，通过这个别名和原来的名字都能够找到这份数据。

引用还类似于人的绰号（笔名），使用绰号（笔名）和本名都能表示一个人。

定义方法

~~~~C++
int a = 100;
int &r = a;
~~~~

也就是

~~~~C++
变量名 &名称 = 数据；
~~~~

**引用必须在定义的同时初始化**

并且以后也要从一而终，不能再引用其它数据，这有点类似于常量（const 变量）

注意，引用在定义时需要添加`&`，在使用时不能添加`&`，使用时添加`&`表示取地址。

例子：

### 5.1 引用

~~~~C++

int main() {
    int a = 99;
    int &r = a;
    cout << a << ", " << r << endl;
    cout << &a << ", " << &r << endl;
    return 0;
}

~~~~

运行结果：
99, 99
0x28ff44, 0x28ff44

### 5.2 引用修改原始变量所储存的数据

~~~~C++
int main() {
    int a = 99;
    int &r = a;
    r = 47;
    cout << a << ", " << r << endl;
    return 0;
~~~~

运行结果：

47，47

### 5.3 不改变数据

~~~~C++
const 数据类型 &名称 = 数据；
~~~~

### 5.4 引用作为函数的参数

1. 可以实现在函数内部影响函数外部数据的效果
2. 和指针一样，传递数据，传递数组

例子：

~~~~C++
void swap1(int a, int b);
void swap2(int *p1, int *p2);
void swap3(int &r1, int &r2);
int main() {
    int num1, num2;
    cout << "Input two integers: ";
    cin >> num1 >> num2;
    swap1(num1, num2);
    cout << num1 << " " << num2 << endl;
    cout << "Input two integers: ";
    cin >> num1 >> num2;
    swap2(&num1, &num2);
    cout << num1 << " " << num2 << endl;
    cout << "Input two integers: ";
    cin >> num1 >> num2;
    swap3(num1, num2);
    cout << num1 << " " << num2 << endl;
    return 0;
}
//直接传递参数内容
void swap1(int a, int b) {
    int temp = a;
    a = b;
    b = temp;
}
//传递指针
void swap2(int *p1, int *p2) {
    int temp = *p1;
    *p1 = *p2;
    *p2 = temp;
}
//按引用传参
void swap3(int &r1, int &r2) {
    int temp = r1;
    r1 = r2;
    r2 = temp;
}
~~~~

运行结果：

Input two integers: 12 34
12 34
Input two integers: 88 99
99 88
Input two integers: 100 200
200 100

**使用引用代替指针**

## 6、new delete

替换 `malloc` 为 `new` 

### 6.1 申请空间

~~~~C++
int *p_0 = new int;//分配一个int星变量所占大小的空间
int *p_1 = new int[10]；//分配一个有10个int型元素的数组所占空间，并将该数组的第一个元素的地址赋给int 型指针p_1;相当于就是申请一个一维数组
int *p_2 = new int(10);//分配一个int型变量所占大小的空间，在其中放入十进制数10，并将首地址赋给int 型指针p
~~~~

### 6.2 动态二维数组

​	1、

~~~~C++
int** arr = new int*[col];
for(int i = 0;i<col;i++)
  arr[i] = new int[row];

for(int i = 0;i<col;i++)
  delete[] arr[i];
delete arr;
~~~~

​	2、

~~~~C++
int (*p)[10] = new int[3][10];
~~~~

## 7、名字空间

为了解决变量，类等名称冲突等问题，使用了名字空间

~~~~C++
namespace Li{  //小李的变量定义
    FILE* fp = NULL;
}
namespace Han{  //小韩的变量定义
    FILE* fp = NULL;
}
~~~~

~~~~c++
namespace name{
  //可以包含变量、类、函数、typedef、#define等
}
~~~~

使用：

​	1、用域解析操作符

~~~~C++
Li::fp = fopen("one.txt", "r");  //使用小李定义的变量 fp
Han::fp = fopen("two.txt", "rb+");  //使用小韩定义的变量 fp
~~~~

​	2、用using

~~~~C++
using Li::fp;
fp = fopen("one.txt", "r");  //使用小李定义的变量 fp
Han :: fp = fopen("two.txt", "rb+");  //使用小韩定义的变量 fp
~~~~

在代码的开头用`using`声明了 Li::fp，它的意思是，using 声明以后的程序中如果出现了未指明命名空间的 fp，就使用 Li::fp；但是若要使用小韩定义的 fp，仍然需要 Han::fp。

也就是说fp默认就是Li的了

​	3、声明整个名字空间

~~~~C++
using namespace Li;
fp = fopen("one.txt", "r");  //使用小李定义的变量 fp
Han::fp = fopen("two.txt", "rb+");  //使用小韩定义的变量 fp
~~~~

如果命名空间 Li 中还定义了其他的变量，那么同样具有 fp 变量的效果。在 using 声明后，如果有未具体指定命名空间的变量产生了命名冲突，那么默认采用命名空间 Li 中的变量。

# 类

## 1、封装类：

就是把成员函数和成员变量限定在以类里面。

类只是一个模板（Template），编译后不占用内存空间，所以在定义类时不能对成员变量进行初始化，因为没有地方存储数据。只有在创建对象以后才会给成员变量分配内存，这个时候就可以赋值了。

这是一个简单的例子

~~~~C++
#include <iostream>
using namespace std;
//类的声明
class Student{
private:  //私有的
    char *m_name;
    int m_age;
    float m_score;
public:  //共有的
    void setname(char *name);
    void setage(int age);
    void setscore(float score);
    void show();
};
//成员函数的定义
void Student::setname(char *name){
    m_name = name;
}
void Student::setage(int age){
    m_age = age;
}
void Student::setscore(float score){
    m_score = score;
}
void Student::show(){
    cout<<m_name<<"的年龄是"<<m_age<<"，成绩是"<<m_score<<endl;
}
int main(){
    //在栈上创建对象
    Student stu;
    stu.setname("小明");
    stu.setage(15);
    stu.setscore(92.5f);
    stu.show();
    //在堆上创建对象
    Student *pstu = new Student;
    pstu -> setname("李华");
    pstu -> setage(16);
    pstu -> setscore(96);
    pstu -> show();
    return 0;
}
~~~~

使用类

```c++
int main(){
  类名 对象名；
}
```

类要注意的几点：

1、类可以看做是一种数据类型，它类似于普通的数据类型，但是又有别于普通的数据类型。类这种数据类型是一个包含成员变量和成员函数的集合。

2、成员函数是一个类的成员，出现在类体中，它的作用范围由类来决定

### 1.1 访问限制

在类的内部（定义类的代码内部），无论成员被声明为 public、protected 还是 private，都是可以互相访问的，没有访问权限的限制。

在类的外部（定义类的代码之外），只能通过对象访问成员，并且通过对象只能访问 public 属性的成员，不能访问 private、protected 属性的成员。



`public` : 外部可以访问

`protect` ： 外部不可以访问，派生类内部可以访问

`private`： 外部可以访问

`class` 默认是private

### 1.2 成员函数

成员函数必须先在类体中作原型声明，然后在类外定义，也就是说类体的位置应在函数定义之前。

~~~~C++
class Student{
public:
    //成员变量
    char *name;
    int age;
    float score;
    //成员函数
    void say();  //函数声明
};
//函数定义
void Student::say(){
    cout<<name<<"的年龄是"<<age<<"，成绩是"<<score<<endl;
}
~~~~

在类体中定义的成员函数会自动成为内联函数，在类体外定义的不会。所以我建议在类体内部对成员函数作声明，而在类体外部进行定义，

用 `：：` 关键字就好

~~~~C++
数据类型 类名：：成员函数（）{}
~~~~

## 2.部分内容

### 2.1 this指针

this 是 [C++](http://c.biancheng.net/cplus/) 中的一个关键字，也是一个 const [指针](http://c.biancheng.net/c/80/)，它指向当前对象，通过它可以访问当前对象的所有成员。

所谓当前对象，是指正在使用的对象。例如对于`stu.show();`，stu 就是当前对象，this 就指向 stu。

~~~~C++
#include <iostream>
using namespace std;
class Student{
public:
    void setname(char *name);
    void setage(int age);
    void setscore(float score);
    void show();
private:
    char *name;
    int age;
    float score;
};
void Student::setname(char *name){
    this->name = name;
}
void Student::setage(int age){
    this->age = age;
}
void Student::setscore(float score){
    this->score = score;
}
void Student::show(){
    cout<<this->name<<"的年龄是"<<this->age<<"，成绩是"<<this->score<<endl;
}
int main(){
    Student *pstu = new Student;
    pstu -> setname("李华");
    pstu -> setage(16);
    pstu -> setscore(96.5);
    pstu -> show();
    return 0;
}
~~~~

本例中成员函数的参数和成员变量重名，只能通过 this 区分。

this 只能用在类的内部，通过 this 可以访问类的所有成员，包括 private、protected、public 属性的

### 2.2 静态成员变量

使用静态成员变量来实现多个对象共享数据的目标。静态成员变量是一种特殊的成员变量，它被关键字`static`修饰，比较经典的用法就是使用静态成员变量来统计对象调用次数

~~~~C++
 class Student{
public:
    Student(char *name, int age, float score);
    void show();
private:
    static int m_total;  //静态成员变量
private:
    char *m_name;
    int m_age;
    float m_score;
};
//初始化静态成员变量
int Student::m_total = 0;
Student::Student(char *name, int age, float score): m_name(name), m_age(age), m_score(score){
    m_total++;  //操作静态成员变量
}
void Student::show(){
    cout<<m_name<<"的年龄是"<<m_age<<"，成绩是"<<m_score<<"（当前共有"<<m_total<<"名学生）"<<endl;
}
int main(){
    //创建匿名对象
    (new Student("小明", 15, 90)) -> show();
    (new Student("李磊", 16, 80)) -> show();
    (new Student("张华", 16, 99)) -> show();
    (new Student("王康", 14, 60)) -> show();
    return 0;
}

~~~~

当然如果对象销毁，我们就在析构函数里static `m_total --`;

### 2.3 静态成员变量

普通成员函数可以访问所有成员（包括成员变量和成员函数），静态成员函数只能访问静态成员

普通成员函数有 this 指针，可以访问类中的任意成员；而静态成员函数没有 this 指针，只能访问静态成员（包括静态成员变量和静态成员函数）

：统计总的成绩和人数

~~~~C++
class Student{
public:
    Student(char *name, int age, float score);
    void show();
public:  //声明静态成员函数
    static int getTotal();
    static float getPoints();
private:
    static int m_total;  //总人数
    static float m_points;  //总成绩
private:
    char *m_name;
    int m_age;
    float m_score;
};
int Student::m_total = 0;
float Student::m_points = 0.0;
Student::Student(char *name, int age, float score): m_name(name), m_age(age), m_score(score){
    m_total++;
    m_points += score;
}
void Student::show(){
    cout<<m_name<<"的年龄是"<<m_age<<"，成绩是"<<m_score<<endl;
}
//定义静态成员函数
int Student::getTotal(){
    return m_total;
}
float Student::getPoints(){
    return m_points;
}
int main(){
    (new Student("小明", 15, 90.6)) -> show();
    (new Student("李磊", 16, 80.5)) -> show();
    (new Student("张华", 16, 99.0)) -> show();
    (new Student("王康", 14, 60.8)) -> show();
    int total = Student::getTotal();
    float points = Student::getPoints();
    cout<<"当前共有"<<total<<"名学生，总成绩是"<<points<<"，平均分是"<<points/total<<endl;
    return 0;
}
~~~~

这里是通过类来调用的

### 2.4 常成员变量和函数

初始化 const 成员变量只有一种方法，就是通过构造函数的初始化列表

需要强调的是，必须在成员函数的声明和定义处同时加上 const 关键字

~~~~C++
class Student{
public:
    Student(char *name, int age, float score);
    void show();
    //声明常成员函数
    char *getname() const;
    int getage() const;
    float getscore() const;//调用const变量的函数后面还要加一个const
private:
    char *m_name;
    int m_age;
    float m_score;
};
Student::Student(char *name, int age, float score): m_name(name), m_age(age), m_score(score){ }
void Student::show(){
    cout<<m_name<<"的年龄是"<<m_age<<"，成绩是"<<m_score<<endl;
}
//定义常成员函数
char * Student::getname() const{
    return m_name;
}
int Student::getage() const{
    return m_age;
}
float Student::getscore() const{//调用const变量的函数后面还要加一个const
    return m_score;
}
~~~~

只是为了更保险，语义更明确

### 2.5 对象数组

许数组的每个元素都是对象，这样的数组称为对象数组

~~~~C++
2. using namespace std;
3.
4. class CSample{
5. public:
6. CSample(){ //构造函数 1
7. cout<<"Constructor 1 Called"<<endl;
8. }
9. CSample(int n){ //构造函数 2
10. cout<<"Constructor 2 Called"<<endl;
11. }
12. };
13.
14. int main(){
15. cout<<"stepl"<<endl;
16. CSample arrayl[2];
17.
18. cout<<"step2"<<endl;
19. CSample array2[2] = {4, 5};
20.
21. cout<<"step3"<<endl;
22. CSample array3[2] = {3};
23.
24. cout<<"step4"<<endl;
25. CSample* array4 = new CSample[2];
26. delete [] array4;
27.
28. return 0;
29. }
~~~~

程序的输出结果是：
stepl
Constructor 1 Called
Constructor 1 Called
step2
Constructor 2 Called
Constructor 2 Called
step3
Constructor 2 Called
Constructor 1 Called
step4
Constructor 1 Called
Constructor 1 Called

数组的初始化列表中要显式地包含对构造函数的调用:

~~~~C++
class CTest{
2. public:
3. CTest(int n){ } //构造函数(1)
4. CTest(int n, int m){ } //构造函数(2)
5. CTest(){ } //构造函数(3)
6. };
7. int main(){
8. //三个元素分别用构造函数(1)、(2)、(3) 初始化
9. CTest arrayl [3] = { 1, CTest(1,2) };
10. //三个元素分别用构造函数(2)、(2)、(1)初始化
11. CTest array2[3] = { CTest(2,3), CTest(1,2), 1};
12. //两个元素指向的对象分别用构造函数(1)、(2)初始化
13. CTest* pArray[3] = { new CTest(4), new CTest(1,2) };
14.
15. return 0;
16. }
~~~~





 ## 3、构造函数

### 3.1 构造函数定义

一种特殊的成员函数，**它的名字和类名相同**，**没有返回值**，**不需要用户显式调用**用户也不能调用），而是在**创建对象时自动执行**、**没有任何数据类型**

~~~~C++
class Student{
private:
    char *m_name;
    int m_age;
    float m_score;
public:
    //声明构造函数
    Student(char *name, int age, float score);
    //声明普通成员函数
    void show();
};
//定义构造函数
Student::Student(char *name, int age, float score){
    m_name = name;
    m_age = age;
    m_score = score;
}
//定义普通成员函数
void Student::show(){
    cout<<m_name<<"的年龄是"<<m_age<<"，成绩是"<<m_score<<endl;
}
int main(){
    //创建对象时向构造函数传参
    Student stu("小明", 15, 92.5f);
    stu.show();
    //创建对象时向构造函数传参
    Student *pstu = new Student("李华", 16, 96);
    pstu -> show();
    return 0;
}
~~~~

### 3.2 构造函数重载

构造函数的调用是强制性的，一旦在类中定义了构造函数，那么创建对象时就一定要调用，不调用是错误的。

如果有多个重载的构造函数，那么创建对象时提供的实参必须和其中的一个构造函数匹配；

反过来说，创建对象时只有一个构造函数会被调用。

~~~~C++

class Student{
private:
    char *m_name;
    int m_age;
    float m_score;
public:
    Student();
    Student(char *name, int age, float score);
    void setname(char *name);
    void setage(int age);
    void setscore(float score);
    void show();
};
Student::Student(){
    m_name = NULL;
    m_age = 0;
    m_score = 0.0;
}
Student::Student(char *name, int age, float score){
    m_name = name;
    m_age = age;
    m_score = score;
}
void Student::setname(char *name){
    m_name = name;
}
void Student::setage(int age){
    m_age = age;
}
void Student::setscore(float score){
    m_score = score;
}
void Student::show(){
    if(m_name == NULL || m_age <= 0){
        cout<<"成员变量还未初始化"<<endl;
    }else{
        cout<<m_name<<"的年龄是"<<m_age<<"，成绩是"<<m_score<<endl;
    }
}
int main(){
    //调用构造函数 Student(char *, int, float)
    Student stu("小明", 15, 92.5f);
    stu.show();
    //调用构造函数 Student()
    Student *pstu = new Student();
    pstu -> show();
    pstu -> setname("李华");
    pstu -> setage(16);
    pstu -> setscore(96);
    pstu -> show();
    return 0;
}
~~~~

### 3.3 默认构造函数

一个类必须有构造函数，要么用户自己定义，要么编译器自动生成。一旦用户自己定义了构造函数，不管有几个，也不管形参如何，编译器都不再自动生成。

调用没有参数的构造函数也可以省略括号。

~~~~C++
Student(){}
~~~~

### 3.4 初始化列表

构造函数的初始化列表使得代码更加简洁

~~~~c++
class Student{
private:
    char *m_name;
    int m_age;
    float m_score;
public:
    Student(char *name, int age, float score);
    void show();
};
//采用初始化列表
Student::Student(char *name, int age, float score): m_name(name), m_age(age), m_score(score){
    //TODO:
}
void Student::show(){
    cout<<m_name<<"的年龄是"<<m_age<<"，成绩是"<<m_score<<endl;
}
int main(){
    Student stu("小明", 15, 92.5f);
    stu.show();
    Student *pstu = new Student("李华", 16, 96);
    pstu -> show();
    return 0;
}
~~~~

初始化 const 成员变量的唯一方法就是使用初始化列表

~~~~C++
class VLA{
private:
    const int m_len;
    int *m_arr;
public:
    VLA(int len);
};
//必须使用初始化列表来初始化 m_len
VLA::VLA(int len): m_len(len){
    m_arr = new int[len];
}
~~~~

### 3.5 拷贝构造函数

当以拷贝的方式初始化一个对象时，会调用一个特殊的构造函数，就是拷贝构造函数

初始化阶段进行的，也就是用其它对象的数据来初始化新对象的内存

~~~~C++
class Student{
public:
    Student(string name = "", int age = 0, float score = 0.0f);  //普通构造函数
    Student(const Student &stu);  //拷贝构造函数（声明）
public:
    void display();
private:
    string m_name;
    int m_age;
    float m_score;
};
Student::Student(string name, int age, float score): m_name(name), m_age(age), m_score(score){ }
//拷贝构造函数（定义）
Student::Student(const Student &stu){
    this->m_name = stu.m_name;
    this->m_age = stu.m_age;
    this->m_score = stu.m_score;
   
    cout<<"Copy constructor was called."<<endl;
}
void Student::display(){
    cout<<m_name<<"的年龄是"<<m_age<<"，成绩是"<<m_score<<endl;
}
int main(){
    Student stu1("小明", 16, 90.5);
    Student stu2 = stu1;  //调用拷贝构造函数
    Student stu3(stu1);  //调用拷贝构造函数
    stu1.display();
    stu2.display();
    stu3.display();
   
    return 0;
}
~~~~

拷贝构造函数只有一个参数，**它的类型是当前类的引用，而且一般都是 const 引用**

### 3.6 浅拷贝

~~~~C++
class Base{
public:
    Base(): m_a(0), m_b(0){ }
    Base(int a, int b): m_a(a), m_b(b){ }
private:
    int m_a;
    int m_b;
};
int main(){
    int a = 10;
    int b = a;  //拷贝
    Base obj1(10, 20);
    Base obj2 = obj1;  //拷贝
    return 0;
}
~~~~



### 3.7 深拷贝

这种将对象所持有的其它资源一并拷贝的行为叫做深拷贝，我们必须显式地定义拷贝构造函数才能达到深拷贝的目的

~~~~C++
//变长数组类
class Array{
public:
    Array(int len);
    Array(const Array &arr);  //拷贝构造函数
    ~Array();
public:
    int operator[](int i) const { return m_p[i]; }  //获取元素（读取）
    int &operator[](int i){ return m_p[i]; }  //获取元素（写入）
    int length() const { return m_len; }
private:
    int m_len;
    int *m_p;
};
Array::Array(int len): m_len(len){
    m_p = (int*)calloc( len, sizeof(int) );
}
Array::Array(const Array &arr){  //拷贝构造函数
    this->m_len = arr.m_len;
    this->m_p = (int*)calloc( this->m_len, sizeof(int) );
    memcpy( this->m_p, arr.m_p, m_len * sizeof(int) );
}
Array::~Array(){ free(m_p); }
//打印数组元素
void printArray(const Array &arr){
    int len = arr.length();
    for(int i=0; i<len; i++){
        if(i == len-1){
            cout<<arr[i]<<endl;
        }else{
            cout<<arr[i]<<", ";
        }
    }
}
int main(){
    Array arr1(10);
    for(int i=0; i<10; i++){
        arr1[i] = i;
    }
   
    Array arr2 = arr1;
    arr2[5] = 100;
    arr2[3] = 29;
   
    printArray(arr1);
    printArray(arr2);
   
    return 0;
}
~~~~

### 3.8 构造函数实现类型的转换

将其它类型转换为当前类类型需要借助转换构造函数

~~~~C++
#include <iostream>
using namespace std;
//复数类
class Complex{
public:
    Complex(): m_real(0.0), m_imag(0.0){ }
    Complex(double real, double imag): m_real(real), m_imag(imag){ }
    Complex(double real): m_real(real), m_imag(0.0){ }  //转换构造函数
public:
    friend ostream & operator<<(ostream &out, Complex &c);  //友元函数
private:
    double m_real;  //实部
    double m_imag;  //虚部
};
//重载>>运算符
ostream & operator<<(ostream &out, Complex &c){
    out << c.m_real <<" + "<< c.m_imag <<"i";;
    return out;
}
int main(){
    Complex a(10.0, 20.0);
    cout<<a<<endl;
    a = 25.5;  //调用转换构造函数
    cout<<a<<endl;
    return 0;
}
~~~~

运行结果：
10 + 20i
25.5 + 0i

```
Complex(double real);
```

就是转换构造函数，它的作用是将 double 类型的参数 real 转换成 Complex 类的对象，并将 real 作为复数的实部，将 0 作为复数的虚部。这样一来，

```
a = 25.5;
```

整体上的效果相当于：

a.Complex(25.5);

将赋值的过程转换成了函数调用的过程。

~~~~C++
Complex c1(26.4);  //创建具名对象
Complex c2 = 240.3;  //以拷贝的方式初始化对象
Complex(15.9);  //创建匿名对象
c1 = Complex(46.9);  //创建一个匿名对象并将它赋值给 c1
~~~~

如果已经对`+`运算符进行了重载，使之能进行两个 Complex 类对象的相加，那么下面的语句也是正确的：

~~~~C++
Complex c1(15.6, 89.9);
Complex c2;
c2 = c1 + 29.6;
cout<<c2<<endl;
~~~~

~~~~C++
int main(){
    Complex c1 = 100;  //int --> double --> Complex
    cout<<c1<<endl;
    c1 = 'A';  //char --> int --> double --> Complex
    cout<<c1<<endl;
    c1 = true;  //bool --> int --> double --> Complex
    cout<<c1<<endl;
    Complex c2(25.8, 0.7);
    //假设已经重载了+运算符
    c1 = c2 + 'H' + true + 15;  //将char、bool、int都转换为Complex类型再运算
    cout<<c1<<endl;
    return 0;
}
~~~~

运行结果：
100 + 0i
65 + 0i
1 + 0i
113.8 + 0.7i

精简后

~~~~C++
class Complex{
public:
    Complex(double real = 0.0, double imag = 0.0): m_real(real), m_imag(imag){ }
public:
    friend ostream & operator<<(ostream &out, Complex &c);  //友元函数
private:
    double m_real;  //实部
    double m_imag;  //虚部
};
//重载>>运算符
ostream & operator<<(ostream &out, Complex &c){
    out << c.m_real <<" + "<< c.m_imag <<"i";;
    return out;
}
int main(){
    Complex a(10.0, 20.0);  //向构造函数传递 2 个实参，不使用默认参数
    Complex b(89.5);  //向构造函数传递 1 个实参，使用 1 个默认参数
    Complex c;  //不向构造函数传递实参，使用全部默认参数
    a = 25.5;  //调用转换构造函数（向构造函数传递 1 个实参，使用 1 个默认参数）
    return 0;
}
~~~~



## 4、析构函数

一种特殊的成员函数，没有返回值，不需要程序员显式调用（程序员也没法显式调用），而是在销毁对象时自动执行

一种特殊的成员函数，没有返回值，不需要程序员显式调用（程序员也没法显式调用），而是在销毁对象时自动执行

~~~~C++
class VLA{
public:
    VLA(int len);  //构造函数
    ~VLA();  //析构函数
public:
    void input();  //从控制台输入数组元素
    void show();  //显示数组元素
private:
    int *at(int i);  //获取第i个元素的指针
private:
    const int m_len;  //数组长度
    int *m_arr; //数组指针
    int *m_p;  //指向数组第i个元素的指针
};
VLA::VLA(int len): m_len(len){  //使用初始化列表来给 m_len 赋值
    if(len > 0){ m_arr = new int[len];  /*分配内存*/ }
    else{ m_arr = NULL; }
}
VLA::~VLA(){
    delete[] m_arr;  //释放内存
}
void VLA::input(){
    for(int i=0; m_p=at(i); i++){ cin>>*at(i); }
}
void VLA::show(){
    for(int i=0; m_p=at(i); i++){
        if(i == m_len - 1){ cout<<*at(i)<<endl; }
        else{ cout<<*at(i)<<", "; }
    }
}
int * VLA::at(int i){
    if(!m_arr || i<0 || i>=m_len){ return NULL; }
    else{ return m_arr + i; }
}
int main(){
    //创建一个有n个元素的数组（对象）
    int n;
    cout<<"Input array length: ";
    cin>>n;
    VLA *parr = new VLA(n);
    //输入数组元素
    cout<<"Input "<<n<<" numbers: ";
    parr -> input();
    //输出数组元素
    cout<<"Elements: ";
    parr -> show();
    //删除数组（对象）
    delete parr;
    return 0;
}
~~~~

## 5、类型转换

类型转换函数的作用就是将当前类类型转换为其它类型，它只能以成员函数的形式出现，也就是只能出现在类中.

因为要转换的目标类型是 type，所以返回值 data 也必须是 type 类型。既然已经知道了要返回 type 类型的数据

~~~~C++
class Complex{
public:
    Complex(): m_real(0.0), m_imag(0.0){ }
    Complex(double real, double imag): m_real(real), m_imag(imag){ }
public:
    friend ostream & operator<<(ostream &out, Complex &c);
    friend Complex operator+(const Complex &c1, const Complex &c2);
    operator double() const { return m_real; }  //类型转换函数
private:
    double m_real;  //实部
    double m_imag;  //虚部
};
//重载>>运算符
ostream & operator<<(ostream &out, Complex &c){
    out << c.m_real <<" + "<< c.m_imag <<"i";;
    return out;
}
//重载+运算符
Complex operator+(const Complex &c1, const Complex &c2){
    Complex c;
    c.m_real = c1.m_real + c2.m_real;
    c.m_imag = c1.m_imag + c2.m_imag;
    return c;
}
int main(){
    Complex c1(24.6, 100);
    double f = c1;  //相当于 double f = Complex::operator double(&c1);
    cout<<"f = "<<f<<endl;
    f = 12.5 + c1 + 6;  //相当于 f = 12.5 + Complex::operator double(&c1) + 6;
    cout<<"f = "<<f<<endl;
    int n = Complex(43.2, 9.3);  //先转换为 double，再转换为 int
    cout<<"n = "<<n<<endl;
    return 0;
}
~~~~

结果：

f = 24.6
f = 43.1
n = 43

1) type 可以是内置类型、类类型以及由 typedef 定义的类型别名，任何可作为函数返回类型的类型（void 除外）都能够被支持。一般而言，不允许转换为数组或函数类型，转换为指针类型或引用类型是可以的。

2) 类型转换函数一般不会更改被转换的对象，所以通常被定义为 const 成员。

3) 类型转换函数可以被继承，可以是虚函数。

`operator` 是 C++ 关键字，type 是要转换的目标类型，data 是要返回的 type 类型的数据。

## 6、运算符重载

### 6.1 基本操作

也就是说，运算符重载是通过函数实现的，它本质上是函数重载

~~~~C++
class complex{
public:
    complex();
    complex(double real, double imag);
public:
    //声明运算符重载
    complex operator+(const complex &A) const;
    void display() const;
private:
    double m_real;  //实部
    double m_imag;  //虚部
};
complex::complex(): m_real(0.0), m_imag(0.0){ }
complex::complex(double real, double imag): m_real(real), m_imag(imag){ }
//实现运算符重载
complex complex::operator+(const complex &A) const{
    complex B;
    B.m_real = this->m_real + A.m_real;
    B.m_imag = this->m_imag + A.m_imag;
    return B;
}
void complex::display() const{
    cout<<m_real<<" + "<<m_imag<<"i"<<endl;
}
int main(){
    complex c1(4.3, 5.8);
    complex c2(2.4, 3.7);
    complex c3;
    c3 = c1 + c2;
  //c3 = c1.operator+(c2);
    c3.display();
    return 0;
}
~~~~

运行结果：

6.7 + 9.5i

简洁形式：

~~~~C++
complex complex::operator+(const complex &A)const{
    return complex(this->m_real + A.m_real, this->m_imag + A.m_imag);
}
~~~~



运算符重载的格式：

~~~~
返回值类型 operator 运算符名称 (形参表列){
    //TODO:
}
~~~~

运算符重载函数不仅可以作为类的成员函数，还可以作为全局函数

~~~~C++
class complex{
public:
    complex();
    complex(double real, double imag);
public:
    void display() const;
    //声明为友元函数
    friend complex operator+(const complex &A, const complex &B);
private:
    double m_real;
    double m_imag;
};
complex operator+(const complex &A, const complex &B);
complex::complex(): m_real(0.0), m_imag(0.0){ }
complex::complex(double real, double imag): m_real(real), m_imag(imag){ }
void complex::display() const{
    cout<<m_real<<" + "<<m_imag<<"i"<<endl;
}
//在全局范围内重载+
complex operator+(const complex &A, const complex &B){
    complex C;
    C.m_real = A.m_real + B.m_real;
    C.m_imag = A.m_imag + B.m_imag;
    return C;
}
int main(){
    complex c1(4.3, 5.8);
    complex c2(2.4, 3.7);
    complex c3;
    c3 = c1 + c2;
  //c3 = operator+(c1, c2);
    c3.display();
    return 0;
}
~~~~

### 6.2 注意事项

1) 并不是所有的运算符都可以重载。

`sizeof`  , `:?` , `.` , `::` 这些不能够改变

 2) 重载不能改变运算符的优先级和结合性。

3) 重载不会改变运算符的用法，原有有几个操作数、操作数在左边还是在右边，这些都不会改变。

4) 运算符重载函数不能有默认的参数

5）将运算符重载函数作为全局函数时，二元操作符就需要两个参数，一元操作符需要一个参数，而且其中必须有一个参数是对象，好让编译器区分这是程序员自定义的运算符，防止程序员修改用于内置类型的运算符的性质。

说白了其中参数得是类。

### 6.3 案例

~~~~c++
class Complex{
public:  //构造函数
    Complex(double real = 0.0, double imag = 0.0): m_real(real), m_imag(imag){ }
public:  //运算符重载
    //以全局函数的形式重载
    friend Complex operator+(const Complex &c1, const Complex &c2);
    friend Complex operator-(const Complex &c1, const Complex &c2);
    friend Complex operator*(const Complex &c1, const Complex &c2);
    friend Complex operator/(const Complex &c1, const Complex &c2);
    friend bool operator==(const Complex &c1, const Complex &c2);
    friend bool operator!=(const Complex &c1, const Complex &c2);
    //以成员函数的形式重载
    Complex & operator+=(const Complex &c);
    Complex & operator-=(const Complex &c);
    Complex & operator*=(const Complex &c);
    Complex & operator/=(const Complex &c);
public:  //成员函数
    double real() const{ return m_real; }
    double imag() const{ return m_imag; }
private:
    double m_real;  //实部
    double m_imag;  //虚部
};
//重载+运算符
Complex operator+(const Complex &c1, const Complex &c2){
    Complex c;
    c.m_real = c1.m_real + c2.m_real;
    c.m_imag = c1.m_imag + c2.m_imag;
    return c;
}
//重载-运算符
Complex operator-(const Complex &c1, const Complex &c2){
    Complex c;
    c.m_real = c1.m_real - c2.m_real;
    c.m_imag = c1.m_imag - c2.m_imag;
    return c;
}
//重载*运算符  (a+bi) * (c+di) = (ac-bd) + (bc+ad)i
Complex operator*(const Complex &c1, const Complex &c2){
    Complex c;
    c.m_real = c1.m_real * c2.m_real - c1.m_imag * c2.m_imag;
    c.m_imag = c1.m_imag * c2.m_real + c1.m_real * c2.m_imag;
    return c;
}
//重载/运算符  (a+bi) / (c+di) = [(ac+bd) / (c²+d²)] + [(bc-ad) / (c²+d²)]i
Complex operator/(const Complex &c1, const Complex &c2){
    Complex c;
    c.m_real = (c1.m_real*c2.m_real + c1.m_imag*c2.m_imag) / (pow(c2.m_real, 2) + pow(c2.m_imag, 2));
    c.m_imag = (c1.m_imag*c2.m_real - c1.m_real*c2.m_imag) / (pow(c2.m_real, 2) + pow(c2.m_imag, 2));
    return c;
}
//重载==运算符
bool operator==(const Complex &c1, const Complex &c2){
    if( c1.m_real == c2.m_real && c1.m_imag == c2.m_imag ){
        return true;
    }else{
        return false;
    }
}
//重载!=运算符
bool operator!=(const Complex &c1, const Complex &c2){
    if( c1.m_real != c2.m_real || c1.m_imag != c2.m_imag ){
        return true;
    }else{
        return false;
    }
}
//重载+=运算符
Complex & Complex::operator+=(const Complex &c){
    this->m_real += c.m_real;
    this->m_imag += c.m_imag;
    return *this;
}
//重载-=运算符
Complex & Complex::operator-=(const Complex &c){
    this->m_real -= c.m_real;
    this->m_imag -= c.m_imag;
    return *this;
}
//重载*=运算符
Complex & Complex::operator*=(const Complex &c){
    this->m_real = this->m_real * c.m_real - this->m_imag * c.m_imag;
    this->m_imag = this->m_imag * c.m_real + this->m_real * c.m_imag;
    return *this;
}
//重载/=运算符
Complex & Complex::operator/=(const Complex &c){
    this->m_real = (this->m_real*c.m_real + this->m_imag*c.m_imag) / (pow(c.m_real, 2) + pow(c.m_imag, 2));
    this->m_imag = (this->m_imag*c.m_real - this->m_real*c.m_imag) / (pow(c.m_real, 2) + pow(c.m_imag, 2));
    return *this;
}
int main(){
    Complex c1(25, 35);
    Complex c2(10, 20);
    Complex c3(1, 2);
    Complex c4(4, 9);
    Complex c5(34, 6);
    Complex c6(80, 90);
   
    Complex c7 = c1 + c2;
    Complex c8 = c1 - c2;
    Complex c9 = c1 * c2;
    Complex c10 = c1 / c2;
    cout<<"c7 = "<<c7.real()<<" + "<<c7.imag()<<"i"<<endl;
    cout<<"c8 = "<<c8.real()<<" + "<<c8.imag()<<"i"<<endl;
    cout<<"c9 = "<<c9.real()<<" + "<<c9.imag()<<"i"<<endl;
    cout<<"c10 = "<<c10.real()<<" + "<<c10.imag()<<"i"<<endl;
   
    c3 += c1;
    c4 -= c2;
    c5 *= c2;
    c6 /= c2;
    cout<<"c3 = "<<c3.real()<<" + "<<c3.imag()<<"i"<<endl;
    cout<<"c4 = "<<c4.real()<<" + "<<c4.imag()<<"i"<<endl;
    cout<<"c5 = "<<c5.real()<<" + "<<c5.imag()<<"i"<<endl;
    cout<<"c6 = "<<c6.real()<<" + "<<c6.imag()<<"i"<<endl;
   
    if(c1 == c2){
        cout<<"c1 == c2"<<endl;
    }
    if(c1 != c2){
        cout<<"c1 != c2"<<endl;
    }
   
    return 0;
}
~~~~

运行结果：
c7 = 35 + 55i
c8 = 15 + 15i
c9 = -450 + 850i
c10 = 1.9 + -0.3i
c3 = 26 + 37i
c4 = -6 + -11i
c5 = 220 + 4460i
c6 = 5.2 + 1.592i
c1 != c2

### 6.4 重载 <<  和 >>

cout 是 ostream 类的对象，cin 是 istream 类的对象，要想达到这个目标，就必须以全局函数（友元函数）的形式重载`<<`和`>>`

**重载<<**

~~~~C++
istream & operator>>(istream &in, complex &a){
  in >> a.m_real >> a.m_imag;
  return in;
}
~~~~

声明：

~~~~C++
friend istream & operator>>(istream & in , complex &a);
~~~~

使用：

~~~~C++
complex c;
cin>>c;//等价于 operator << (cin,c);
~~~~

**重载>>**

~~~~C++
ostream & operator << (ostream &out , complex &a){
  out << a.m_real << "+" << a.m_imag << "i" ; 
  return out;
}
~~~~

案例：

~~~~C++
class complex{
public:
    complex(double real = 0.0, double imag = 0.0): m_real(real), m_imag(imag){ };
public:
    friend complex operator+(const complex & A, const complex & B);
    friend complex operator-(const complex & A, const complex & B);
    friend complex operator*(const complex & A, const complex & B);
    friend complex operator/(const complex & A, const complex & B);
    friend istream & operator>>(istream & in, complex & A);
    friend ostream & operator<<(ostream & out, complex & A);
private:
    double m_real;  //实部
    double m_imag;  //虚部
};
//重载加法运算符
complex operator+(const complex & A, const complex &B){
    complex C;
    C.m_real = A.m_real + B.m_real;
    C.m_imag = A.m_imag + B.m_imag;
    return C;
}
//重载减法运算符
complex operator-(const complex & A, const complex &B){
    complex C;
    C.m_real = A.m_real - B.m_real;
    C.m_imag = A.m_imag - B.m_imag;
    return C;
}
//重载乘法运算符
complex operator*(const complex & A, const complex &B){
    complex C;
    C.m_real = A.m_real * B.m_real - A.m_imag * B.m_imag;
    C.m_imag = A.m_imag * B.m_real + A.m_real * B.m_imag;
    return C;
}
//重载除法运算符
complex operator/(const complex & A, const complex & B){
    complex C;
    double square = A.m_real * A.m_real + A.m_imag * A.m_imag;
    C.m_real = (A.m_real * B.m_real + A.m_imag * B.m_imag)/square;
    C.m_imag = (A.m_imag * B.m_real - A.m_real * B.m_imag)/square;
    return C;
}
//重载输入运算符
istream & operator>>(istream & in, complex & A){
    in >> A.m_real >> A.m_imag;
    return in;
}
//重载输出运算符
ostream & operator<<(ostream & out, complex & A){
    out << A.m_real <<" + "<< A.m_imag <<" i ";;
    return out;
}
int main(){
    complex c1, c2, c3;
    cin>>c1>>c2;
    c3 = c1 + c2;
    cout<<"c1 + c2 = "<<c3<<endl;
    c3 = c1 - c2;
    cout<<"c1 - c2 = "<<c3<<endl;
    c3 = c1 * c2;
    cout<<"c1 * c2 = "<<c3<<endl;
    c3 = c1 / c2;
    cout<<"c1 / c2 = "<<c3<<endl;
    return 0;
}
~~~~

### 6.5 重载 [] 

需要同时重载 `const` 和 非 `const` 这两种类型

~~~~
返回类型 & operator [] (参数)
~~~~

~~~~
const 返回类型 & opertor [] (参数)
~~~~

案例：

~~~~C++

class Array{
public:
    Array(int length = 0);
    ~Array();
public:
    int & operator[](int i);
    const int & operator[](int i) const;
public:
    int length() const { return m_length; }
    void display() const;
private:
    int m_length;  //数组长度
    int *m_p;  //指向数组内存的指针
};
Array::Array(int length): m_length(length){
    if(length == 0){
        m_p = NULL;
    }else{
        m_p = new int[length];
    }
}
Array::~Array(){
    delete[] m_p;
}
int& Array::operator[](int i){
    return m_p[i];
}
const int & Array::operator[](int i) const{
    return m_p[i];
}
void Array::display() const{
    for(int i = 0; i < m_length; i++){
        if(i == m_length - 1){
            cout<<m_p[i]<<endl;
        }else{
            cout<<m_p[i]<<", ";
        }
    }
}
int main(){
    int n;
    cin>>n;
    Array A(n);
    for(int i = 0, len = A.length(); i < len; i++){
        A[i] = i * 5;
    }
    A.display();
   
    const Array B(n);
    cout<<B[n-1]<<endl;  //访问最后一个元素
   
    return 0;
}
~~~~

###  6.6 重载 ++ 和 --  

这个看具体的需求吧



