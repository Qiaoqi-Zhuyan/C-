### 变长数组

~~~~C++
class MyArray
{
public:
    // 1. 构造函数，s代表数组元素的个数
    MyArray(int s = 0):m_size(s)
    {
        if(s == 0)
            m_ptr = NULL;
        else
            m_ptr = new int[s];
    }

    // 2. 复制构造函数
    MyArray(const MyArray &a)
    {
        if(a.m_ptr == NULL)
        {
            m_ptr = NULL;
            m_size = 0;
        }
        else
        {
            m_ptr = new int[a.m_size];
            memcpy(m_ptr, a.m_ptr, sizeof(int)*a.m_size); // 拷贝原数组内容
            m_size = a.m_size;
        }
    }

    // 3. 拷贝构造函数
    ~MyArray()
    {
        if(m_ptr)
            delete [] m_ptr;
    }

    // 4. 重载赋值=运算符函数
    MyArray & operator=(const MyArray & a)
    {
        if(m_ptr == a.m_ptr)
            return *this;

        if(a.m_ptr == NULL)
        {
            if(m_ptr)
                delete [] m_ptr;

            m_ptr = NULL;
            m_size = 0;
            return *this;
        }

        if(m_size < a.m_size)
        {
            if(m_ptr)
                delete [] m_ptr;

            m_ptr = new int[a.m_size];
        }

        memcpy(m_ptr, a.m_ptr, sizeof(int)*a.m_size); // 拷贝原数组内容
        m_size = a.m_size;
        return *this;
    }

    // 5. 重载[]运算符函数
    int & operator[](int i)
    {
        return m_ptr[i];
    }

    // 6. 在数组的末尾加入一个新的元素
    void push_back(int v)
    {
        if(m_ptr) // 如果数组不为空
        {
            int *tmpPtr = new int[m_size + 1]; // 重新分配空间
            memcpy(tmpPtr, m_ptr, sizeof(int)*m_size); // 拷贝原数组内容
            delete [] m_ptr;
            m_ptr = tmpPtr;
        }
        else // 如果数组本来就是空的
        {
            m_ptr = new int[1];   
        }

        m_ptr[m_size++] = v; //加入新的数组元素
    }

    // 7. 获取数组的长度
    int length()
    {
        return m_size;
    }

private:
    int  m_size; // 数组元素的个数
    int* m_ptr;  // 指向动态分配的数组
};
~~~~



### iomanip

| setbase(n)                   | 设置整数为n进制(n=8,10,16) |
| ---------------------------- | ------------------- |
| setfill(n)                   | 设置字符填充，c可以是字符常或字符变量 |
| setprecision(n)              | 设置浮点数的有效数字为n位       |
| setw(n)                      | 设置字段宽度为n位           |
| setiosflags(ios::fixed)      | 设置浮点数以固定的小数位数显示     |
| setiosflags(ios::scientific) | 设置浮点数以科学计数法表示       |
| setiosflags(ios::left)       | 输出左对齐               |
| setiosflags(ios::right)      | 输出右对齐               |
| setiosflags(ios::skipws)     | 忽略前导空格              |

### 例子

~~~~C++
#include <iostream>
#include<istream>
#include<ostream>
using namespace std;
class Vehicle {
protected:
	string No;
public:
	virtual void display() = 0;
};


class Car :public Vehicle {
private:
	int m_capacity;
	int m_weight;
public:
	Car(const string no, const int capacity, const int weight) :m_capacity(capacity),m_weight(weight){
		No = no;
	}
	void display();
	friend istream& operator>>(istream& in, Car& car);
};
void Car::display() {
	cout <<No<<" " << m_capacity * 8 + m_weight * 2 << endl;
}
istream& operator>>(istream& in, Car& car) {
	in >>car.No >>car.m_capacity >> car.m_weight;
	return in;
}

