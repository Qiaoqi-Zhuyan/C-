-  -  7-1 例4-3游泳池改造预算 (10分)

   -  -  按题目要求（1）
      -  能通过就是好代码（2）

   -  7-2 复数类的操作 (10分)

   -  -  能通过就是好代码（1）
      -  按要求来

   -  7-3 运算符重载 (15分)

   -  -  偷懒不用类法（1）
      -  做一个好孩子法（2）

   -  7-4 日程安排（多重继承+重载） (15分)

   -  -  代码

   -  7-5 汽车收费 (10分)

   -  -  题目让我用我就用吗？（1）
      -  还是要做一个好孩子（2）

   -  7-6 师生信息管理 (10分)

   -  -  代码

   -  7-7 数据的最大值问题（重载+函数模板） (20分)

   -  -  代码

   -  7-8 数据的间距问题 (15分)

   -  -  就不用函数模板(2)
      -  听老师的话（2）

   -  7-9 组最大数 (15分)

   -  -  代码

   -  7-10 对称排序 (15分)

   -  -  代码

## 7-1 例4-3游泳池改造预算 (10分)

例4-3一圆形游泳池如图所示，现在需在其周围建一圆形过道，并在其四周围上栅栏。栅栏价格为35元/米，过道造价为20元/平方米。过道宽度为3米，游泳池半径由键盘输入。要求编程计算并输出过道和栅栏的造价。
【图片显示失败哈哈】
输入格式:
输入一个整数或小数。
输出格式:
分两行输出：在第一行中输出栅栏的造价。在第二行输出过道的造价。
输入样例:
10

输出样例:
Fencing Cost is $2858.85
Concrete Cost is $4335.4

### 按题目要求（1）

```c++
#include <iostream>
using namespace std;
const double PI =3.1415926;
class Box
{
public:double n,zl,gd;
    void seta(double ab){
      n=ab;
    };
    void getvolume(){
      zl=(n+3)*2*PI*35;
    };
    void getarea( ){
      double m=n+3;
      gd=(m*m*PI-n*n*PI)*20;
    };
    void disp(){
      cout<<"Fencing Cost is $"<<zl<<endl;
      cout<<"Concrete Cost is $"<<gd<<endl;
    };
};
int  main( ){
    double ab;
    cin>>ab;
    Box  obj;
    obj.seta( ab );
    obj.getvolume( );
    obj.getarea( );
    obj.disp( );
    return 0;
}

```

## 7-2 复数类的操作 (10分)

1、声明一个复数类Complex（类私有数据成员为double型的real和image）
2、定义构造函数，用于指定复数的实部与虚部。
3、定义取反成员函数，调用时能返回该复数的相反数（实部、虚部分别是原数的相反数）。
4、定义成员函数Print()，调用该函数时，以格式(real, image)输出当前对象。
5、编写加法友元函数，以复数对象c1，c2为参数，求两个复数对象相加之和。
6、主程序实现：
（1）读入两个实数，用于初始化对象c1。
（2）读入两个实数，用于初始化对象c2。
（3）计算c1与c2相加结果，并输出。
（4）计算c2的相反数与c1相加结果，并输出。
输入格式:
输入有两行：
第一行是复数c1的实部与虚部，以空格分隔；
第二行是复数c2的实部与虚部，以空格分隔。
输出格式:
输出共三行:
第一行是c1与c2之和；
第二行是c2的相反数与c1之和；
第三行是c2 。
输入样例:
在这里给出一组输入。例如：
2.5 3.7
4.2 6.5

输出样例:
在这里给出相应的输出。例如：
(6.7, 10.2)
(-1.7, -2.8)
(4.2, 6.5)



### 按要求来

```c++
#include<iostream>
#include<cmath>
#include<vector>
#include<iomanip>
using namespace std;
class Complex{
    public:
           Complex(double r,double i){
            real=r;
            image=i;
       }
       Complex xf(){
          Complex c(real*-1,image*-1);
          return c;
       }
       void Print(){
          cout<<'('<<real<<", "<<image<<')'<<endl;
       }
              friend void add(const Complex & c1,const Complex & c2);
    private:
        double real,image;
};
void add(const Complex & c1,const Complex & c2){
    double r,i;
    r=c1.real+c2.real;
    i=c1.image+c2.image;
     cout<<'('<<r<<", "<<i<<')'<<endl;
}
int main(){
    double r1,r2,i1,i2;
    cin>>r1>>i1>>r2>>i2;
    Complex c1(r1,i1);
    Complex c2(r2,i2);
    add(c1,c2);
    Complex c3=c2.xf();
    add(c3,c1);
    c2.Print();
    return 0;
}

```

