# oracle-managerxt
#include <stdio.h>
#include<iostream>
using namespace std;
template<class T>
class MyArray
{
private:
	int m_nTotalSize;
	int m_nValidSize;
	T *m_pData;
public:
	MyArray(int nSize=3)
{
		m_pData=new T[nSize]; //创建动态数组
		m_nTotalSize=nSize;
		m_nValidSize=0;
}
	void Add(T value)
	{
		if(m_nValidSize<m_nTotalSize)
		{
			m_pData[m_nValidSize]=value;
			m_nValidSize++;
		}
		else
		{
			int i=0;
			T *tmpData=new T[m_nTotalSize];
			for (i=0;i<m_nTotalSize;i++)
			{
				tmpData[i]=m_pData[i];

			}
			delete []m_pData;
			m_nTotalSize*=2;
			m_pData=new T[m_nTotalSize];
			for (i=0;i<m_nValidSize;i++)
			{
				m_pData[i]=tmpData[i];
			}
			delete[]tmpData;
			m_pData[m_nValidSize]=value;
			m_nValidSize++;
		}
	}
	int GetSize()
	{
		return m_nValidSize;
	}
	T Get(int pos)
	{
		return m_pData[pos];
	}
	virtual ~MyArray()
	{

		if (m_pData!=NULL)
		{
			delete [] m_pData;
			m_pData=NULL;
		}
	}
};


int main(int argc, char * argv[])
{
	MyArray<int> obj;
	obj.Add(1);
	obj.Add(2);
	obj.Add(3);
	obj.Add(4);
	for(int i=0;i<obj.GetSize();i++)
	{
		//printf("%d\n",obj.Get(i));
		cout<<obj.Get(i)<<endl;
	}
	return 0;
}


2.2 
#include <iostream>
using namespace std;

template <class T>
T sum(T data[], int nSize)
{
	T sum=0;
	for (int i=0;i<nSize; i++)
		sum+=data[i];
	return sum;
}

int main()
{

	int data[]={1,2,3,4,5};
	cout<<sum(data,5)<<endl;
	return 0;
}




2.3 
#include <iostream>
using namespace std;



class CIntArray
{
	int a[10];
public: CIntArray()
		{
	    for(int i=0;i<10;i++)
	    {
	    	a[i]=i+1;
	    }
		}
   int GetSum(int times)
   {
	   int sum=0;
	   for (int i=0;i<10;i++)
	   {
		   sum+=a[i];
	   }
	   return sum*times;
	   }
};
class CFloatArray
{
	float f[10];
public: CFloatArray()
		{
	    for(int i=1;i<10;i++)
	    {
	    	f[i-1]=1.0f/i;
	    }
		}
   int GetSum(float times)
   {
	   float sum=0.0f;
	   for (int i=0;i<10;i++)
	   {
		   sum+=f[i];
	   }
	   return sum*times;
	   }
};





template <class T>
class CApply
{
public:
	float GetSum(T &t, float inpara)
	{
		return t.GetSum(inpara);
	}
};
int main()
{
	CIntArray intary;
	CFloatArray fltary;
	CApply<CIntArray> c1;
	CApply<CFloatArray> c2;
	cout<<c1.GetSum(intary,3)<<endl;
	cout<<c2.GetSum(fltary,3.2f)<<endl;
	return 0;
}

/* 2.3 使用萃取技术后*/


#include <iostream>
using namespace std;



class CIntArray
{
	int a[10];
public: CIntArray()
		{
	    for(int i=0;i<10;i++)
	    {
	    	a[i]=i+1;
	    }
		}
   int GetSum(int times)
   {
	   int sum=0;
	   for (int i=0;i<10;i++)
	   {
		   sum+=a[i];
	   }
	   return sum*times;
	   }
};
class CFloatArray
{
	float f[10];
public: CFloatArray()
		{
	    for(int i=0;i<10;i++)
	    {
	    	f[i-1]=1.0f/i;
	    }
		}
   int GetSum(float times)
   {
	   float sum=0.0f;
	   for (int i=0;i<10;i++)
	   {
		   sum+=f[i];
	   }
	   return sum*times;
	   }
};


template<class T>
class NumTraits
{

};


template<>
class NumTraits<CIntArray>
{
public:
	typedef int resulttype;
	typedef int inputpara;

};

template<>
class NumTraits<CFloatArray>
{
public:
	typedef float resulttype;
	typedef float inputpara;

};

template <class T>
class CApply
{
public:
	typedef typename NumTraits<T>::resulttype result;
	typedef typename NumTraits<T>::inputpara input;
	result GetSum(T &obj, input in)
	{
		return obj.GetSum(in);
	}
};





int main()
{
	CIntArray intary;
	CFloatArray fltary;
	CApply<CIntArray> c1;
	CApply<CFloatArray> c2;
	cout<<c1.GetSum(intary,3)<<endl;
	cout<<c2.GetSum(fltary,3.2f)<<endl;
	return 0;
}

例子