class Truck :public Vehicle {
protected:
	int m_weight;
public:
	friend istream& operator>>(istream& in, Truck& truck);
	void display();
	Truck(const string no, const int weight) :m_weight(weight) {
		No = no;
	}
};
void Truck::display() {
	cout <<No<<" " << m_weight * 5 << endl;
}
istream& operator>>(istream& in, Truck& truck) {
	in >> truck.No>>truck.m_weight;
	return in;
}

class Bus :public Vehicle {
protected:
	int m_cacpcity;
public:
	void display();
	friend istream& operator>>(istream& in, Bus& bus);
	Bus(const string no, const int cacpcity) :m_cacpcity(cacpcity) {
		No = no;
	}
};
void Bus::display() {
	cout <<No<<" " << m_cacpcity * 3 << endl;
}
istream& operator>>(istream& in, Bus& bus) {
	in >> bus.No >> bus.m_cacpcity;
	return in;
}


int main() {
	int weight = 0, capcity = 0,mode = 0;
	string No = " ";
	cin >> mode;
	Vehicle* pv[10];
		while(mode)
		{
			switch (mode)
			{
			case 1: {
				cin >> No >> capcity >> weight;
				pv[1] = new Car(No, capcity, weight);
				pv[1]->display();
			}; break;
			case 2: {
				capcity = 0;
				cin >> No >> weight;
				pv[1] = new Truck(No, weight);
				pv[1]->display();
			}; break;
			case 3: {
				weight = 0;
				cin >> No >> capcity;
				pv[1] = new Bus(No, capcity);
				pv[1]->display();
			}
			default:
				break;
			}
			cin >> mode;
		}
}
~~~~



~~~~C++
#include<iostream>
#include<istream>
#include<ostream>
#include<iomanip>
using namespace std;
template <typename t>
class Mycomplex {
private:
	t m_real;
	t m_imag;
public:
	friend istream& operator>> <>(istream& in , Mycomplex<t>& complex);
	friend ostream& operator<< <>(ostream& out,const Mycomplex<t>& complex);
	Mycomplex<t> operator+(const Mycomplex<t>& complex);
	Mycomplex<t> operator-(const Mycomplex<t>& complex);
	Mycomplex<t> operator*(const Mycomplex<t>& complex);
	Mycomplex<t> operator/(const Mycomplex<t>& complex);
	Mycomplex<t> operator+(const t num);
	Mycomplex<t> operator-(const t num);
	Mycomplex<t> operator*(const t num);
	Mycomplex<t> operator/(const t num);
};
template <typename t>//重载输入
istream& operator>> <>(istream& in,  Mycomplex<t>& complex) {
	in >> complex.m_real >> complex.m_imag;
	return in;
}
template <typename t>//重载输出
ostream& operator<< <>(ostream& out, const Mycomplex<t>& complex) {
	out <<fixed<<setprecision(2) <<'(' <<complex.m_real << ',' << complex.m_imag << 'i' << ')' << endl;
	return out;
}
template <typename t>//重载加法
Mycomplex<t> Mycomplex<t>::operator+(const Mycomplex<t>& complex) {
	Mycomplex<t> m_complex;
	m_complex.m_real = this->m_real + complex.m_real;
	m_complex.m_imag = this->m_imag + complex.m_imag;
	return m_complex;
}
template <typename t>//重载减法
Mycomplex<t> Mycomplex<t>::operator-(const Mycomplex<t>& complex) {
	Mycomplex<t> m_complex;
	m_complex.m_real = this->m_real - complex.m_real;
	m_complex.m_imag = this->m_imag - complex.m_imag;
	return m_complex;
}
template <typename t>//重载乘法
Mycomplex<t> Mycomplex<t>::operator*(const Mycomplex<t>& complex) {
	Mycomplex<t> m_complex;
	m_complex.m_real = (this->m_real) * complex.m_real - (this->m_imag) * complex.m_imag;
	m_complex.m_imag = (this->m_real)*complex.m_imag + (this->m_imag)*complex.m_real;
	return m_complex;
}
template <typename t>//重载除法
Mycomplex<t> Mycomplex<t>::operator/(const Mycomplex<t>& complex) {
	Mycomplex<t> m_complex;
	t div = (complex.m_real * complex.m_real + complex.m_imag*complex.m_imag);
	m_complex.m_real = ((this->m_real) * complex.m_real - (this->m_imag) * complex.m_imag) / div;
	m_complex.m_imag = ((this->m_real) * complex.m_imag + (this->m_imag) * complex.m_real) / div;
	return m_complex;
}
template <typename t>//实数加法
Mycomplex<t> Mycomplex<t>::operator+(const t num) {
	Mycomplex<t> m_complex;
	m_complex.m_real = this->m_real + num;
	m_complex.m_imag = this->m_imag;
	return m_complex;
}
template <typename t>//实数减法
Mycomplex<t> Mycomplex<t>::operator-(const t num) {
	Mycomplex<t> m_complex;
	m_complex.m_real = this->m_real - num;
	m_complex.m_imag = this->m_imag;
	return m_complex;
}
template <typename t>//实数乘法
Mycomplex<t> Mycomplex<t>::operator*(const t num) {
	Mycomplex<t> m_complex;
	m_complex.m_real = this->m_real * num;
	m_complex.m_imag = this->m_imag * num;
	return m_complex;
}
template <typename t>//实数除法
Mycomplex<t> Mycomplex<t>::operator/(const t num) {
	Mycomplex<t> m_complex;
	m_complex.m_real = this->m_real / num;
	m_complex.m_imag = this->m_imag / num;
	return m_complex;
}
int main() {
	Mycomplex<double> c1;
	Mycomplex<double> c2;
	Mycomplex<double> c3;
	double num;
	cin >> c1 >> c2;
	cin >> num;
	c3 = c1 + c2;
	cout  <<"c1 + c2 = " << c3;
	c3 = c1 - c2;
	cout << "c1 - c2 = " << c3;
	c3 = c1 * c2;
	cout << "c1 * c2 = " << c3;
	c3 = c1 / c2;
	cout << "c1 / c2 = " << c3;
	c3 = c1 + num;
	cout << "c1 + num = "<< c3;
	c3 = c1 - num;
	cout << "c1 - num = " << c3;
	c3 = c1 * num;
	cout << "c1 * num = " << c3;
	c3 = c1 / num;
	cout << "c1 / num = " << c3;
	
	
}
~~~~