## 7-3 运算符重载 (15分)

请定义一个分数类，拥有两个整数的私有数据成员，分别表示分子和分母（分母永远为正数，符号通过分子表示）。
重载运算符加号"+"，实现两个分数的相加，所得结果必须是最简分数。
输入:
第一行的两个数 分别表示 第一个分数的分子和分母（分母不为0）。 第二行的两个数 分别表示 第二个分数的分子和分母。
输出:
第一个数表示分子，第二个数表示分母（若分数代表的是整数，则不输出分母）。
输入样例:
1 5
2 5

输出样例:
3 5



### 做一个好孩子法（2）

```c++
#include<iostream>
using namespace std;
int gcd(int a,int b)
{
    if(b==0)
        return a;
    else
        return gcd(b,a%b);
}
class Complex
{
public:
    Complex(int fz,int fm)
    {
        this->fz = fz;
        this->fm= fm;
    }
    Complex() {};
    Complex  operator +(Complex & b);
    friend ostream &operator <<(ostream &os,Complex&s);
private:
    int fz,fm;
};
Complex Complex::operator+(Complex & b)
{
    Complex a;
    a.fz=this->fm*b.fz+this->fz*b.fm;
    a.fm=this->fm*b.fm;
    int m=gcd(a.fz,a.fm);
    a.fz=a.fz/m;
    a.fm=a.fm/m;
    return a;
}
ostream &operator <<(ostream &os,Complex&s)
{   if(s.fm==1) os<<s.fz;
    else os<<s.fz<<" "<<s.fm;

    return os;
}
int main()
{
    int a,b,c,d;
    cin>>a>>b>>c>>d;
    Complex x(a,b),y(c,d);
    Complex z=x+y;
    cout<<z<<endl;
    return 0;
}

```

## 7-4 日程安排（多重继承+重载） (15分)

7-4 日程安排（多重继承+重载） (15分)

已有一个日期类Date，包括三个protected成员数据
int year;
int month;
int day;
另有一个时间类Time，包括三个protected成员数据
int hour;
int minute;
int second;
现需根据输入的日程的日期时间，安排前后顺序，为此以Date类和Time类为基类，建立一个日程类Schedule，包括以下新增成员：
int ID；//日程的ID
bool operator < (const Schedule & s2);//判断当前日程时间是否早于s2
生成以上类，并编写主函数，根据输入的各项日程信息，建立日程对象，找出需要最早安排的日程，并输出该日程对象的信息。
输入格式： 测试输入包含若干日程，每个日程占一行（日程编号ID 日程日期（**//）日程时间（::））。当读入0时输入结束，相应的结果不要输出。
输入样例：
1 2014/06/27 08:00:01
2 2014/06/28 08:00:01
0
输出样例：
The urgent schedule is No.1: 2014/6/27 8:0:1

### 代码

```c++
#include <iostream>
using namespace std;
class Date
{
public:
    Date(int a,int b,int c)//=0都一样
    {
        year=a;
        month=b;
        day=c;
    }
//protected:
    int year;
    int month;
    int day;
};
class Time
{
    public:
    Time(int a,int b,int c)
    {
        hour=a;
        minute=b;
        second=c;
    }
//protected://用protected的话，别忘了底下也要换
    int hour;
    int minute;
    int second;
};
class Schedule:public Date,public Time{//题目里要protected，烦哦
    int ID;
public:
    Schedule(int a=10000,int b=0,int c=0,int d=0,int e=0,int f=0,int id=0)
    :Date(a,b,c),Time(d,e,f){
        ID=id;
    }
    bool operator < (const Schedule & s2){//易错
    if(year<s2.year){
        return true;
    }
    if(year==s2.year&&month<s2.month){
        return true;
    }
    if(year==s2.year&&month==s2.month&&day<s2.day){
        return true;
    }
    int sum1=hour*3600+minute*60+second;
    int sum2=s2.hour*3600+s2.minute*60+s2.second;
    if(year==s2.year&&month==s2.month&&day==s2.day&&sum1<sum2){
        return true;
    }
    return false;
    }
    void show(){
    cout<<"The urgent schedule is No.";//写在外边里边都行
    cout<<ID<<": "<<year<<"/"<<month<<"/"<<day<<" "<<hour<<":"<<minute<<":"<<second<<endl;
    }
};
int main() {
    int a,b,c,d,e,f,id;
    cin>>id;
    Schedule maxx;
    while(id){
        scanf("%d/%d/%d%d:%d:%d",&a,&b,&c,&d,&e,&f);
        Schedule sch(a,b,c,d,e,f,id);
        if(!(maxx<sch)){
            maxx=sch;
        }
        cin>>id;
    }
    
    maxx.show();
    return 0;
}

```