#include<iostream>
using namespace std;


template <typename T>
class TraitsHelper {

     
};
template <>
class TraitsHelper<int> {
	public:
     typedef int ret_type;
     typedef int par_type;
};
template <>
class TraitsHelper<double> {
	public:
     typedef double ret_type;
     typedef double par_type;
};

template <typename T>
class Test {
public:
     TraitsHelper<T>::ret_type Compute(TraitsHelper<T>::par_type d)
	 {
        
		 return sizeof(d);
	 };

private:
     T mData;
};

int main()
{
    
	Test<int>  t1;
	Test<double> t2;


	cout<<t1.Compute(10.0)<<endl;
    cout<<t2.Compute(10.0)<<endl;


	return 0;
}



模板与操作符重载

#include <iostream>
using namespace std;

template <class U,class V>
bool Grater(U const &u,V const &v)
{
    return u>v;
}

int main()
{
    cout<<Grater(2,1)<<endl;
    cout<<Grater(2.2,1.1)<<endl;
    cout<<Grater('a',10)<<endl;
    return 0;
}


关于 const：const修饰成员函数，则该成员函数不能修改对象的成员变量，也不能调用类中任何非const成员函数。

#include <iostream>
using namespace std;
class MyClass
{
	private:
	     //	mutable //在C++中,mutable也是为了突破const的限制而设置的。被mutable修饰的变量,将永远处于可变的状态

		int counter;
	public:
		MyClass() : counter(0) {}
		void Foo()
		{
			counter++;
			std::cout << "Foo" << std::endl;    
		}

		void Foo() const
		{
			//counter++;  //若去掉mutable，则必须注释
			std::cout << "Foo const" << std::endl;
		}

		int GetInvocations() const
		{
			return counter;
	    }
};

int main(void)
{
	    MyClass cc;
		const MyClass& ccc = cc;
		cc.Foo();
		ccc.Foo();
	//	std::cout << "The MyClass instance has been invoked " << ccc.GetInvocations() << " times" << endl;
}





完整例子
#include <iostream>
#include <string.h>
using namespace std;

class Student
{
private:
    char name[20];
    int grade;
public:
    Student(char name[],int grade)
    {
        strcpy(this->name,name);
        this->grade=grade;
    }
    bool operator>(const int &value)const
    {
        return this->grade>value;
    }
};

template <class U,class V>
bool Grater(U const &u,V const &v)
{
    return u>v;
}

int main()
{
    Student s("Raito",100);
    cout<<Grater(s,99)<<endl;
    return 0;
}


二分查找




#include <iostream>
using namespace std;

template <typename T1, typename T2>
int binary_search(T1 t[], int n, T2 value) {
	int left = 0;
	int right = n - 1;
	int mid = 0;
	bool hit = false;

	//while (1 && ( right >= left )) {
	while ( right >= left ) {
		mid = (left + right)/2;
		if (t[mid] == value) {
			hit = true;
			break;
		}
		if (t[mid] < value) {
			left = mid + 1;
		}
		if (t[mid] > value) {
			right = mid - 1;
		}
	}

	int pos = hit ? mid : -1;
	return pos;
}

int main(int argc, char **argv) {
	int s[5] = {0, 2, 6, 7, 10};

    cout << binary_search(s, sizeof(s), 7) << endl;
    cout << binary_search(s, sizeof(s), 8) << endl;
	return 0;
}





学生成绩查找

#include <cstring>
#include <iostream>
using namespace std;
class Student
{
	char name[20];
	int grade;	

	public:
		Student(char name[], int grade)
		{
		    strcpy(this->name, name); this->grade = grade;
		}

		bool operator > (const int &value) const {
			return this->grade > value;
		}

		bool operator == (const int &value) const {
			return this->grade == value;
		}

		bool operator < (const int &value) const {
			return this->grade < value;
		}
};

template <typename T1, typename T2>
int binary_search(T1 t[], int n, T2 value) {
	int left = 0;
	int right = n - 1;
	int mid = 0;
	bool hit = false;

	//while (1 && ( right >= left )) {
	while ( right >= left ) {
		mid = (left + right)/2;
		if (t[mid] == value) {
			hit = true;
			break;
		}
		if (t[mid] < value) {
			left = mid + 1;
		}
		if (t[mid] > value) {
			right = mid - 1;
		}
	}

	int pos = hit ? mid : -1;
	return pos;
}

int main(int argc, char **argv) {
	Student j0("jon", 0);
	Student j1("jon1", 10);
	Student j2("jon2", 20);
	Student j3("jon3", 30);
	Student j4("jon4", 40);
	Student j5("jon5", 50);
	Student j6("jon6", 60);
	Student j7("jon7", 70);
	Student j8("jon8", 80);
	Student j9("jon9", 90);

    Student c[10] = { j0, j1, j2, j3, j4, j5,j6, j7, j8, j9 };

	cout << binary_search(c, 10, 70) << endl;
	cout << binary_search(c, 10, 85) << endl;

	return 0;
}