~~~~C++
#include<iostream>
using namespace std;
template<typename t>
class Vector {
private:
	t *m_data;
	int m_size;
public:
	Vector<t>():m_size(0){
		m_data = new t[m_size];
	}
	t add(t & i);
	int get_size();
	void remove(int i){}
	t& operator[](int i) {
		return m_data[i];
	}
};
template<typename t>
int Vector<t>::get_size() {
	return m_size;
}
template<typename t>
t Vector<t>::add(t & i) {
	if (m_data) {
		t* t_m_data = new t[m_size + 1];
		memcpy(t_m_data, m_data, sizeof(t) * m_size);
		delete[] m_data;
		m_data = t_m_data;
	}
	else
	{
		m_data = new t[1];
		
	}
	m_data[m_size++] = i;
	return i;
}

int main() {
	Vector<int> v;
	for (int i = 0; i < 10; i++)
		v.add(i);
	for (int i = 0; i < 10; i++)
		cout << v[i] << endl;
}
~~~~



~~~~C++
/* 学校选拔篮球队员，每间宿舍最多有 4 个人。现给出宿舍列表，
请找出每个宿舍最高的同学。定义一个学生类 Student，有身高 height，体重 weight 等。

输入格式 :
首先输入一个整型数 n （1≤n≤10 ^ 6），表示有 n 位同学。

紧跟着 n 行输入，每一行格式为：宿舍号 name height weight。
宿舍号的区间为[0, 999999]， name 由字母组成，长度小于 16，height，weight 为正整数。

输出格式 :
按宿舍号从小到大排序，输出每间宿舍身高最高的同学信息。题目保证每间宿舍只有一位身高最高的同学。

注意宿舍号不足 6 位的，要按 6 位补齐前导 0。
输入样例 :
7
000000 Tom 175 120
000001 Jack 180 130
000001 Hale 160 140
000000 Marry 160 120
000000 Jerry 165 110
000003 ETAF 183 145
000001 Mickey 170 115
输出样例:
000000 Tom 175 120
000001 Jack 180 130
000003 ETAF 183 145

*/
#include<iostream>
#include<cstdio>
#include<cstdlib>
#include<string>
#include<ostream>
using namespace std;
class student {
private:
	int m_height;
	int m_weight;
	char m_name[20];
	char m_dornum[10];
public:
	friend istream & operator>>(istream& in, student& stu);
	friend ostream & operator<<(ostream& out, student& stu);
	void getinfor(char name[], char dornum[], int weight, int height) {
		strcpy_s(m_name, name);
		strcpy_s(m_dornum, dornum);
		m_weight = weight;
		m_height = height;
	}
	char* getname() {
		return m_name;
	}
	int getdornum() {
		return stoi(m_dornum);
	}
	int getheight() {
		return m_height;
	}
	int getweight() {
		return m_weight;
	}
};
istream & operator>>(istream& in, student& stu) {
	in >> stu.m_dornum >> stu.m_name >> stu.m_height >> stu.m_weight;
	return in;
}
ostream& operator<<(ostream& out, student& stu) {
	out << stu.m_dornum << " " << stu.m_name << " " << stu.m_height << " " << stu.m_weight<<endl;
	return out;
}