## 7-5 汽车收费 (10分)

现在要开发一个系统，管理对多种汽车的收费工作。 给出下面的一个基类框架
class Vehicle
{ protected:
string NO;//编号
public:
virtual void display()=0;//输出应收费用
}
以Vehicle为基类，构建出Car、Truck和Bus三个类。
Car的收费公式为： 载客数8+重量2
Truck的收费公式为：重量*5Bus的收费公式为： 载客数*3
生成上述类并编写主函数，要求主函数中有一个基类Vehicle指针数组，数组元素不超过10个。
Vehicle *pv[10];
主函数根据输入的信息，相应建立Car,Truck或Bus类对象，对于Car给出载客数和重量，Truck给出重量，Bus给出载客数。假设载客数和重量均为整数
输入格式：每个测试用例占一行，每行给出汽车的基本信息，每一个为当前汽车的类型1为car，2为Truck，3为Bus。接下来为它的编号，接下来Car是载客数和重量，Truck给出重量，Bus给出载客数。最后一行为0，表示输入的结束。 要求输出各车的编号和收费。
提示：应用虚函数实现多态
输入样例
1 002 20 5
3 009 30
2 003 50
1 010 17 6
0
输出样例
002 170
009 90
003 250
010 148



### 还是要做一个好孩子（2）

```c++
#include<iostream>
using namespace std;
typedef long long ll;

class Vehicle
{
protected:
    string NO;//编号
public:
    virtual void display()=0;//输出应收费用
};

class Car:public Vehicle{
public:
    int zk;
    int mg;
    Car(string a,int b,int c){
    NO=a;
    zk=b;
    mg=c;
    }
    void display(){
        int sum=zk*8+mg*2;
        cout<<NO<<" "<<sum<<endl;
    }
};
class Truck:public Vehicle{
public:
    int zk;
    int mg;
    Truck(string a,int c){
    NO=a;
    mg=c;
    }
    void display(){
        int sum=mg*5;
        cout<<NO<<" "<<sum<<endl;
    }
};
class Bus:public Vehicle{
public:
    int zk;
    int mg;
    Bus(string a,int b){
    NO=a;
    zk=b;
    }
    void display(){
        int sum=zk*3;
        cout<<NO<<" "<<sum<<endl;
    }
};
int main(){
    Vehicle *pv[10];
    int n,b,c,i=0;
    string a;
    cin>>n;
    while(n){
        cin>>a;
        if(n==1){
            cin>>b>>c;
            pv[i]=new Car(a,b,c);
        }
        if(n==2){
            cin>>c;
            pv[i]=new Truck(a,c);
        }if(n==3){
            cin>>b;
            pv[i]=new Bus(a,b);
        }
        pv[i]->display();
        i++;
        cin>>n;
    }
    delete *pv;
    return 0;
}

```

## 7-6 师生信息管理 (10分)

给出下面的一个基类框架
class Person
{
protected:
int NO;//编号
public:
virtual void display()=0;//输出相关信息
}
以Person为基类，构建出Student、Teacher两个类。
生成上述类并编写主函数，要求主函数中有一个基类Person指针数组，数组元素不超过10个。
Person *pp[10];
主函数根据输入的信息，相应建立Student, Teacher类对象，对于Student给出期末5门课的成绩（为整数，缺考的科目填-1）， 对于Teacher则给出近3年，每年发表的论文数量。
输入格式：每个测试用例占一行，第一项为人员类型，1为Student,2为Teacher.接下来为编号（0-9999），接下来Student是5门课程成绩，Teacher是3年的论文数。最后一行为0，表示输入的结束。
要求输出编号，以及Student缺考的科目数和已考科目的平均分(保留1位小数，已考科目数为0时，不输出平均分)，Teacher的3年论文总数。
提示：应用虚函数实现多态
输入样例
1 19 -1 -1 -1 -1 -1
1 125 78 66 -1 95 88
2 68 3 0 7
2 52 0 0 0
1 6999 32 95 100 88 74
0
输出样例
19 5
125 1 81.8
68 10
52 0
6999 0 77.8

### 代码

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;

class Person
{
protected:
    int NO;//编号
public:
    virtual void display()=0;//输出相关信息
    Person(int n){NO=n;}
    virtual ~Person(){};
};

class Student:public Person
{
    double pj;
    int num=0;
public:
    Student(int n,int n1,int n2,int n3,int n4,int n5):Person(n)
    {
        if(n1==-1) num++;
        else pj+=n1;
        if(n2==-1) num++;
        else pj+=n2;
        if(n3==-1) num++;
        else pj+=n3;
        if(n4==-1) num++;
        else pj+=n4;
        if(n5==-1) num++;
        else pj+=n5;
        pj=pj/(5-num);
    }

    virtual void display()
    {
        cout<<NO<<" "<<num;
         if(num!=5){
            cout<<" "<<setiosflags(ios::fixed)<<setprecision(1)<<pj;
         }
         cout<<endl;
    }
};

class Teacher:public Person
{
int num;
public:
    Teacher(int n,int n1,int n2,int n3):Person(n)
    {
        num=n1+n2+n3;
    }

    virtual void display()
    {
        cout<<NO<<" "<<num<<endl;
    }
};

int main()
{
    Person *pd[10];
    string s;
    int t,n,i=0,n1,n2,n3,n4,n5;
    cin>>t;
    while(t){
        cin>>n;
        if(t==1){
            cin>>n1>>n2>>n3>>n4>>n5;
            pd[i]=new Student(n,n1,n2,n3,n4,n5);
        }else if(t==2){
            cin>>n1>>n2>>n3;
            pd[i]=new Teacher(n,n1,n2,n3);
        }
        pd[i]->display();
        i++;
        cin>>t;
    }
    delete *pd;
    return 0;
}