int main() {

	int n;
	cin >> n;
	student* stu = new student[n];
	for (int i = 0; i < n; i++)
		cin >> stu[i];
	for (int i = 0; i < n; i++)
		for (int k = 0; k < n - 1 - i; k++) {
			student tape;
			if (stu[k].getdornum() > stu[k + 1].getdornum()) {
				tape = stu[k];
				stu[k] = stu[k + 1];
				stu[k + 1] = tape;
			}
		}
	for (int i = 0; i < n; i++)
		for (int k = 0; k < n - 1 - i; k++) {
			student t;
			if (stu[k].getheight() < stu[k + 1].getheight()) {
				t = stu[k];
				stu[k] = stu[k + 1];
				stu[k + 1] = t;
			}
		}
}
~~~~

~~~~C++

#include<iostream>
using namespace std;
template<typename t>
class Vector {
private:
	t *m_data;
	int m_size;
public:
	Vector<t>():m_size(0){
		m_data = new t[m_size];
	}
	Vector(const Vector& v) {
		this->m_size = v.m_size;
		this->m_data = (t*)calloc(this->m_size, sizeof(t));
		memcpy(this->m_data, v.m_data, m_size * sizeof(t));
	}
	t add(t i);
	int get_size();
	void remove(int m);
	t& operator[](int i) {
		return m_data[i];
	}
};
template<typename t>
int Vector<t>::get_size() {
	return m_size;
}
template<typename t>
t Vector<t>::add(t i) {
	if (m_data) {
		t* t_m_data = new t[m_size + 1];
		memcpy(t_m_data, m_data, sizeof(t) * m_size);
		delete[] m_data;
		m_data = t_m_data;
	}
	else
	{
		m_data = new t[1];
		
	}
	m_data[m_size++] = i;
	return m_size-1;
}
template<typename t>
void Vector<t>::remove(int m) {
	for (int i = m; i < this->get_size()-1; i++)
		this->m_data[i] = this->m_data[i + 1];
	m_size--;
}
int main() {
	Vector<int> vint;
	int n, m;
	cin >> n >> m;
	for (int i = 0; i < n; i++)
		vint.add(i);
	cout << vint.get_size() << endl;
	cout << vint[m] << endl;
	vint.remove(m);
	cout << vint.add(-1) << endl;
	cout << vint[m] << endl;
	Vector<int> vv = vint;
	cout << vv[vv.get_size() - 1] << endl;
	vv.add(m);
	cout << vint.get_size() << endl;
}
~~~~