```

## 7-7 数据的最大值问题（重载+函数模板） (20分)

两个类如下设计：类time有三个数据成员，hh，mm，ss，分别代表时，分和秒，并有若干构造函数和一个重载-（减号）的成员函数。类date有三个数据成员，year，month，day分别代表年月日，并有若干构造函数和一个重载>（<）（大于号或者小于号）的成员函数。
要求设计一个函数模板template <\class T>\ double maxn(T x[], int len) 对int，float，time和date或者其他类型的数据，返回最大值。
主函数有如下数据成员：
int intArray[100];
double douArray[100];time timeArray[100];
date dateArray[100];
其中，hh = 3600 ss， mm = 60 ss， year = 365 day， month = 30 day，对于time和date类型，数据在转换成ss或者day后进行运算。
输入格式：
每行为一个操作，每行的第一个数字为元素类型，1为整型元素，2为浮点型元素，3为time类型，4为date类型，若为整型元素，接着输入整型数据，以0结束。若为浮点型元素，接着输入浮点型数据，以0结束。若为time型元素， 输入time型数据（hh1 mm1 ss1 hh2 mm2 ss2），以0结束。若为date型数据，输入date型数据（year1 month1 day1 year2 month2 day2），以0结束。输入-1时表示全体输入结束。
输出格式：
对每次输入，输出一个最大值。
样例输入：
1 4 5 9 3 7 0
2 4.4 5.5 6.9 3.2 2.7 0
3 18 21 22 18 20 31 18 21 49 0
4 2013 5 14 2013 5 15 2013 4 1 0
-1
样例输出：
9
6.9
18 21 49
2013 5 15

### 代码

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
#define ture true
#define flase false
class Time
{
public :
    void setx(double a1,double b1,double c1)
    {
        h=a1;
        m=b1;
        s=c1;
    }
    bool operator >(Time b)
    {
        double sum1,sum2;
        sum1=h*3600+m*60+s;
        sum2=b.h*3600+b.m*60+b.s;
        if(sum1>sum2)
        {
            return ture;
        }
        else
            return flase;
    }
    friend ostream& operator <<(ostream&os ,Time&);
private:
    double h,m,s;
};
ostream& operator <<(ostream&os,Time&a)
{
    os<<a.h<<" "<<a.m<<" "<<a.s;
}
class Date{
public :
    void setx(double a1,double b1,double c1)
    {
        h=a1;
        m=b1;
        s=c1;
    }
    bool operator >(Date b)
    {
        double sum1,sum2;
        sum1=h*365+m*30+s;
        sum2=b.h*365+b.m*30+b.s;
        if(sum1>sum2)
        {
            return ture;
        }
        else
            return flase;
    }
    friend ostream& operator <<(ostream& ,Date&);
private:
    double h,m,s;
};
ostream& operator <<(ostream&os ,Date&a){
os<<a.h<<" "<<a.m<<" "<<a.s;
}
template <class T>
double dist(T a[],int n){
    T maxx=a[0];
    for(int i=1;i<n;i++){
        if(a[i]>maxx){
            maxx=a[i];
        }
    }
    cout<<maxx<<endl;
    return 0;
}

int main() {

     int n;
     cin>>n;
     int intArray[100];
     double douArray[100];
     Time timeArray[100];
     Date dateArray[100];
     while(n!=-1){
        if(n==1){
            int a,b=0;
            cin>>a;
            while(a){
                intArray[b]=a;
                b++;
                cin>>a;
            }

            dist(intArray,b);
        }
        else if(n==2){
            double a;int b=0;
            cin>>a;
            while(a){
                douArray[b]=a;
                b++;
                cin>>a;
            }
            dist(douArray,b);
        }
        else if(n==3){
            double a1,b1,c1;int b=0;
            cin>>a1;
            while(a1){
                cin>>b1>>c1;
                timeArray[b].setx(a1,b1,c1);
                cin>>a1;
            b++;
            }
            dist(timeArray,b);
        }
         else if(n==4){
            double a1,b1,c1;int b=0;
            cin>>a1;
            while(a1){
                cin>>b1>>c1;
                dateArray[b].setx(a1,b1,c1);
                cin>>a1;
            b++;
            }
            dist(dateArray,b);
        }
        cin>>n;
     }

      return 0;
}

```

## 7-8 数据的间距问题 (15分)

复数类Complex有两个数据成员：a和b, 分别代表复数的实部和虚部，并有若干构造函数和一个重载-（减号，用于计算两个复数的距离）的成员函数。 要求设计一个函数模板
template < class T >
double dist(T a, T b)
对int，float，Complex或者其他类型的数据，返回两个数据的间距。
以上类名和函数模板的形式，均须按照题目要求，不得修改
输入格式:
每一行为一个操作，每行的第一个数字为元素类型，1为整型元素，2为浮点型元素，3为Complex类型，若为整型元素，接着输入两个整型数据，若为浮点型元素，接着输入两个浮点型数据，若为Complex型元素，输入两个Complex型数据（a1 b1 a2 b2），输入0时标志输入结束。
输出格式:
对每个输入，每行输出一个间距值。
输入样例:
1 2 5
3 2 4 5 9
2 2.2 9.9
0

输出样例:
3
5.83095
7.7



### 听老师的话（2）

```c++
#include<cmath>
#include<cstdio>
#include<algorithm>
#include<string>
#include<vector>
#include<iomanip>
#include<iostream>
using namespace std;
typedef long long ll;
class point{

public :
    point(double a1,double b1,double c1){
        a=a1;
        b=b1;
        c=c1;
    }
    friend double operator - (point a, point b);
private:
    double a,b,c;

};
double operator - (point a, point b){

    return sqrt(pow(a.a - b.a, 2.0) + pow(a.b - b.b, 2.0)+ pow(a.c - b.c, 2.0));
}
template <class T>
double dist(T a, T b){
    return abs(a-b);
}

int main() {

     int n;
     cin>>n;
     while(n){
        if(n==1){
            int a,b;
            cin>>a>>b;
            cout<<dist(a,b)<<endl;
        }
        else if(n==2){
            float a,b;
            cin>>a>>b;
            cout<<dist(a,b)<<endl;
        }
        else if(n==3){
            double a1,b1,c1,a2,b2,c2;
            cin>>a1>>b1>>c1>>a2>>b2>>c2;
            point a(a1,b1,c1),b(a2,b2,c2);
            cout<<dist(a,b)<<endl;
        }
        cin>>n;
     }

      return 0;
}

```

## 7-9 组最大数 (15分)

设有n个正整数，将他们连接成一排，组成一个最大的多位整数。
如:n=3时，3个整数13,312,343连成的最大整数为34331213。
如:n=4时，4个整数7,13,4,246连接成的最大整数为7424613。
输入格式:
有多组测试样例，每组测试样例包含两行，第一行为一个整数N（N<=100），第二行包含N个数(每个数不超过1000，空格分开)。
输出格式:
每组数据输出一个表示最大的整数。
输入样例:
2
12 123
4
7 13 4 246

输出样例:
12312
7424613

### 代码

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
bool cmp(int a,int b)//自定义判断大小的函数
{
    int sum1,sum2;
    if(b<10){
        sum1=a*10+b;
    }else if(b<100){
        sum1=a*100+b;
    }else{
        sum1=a*1000+b;
    }
    if(a<10){
        sum2=b*10+a;
    }else if(a<100){
        sum2=b*100+a;
    }else{
        sum2=b*1000+a;
    }
    return sum1>sum2;
}
int main()
{
    int n;
    while(cin>>n){
        int i,num,j;
        vector<int>a;
        for(i=0;i<n;i++){
            cin>>num;
            a.push_back(num);
        }
        sort(a.begin(),a.end(),cmp);
        //用自定义的函数进行排序
        //比如两个数7,23，
        //判断723>237，所以7放在前边
        for(i=0;i<n;i++){
            cout<<a[i];
        }
        cout<<endl;

    }

    return 0;
}

```

## 7-10 对称排序 (15分)

你供职于由一群丑星作为台柱子的信天翁马戏团。你刚完成了一个程序编写，它按明星们姓名字符串的长度非降序（即当前姓名的长度至少与前一个姓名长度一样）顺序输出他们的名单。然而，你的老板不喜欢这种输出格式，提议输出的首、尾名字长度较短，而中间部分长度稍长，显得有对称性。老板说的具体办法是对已按长度排好序的名单逐对处理，将前者放于当前序列的首部，后者放在尾部。如输入样例中的第一个案例，Bo和Pat是首对名字，Jean和Kevin是第二对，余此类推。
输入格式:
输入包含若干个测试案例。每个案例的第一行含一个整数n（n>=1），表示名字串个数。接下来n行每行为一个名字串，这些串是按长度排列的。名字串中不包含空格，每个串至少包含一个字符。n=0为输入结束的标志。
输出格式:
对每一个测试案例，先输出一行“Set n”，其中n从1开始取值，表示案例序号。接着是n行名字输出，如输出样例所示。
输入样例:
7
Bo
Pat
Jean
Kevin
Claude
William
Marybeth
6
Jim
Ben
Zoe
Joey
Frederick
Annabelle
5
John
Bill
Fran
Stan
Cece
0

输出样例:
SET 1
Bo
Jean
Claude
Marybeth
William
Kevin
Pat
SET 2
Jim
Zoe
Frederick
Annabelle
Joey
Ben
SET 3
John
Fran
Cece
Stan
Bill

### 代码

```c++
#include<bits/stdc++.h>
using namespace std;
typedef long long ll;
#define ture true
#define flase false
int main()
{
    int n,i,j=1,k=1;
    cin>>n;
    while(n)
    {
        cout<<"SET "<<j++<<endl;
        string s;
        vector<string> a;
        for(i=0; i<n; i++)
        {
            cin>>s;//他给的已经按长度排好了
            a.push_back(s);

        }
        for(i=0; i<n; i=i+2)//输出第1.3.5..个
        {
            cout<<a[i]<<endl;

        }
        if(n%2==0)
        {
            i=n-1;//偶数个就是最后一个
        }
        else
        {
            i=n-2;//奇数个就是倒数第二个
        }
        for(; i>0; i=i-2)
        {
            cout<<a[i]<<endl;//倒着输出2.4.6....
        }
        cin>>n;
    }

    return 0;
}
```