# 생성자
- 객체가 생성될 때 자동으로 호출되는 메서드
- 모든 클래스는 생성자를 보유함
- 객체가 생성될 때 하고싶은 명령을 생성자 안에 정의
- 보통은 필드를 초기화하는 작업을 정의함
- 생성자는 클래스의 이름을 사용하여 선언한다.
- 생성자의 머리부에는 반환 자료형을 표시하지 않는다.
- 또한 몸체 내에서 return 명령으로 값을 반환할 수 없다.
- public으로 선언한 생성자만 클래스 외부에서 객체를 만드는 데 사용할 수 있다.

## 생성자의 선언
```c
class 클래스명{
public:
  클래스명(매개변수){ -> 생성자

  }
}
```


### 생성자1.cpp
```c
#include <iostream>

using namespace std;

class A{
    public:
		//만약 개발자가 클래스에 생성자를 정의하지 않은 경우 컴파일러가 기본생성자를 추가해준다.
        //A(){};
};

class B{
    public:
        int a,b;

		//기본(default)생성자 : 매개변수가 없는 생성자
        B(){
            cout <<"B클래스의 기본생성자"<<endl;
            a = b = 0;
            print();
        }

        //생성자도 오버로딩이 가능하다.
        B(int c){
            cout << "두번째 생성자" <<endl;
            a = b = c;
            print();
        }

        B(int c,int d){
            cout << "세번째 생성자" <<endl;
            a = c; b = d;
            print();
        }

        void print(){
            cout <<"a="<< a << " b="<<b<<endl;
        }
};

int main(){
    A a;
    B b;
    B b2(10);
    B b3(20,30); //과거에 쓰던 방법
    B b4{40,50}; //최신 방법
	B b5 = {60,70}; //가능

    return 0;

}
```

### 생성자연습1.cpp
```c
//Student 클래스
//필드 : 학번, 이름, 국,영,수 점수
//생성자1 : 학번,이름을 인자로 받아 저장하고 국영수 점수를 0으로 초기화
//생성자2 : 학번 이름 정수를 인자로 받아 저장하고 해당 점수로 국영수 점수 초기화 하기
//생성자3 : 학번, 이름, 국,영,수 점수를 모두 인자로 받아 모든 필드 초기화하기
//학번 이름, 국,영,수 점수를 출력하는 print()메서드 생성

#include <iostream>
#include <string>

using namespace std;

class Student{
    public:
        Student(int idx, string name){
            this->idx = idx;
            this->name = name;
            kor = 0;
            eng = 0;
            mat = 0;
            print();
        }

        Student(int idx, string name, int score){
            this->idx = idx;
            this->name = name;
            kor = score;
            eng = score;
            mat = score;
            print();
        }

        Student(int idx, string name, int kor, int eng, int mat){
            this->idx = idx;
            this->name = name;
            this->kor = kor;
            this->eng = eng;
            this->mat = mat;
            print();
        }

        void print(){
            cout << "학번 : " << idx<< endl;
            cout << "이름 : " << name << endl;
            cout << "국어 : " << kor << endl;
            cout << "영어 : " << eng << endl;
            cout << "수학 : " << mat << endl;
        }

    private:
        int idx, kor,eng,mat;
        string name;
};

int main(){
    Student a{20240001,"jaems"};
    Student b{20240002,"wade",50};
    Student c{20240003,"paul",90,80,88};
}
```

# 소멸자
- 소멸자(destructor)는 객체가 소멸될 때 자동으로 실행되는 함수로서, 객체의 소멸에 따라 필요한 제반 처리를 하기 위한 코드가 포함된다.

```c
class 클래스명 {

  ……

public:

  ……

~ClassName() {   // 소멸자

  ……

}

};
```
1. 소멸자는 클래스의 이름에 ‘~’를 붙여 선언한다.
2. return 명령으로 값을 반환할 수 없으며, 함수 머리에 반환할 자료형을 표시하지 않는다.
3. 매개변수를 포함할 수 없다.
4. 소멸자는 다중정의할 수 없으며, 클래스에 하나만 정의한다.
5. public 멤버로 선언하는 것이 일반적이다.1)
6. 상속을 통해 파생 클래스를 정의하는 경우 virtual을 지정하여 가상함수가 되도록 하는 것이 좋다.

### 소멸자1.cpp
```c
//소멸자


#include <iostream>
using namespace std;
class A{
    public:
        A(){cout <<"생성자"<<endl;}
        ~A(){cout <<"소멸자"<<endl;}
};

int main(){
    A a;
    return 0;
}
```

### 생성자2.cpp
```c
#include <iostream>

using namespace std;

class A{
    public:
        int a,b;
        //A(int c,int d){
        //   a = c;
        //    b = d;
        //};

					//멤버 초기화 리스트
		A(int c,int d):a(c),b(d){};
};


int main(){
    //A a; 개발자가 직접 생성자를 하나라도 정의한 경우,
        // 컴파일러는 기본생성자를 추가하지 않음
        //-> 정의한 생성자 형식에 맞게 객체를 생성해야함

    A a{10,20};

    cout << a.a<<" "<<a.b<<endl;
    
    return 0;

}
```

### 생성자연습2.cpp
```c
#include <iostream>
#include <string>
using namespace std;

//Car클래스
//private멤버 : speed, gear -> int, color -> string
//public메서드 생성자 -> 세가지를 인수로 받아서 필드를 초기화
//           print() -> 생성자에서 호출
//멤버 초기화 리스트를 사용하기

class Car{
    private:
        int speed,gear;
        string color;

    public:
        Car(int speed, int gear, string color): speed(speed),gear(gear),color(color)
        {
            print();
        }
        void print(){
            cout << "speed : " << speed<<endl;
            cout << "gear : " << gear<<endl;
            cout << "color : " << color << endl;
            cout << "---------------"<<endl;
        }
};

int main(){
    Car a(100,5,"red");
    Car b(90,6,"blue");

    return 0 ;
}
```

# 복사 생성자
- 같은 클래스의 객체를 복사하여 객체를 만드는 생성자이다.

## 컴파일러가 자동적으로 제공하는 복사 생성자. 
- 만일 복사 생성자를 명시적으로 선언하지 않으면 컴파일러는 원본 객체의 멤버들을 그대로 복사하여 객체를 정의하는 복사 생성자를 자동으로 만든다.

### 복사생성자1.cpp
```c
//얕은복사(shallow copy) -> 객체의 주소만 복사
//깊은복사(Deep copy) -> 새로운 객체를 만들고 필드값을 복사
	//복사 생성자를 정의하면 깊은복사를 할 수 있다.

#include <iostream>
#include <string>

using namespace std;

class SimpleClass{
    private:
        int num1, num2;
    public:
        SimpleClass(int num1, int num2){
            this->num1 = num1;
            this->num2 = num2;
            cout<<"생성자 호출"<<endl;
        }
        //복사생성자의 정의
        //-> 매개변수가 다른 객체의 참조타입
        SimpleClass(const SimpleClass& copy){
			this->num1 = copy.num1;
            this->num2 = copy.num2;
		}

        void show(){
            cout<<"num1: "<<num1<<endl;
            cout<<"num2: "<<num2<<endl;
        }
};

int main(){
    SimpleClass cls1(100,200);
    SimpleClass cls2 = cls1; //얕은복사
    SimpleClass cls3(cls1); //깊은복사
    cout<<"=== 클래스 1의 멤버변수 값==="<<endl;
    cls1.show();
    cout<<"=== 클래스 2의 멤버변수 값==="<<endl;
    cls2.show();
    cout<<"=== 클래스 3의 멤버변수 값==="<<endl;
    cls3.show();
    
    return 0;
}
```
- 지금까지의 객체는 스택 영역에 생성이 됐다.

# 동적객체 생성
## 객체 포인터
- c언어의 포인터와 동일
- 객체의 주소값을 가지는 변수

### 포인터로 멤버에 접근할 때
- 객체포인터명 -> 멤버명

### 객체의 동적할당
- 필요한 양이 예측되지 않는 경우, 프로그램 작성시 할당받을 수 없다.
- 실행중 힙 메모리에서 할당하면 된다.
  - 힙(heap)으로부터 할당
    - 힙은, OS가 프로그램의 실행을 시작시킬 때(즉, 프로그램이 메모리에 올라가 프로세스가 될 때) 동적 할당 공간으로 준 메모리 공간이다
- C언어에서 동적 메모리 할당 : malloc() / free() 함수 사용
- C++의 동적 메모리 할당 / 반환
  - new 연산자
  - 기본 타입 메모리 할당, 배열 할당, 객체 할당, 객체 배열 할당
  - 객체의 동적 생성 : 힙 메모리로부터 객체를 위한 메모리 할당 요청
### 동적객체생성.cpp
```c
#include <iostream>
#include <string>

using namespace std;

class A{
    public:
        int a;
        A(){
            cout << "생성자" << endl;
        };
};

int main(){

    A a; //메모리의 스택영역에 할당

    A* b = new A(); //메모르의 힙영역에 할당
    //new -> 동적할당을 하는 예약어
    b-> a = 10;
    cout << b-> a<< endl;

    return 0;
}
```

# static 멤버 변수와 static 멤버 함수
- 멤버함수들에 대해서는 명령어들을 저장하는 프로그램 메모리가 필요하다.
- 멤버함수들을 위한 프로그램 메모리 공간은 클래스마다 하나씩만 있으면 된다.
- 그러나 데이터 멤버는 각각의 객체별 상태를 저장해야 하므로 클래스의 객체가 정의되면 그 객체의 데이터 멤버를 위해 고유한 메모리 공간이 할당된다.

![image](https://github.com/to7485/CppLang/assets/54658614/ecdc92e2-567e-4b88-b8c8-9a4ad6fb2d90)

- 그러나 때로는 데이터 멤버를 클래스에 속한 객체들이 함께 공유하는 것이 필요할 때가 있다.
- C를 비롯한 절차지향 언어들에서 이처럼 변수를 공유하기 위해 사용하는 것이 전역변수이다.
- 전역변수는 클래스의 객체들만 공유하는 것이 아니어서 프로그램의 해석을 어렵게 하는 요인으로 작용할 수 있다.

## static 변수
- C++ 언어에서 이러한 경우 사용할 수 있는 것이 static 데이터 멤버이다.
- 클래스를 선언할 때 static으로 선언한 데이터 멤버는 각각의 객체마다 개별적으로 존재하는 것이 아니라 전체 클래스에 대해 하나만 존재하며, 해당 클래스의 객체들이 그 데이터 멤버를 공유한다.
- static 데이터 멤버는 클래스의 외부에서 별도로 정의해야 한다.

![image](https://github.com/to7485/CppLang/assets/54658614/4eb096be-fcc7-468e-a4d0-2054a848f780)

### 정적멤버변수.cpp
```c
#include <iostream>
#include <string>

using namespace std;

//static 멤버변수
//-> 메모리의 데이터 영역에 할당
//-> 클래스의 모든 객체가 공유하는 변수
//-> 객체를 생성하지 않아도 저장공간이 있음
class Circle{
public:
    int radius;
    static int count;
    Circle(){
        radius = 0;
        radius++;
        count++;
        cout << radius <<" " <<count << endl;
    }
};
//static 데이터 멤버는 클래스의 외부에서 별도로 정의해야 한다.
int Circle::count = 0;


int main(){
    Circle c1;
    Circle c2;
    Circle c3;

    return 0;
}

```

### 정적멤버변수연습1.cpp
```c
#include <iostream>
#include <string>

using namespace std;

//Student클래스
//필드 : 학번, 이름
//이름은 생성자에서 인자로 받아서 채우고
//학번을 생성한 순서대로 20240001부터 1씩 증가
//학번, 이름을 생성자에서 출력

class Student{
public:
    int id;
	static int count;
    string name;
    Student(string name){
		count++;
        this->name = name;
        id = 20240000 + count;
        print();
    }

	void print(){
		cout << id <<" " << name <<end;
	}
};

int Student::count = 0;

int main(){
    Student s1("james"); // 20240001 james
    Student s2("wade"); // 20240002 wade
    Student s3("paul"); // 20240003 paul

    return 0;
}

```

### 정적변수연습2.cpp
```c
//Customer클래스
//필드 : 고객번호 1부터 순차적으로 증가
//      마일리지 1000으로 초기화
//      단 다섯번째 고객에게는 5000으로 초기화
//생성자: 고객번호와 마일리지 출력

#include <iostream>

using namespace std;

class Customer{
public:
    int cnum =0;
    static int count;
    int mile = 1000;
    Customer(){
        count++;
        cnum = cnum + count;
        if(cnum ==5){
            this->mile = 5000;
        }
        cout << cnum <<" " <<mile <<endl;
        cout <<"------------"<<endl;
    }
};

int Customer::count = 0;

int main(){

    Customer c1;
    Customer c2;
    Customer c3;
    Customer c4;
    Customer c5;
    Customer c6;
    Customer c7;

    return 0;

}

```

## static 함수
- static 멤버함수는 특정 객체에 대한 처리를 하는 것이 아니라 클래스의 이름으로 어떠한 작업을 수행하는 함수이다.
- 일반 멤버함수들은 객체가 정의되어야만 사용할 수 있지만, static 멤버함수는 객체가 정의되지 않아도 사용할 수 있다.
- static 멤버함수에서는 객체가 정의되지 않아도 존재하는 static 데이터 멤버만 액세스할 수 있고, 일반 데이터 멤버는 액세스할 수 없다.

## NamedObj 클래스 만들어보기
- 이름을 갖는 객체를 만들 수 있는 NamedObj 클래스를 정의해보자.
- 객체가 생성될 때 고유번호를 가지게 되는데, 이 번호는 NamedObj객체가 생성됨에 따라 1번부터 시작하여 차례로 부여되는 일련번호이다.
- 객체는 자기 자신의 일련번호와 이름을 출력할 수 있으며, 현재 존재하는 NamedObj클래스의 객체 수를 구할 수 있다.

### NamedObj 클래스의 메소드
|메서드|비고|
|----|-------|
|NamedObj(const char* s)|생성자(이름을 s로 초기화함)|
|~NamedObj()|소멸자|
|void display()|ID와 이름을 출력한다.|
|static int nObj()|현재 존재하는 객체의 수를 구한다.|

### NameObj 클래스의 속성
|속성|비고|
|----|-------|
|char* name|이름을 저장한다.|
|int id|ID번호를 저장한다.|
|static int nConstr|생성된 객체의 수|
|static int nDestr|소멸된 객체의 수|

## NamedObj.h
```c
#ifndef NAMEDOBJ_H_INCLUDED
#define NAMEDOBJ_H_INCLUDED
#include <iostream>
using namespace std;

class NamedObj {
	char *name;
	int id;

	//static 데이터 멤버
	static int nConstr;   // 생성된 객체의 수
	static int nDestr;    // 소멸된 객체의 수

public:
	NamedObj(const char *s);  // 생성자

	~NamedObj();			  // 소멸자

	void display() const {   // 객체의 속성 출력
		cout << "ID : " << id << " --> 이름 : " << name << endl;
	}
	static int nObj() {       //존재하는 객체 수 반환
		return nConstr - nDestr;
	}
};

#endif
```

## NamedObj.cpp
```c
#include <cstring>
#include "NamedObj.h"

NamedObj::NamedObj(const char *s)
{
	name = new char[strlen(s) + 1]; //문자열을 복사할 공간을 할당한다.
	strcpy(name, s);
	id = ++nConstr;  // 생성된 객체의 수를 증가시키고 이를 ID로 부여한다.
}

NamedObj::~NamedObj()
{
	++nDestr;   // 소멸된 객체의 수를 증가시킨다.
	delete[] name;
}

// static 데이터 멤버의 정의 및 초기화
int NamedObj::nConstr = 0;
int NamedObj::nDestr = 0;
```

## StaticDM.cpp
```c
#include <iostream>
#include "NamedObj.h"
using namespace std;

void f() {
	NamedObj x("Third"); //세 번째 객체 생성
	x.display(); //함수 반환 후 x는 소멸됨
}


void main() {
	cout << "NamedObj 클래스의 객체 수 : " << NamedObj::nObj() << endl;
	NamedObj a("First");  // 첫 번째 객체 생성
	NamedObj b("Second"); // 두 번째 객체 생성
	a.display();
	b.display();
	f();
	NamedObj c("Fourth");
	c.display();
	cout << "NamedObj 크래스의 객체 수: "<< NamedObj::nObj() << endl;
}
```

# 클래스의 활용

## 문자를 저장하는 스택 클래스 - CharStack
- 스택은 데이터를 저장하는 자료구조의 하나로서, 마치 밑이 막힌 원통에 구슬을 넣거나 꺼내는 것과 같은 방식으로 동작한다.
- 입력한 데이터는 차례로 스택에 쌓이며, 데이터를 꺼낼 때에는 입력된 순서의 역순, 즉 마지막에 넣은 데이터가 가장 먼저 인출(Last In First Out, LIFO)되는 특성을 갖는다.
- 데이터를 저장하는 연산을 push라고 부른다.
- 데이터를 꺼내는 연산을 pop이라고 한다.
- 이러한 방식으로 문자 데이터를 저장하는 스택을 클래스로 표현해 보고자 한다.

## CharStack클래스
- 문자를 최대 20개까지 저장할 수 있는 스택을 나타내는 클래스를 선언하라.
- 스택 객체는 데이터를 저장(push)하거나 인출(pop)할 수 있으며, 스택이 비어 있는지 가득 차 있는지 검사할 수 있다.
- 또한 이 스택을 이용하여 입력된 단어를 역순으로 출력하는 프로그램을 작성하라.

## CharStack클래스의 메서드
|메서드|비고|
|----|-------|
|CharStack()|생성자|
|bool chkEmpty()|스택이 비어있는지 검사|
|bool chkFull()|스택이 가득 차 있는지 검사함|
|bool push(char)|스택에 데이터를 저장함|
|char pop() | 스택에서 데이터를 삭제함|

## CharStack.h
```c
#ifndef CHARSTACK_H_INCLUDED
#define CHARSTACK_H_INCLUDED

class CharStack {
	//열거형은 사용하면 변수가 갖는 값에 의미를 부여할 수 있고 프로그램 가독성이 향상됩니다.
	//열거형은 명명된 정수형 상수의 집합으로 구성됩니다.
	//열거형을 선언하면 컴파일러는 열거형 멤버들을 정수형 상수로 인식합니다.
	enum {size = 20}; //스택의 크기
	int top;		//마지막 데이터를 가리키는 포인터
	char buf[size]; //스택의 저장공간

public:
	//데이터는 buf[19]로부터 시작하여 차례로 위에 쌓는다. 
	CharStack() : top{ size } {}; //생성자
	bool chkEmpty() const { //스택에 데이터가 없으면 true
		return top == size;
	}
	bool chkFull() const { //스택이 가득 차 있으면 true
		return !top;
	}
	bool push(char ch); //스택에 데이터를 넣음
	char pop();			//스택에서 데이터를 꺼냄
};


#endif
```

## CharStack.cpp
```c
#include <iostream>
#include "CharStack.h"
using namespace std;

bool CharStack::push(char ch) {
	if(chkFull()) {
		cout << "스택이 가득 차 있습니다." << endl;
		return false;
	}
	buf[--top] = ch;
	return true;
}

char CharStack::pop() {
	if (chkEmpty()) {
		cout << "스택에 데이터가 없습니다." << endl;
		return 0;
	}
	return buf[top++];
}
```
## CSMain.cpp
```c
#include <iostream>
#include "CharStack.h"
using namespace std;

void main() {
	CharStack chStack; //20개의 문자를 저장할 수 있는 스택
	char str[20];

	cout << "영어 단어를 입력하시오 : ";
	cin >> str;

	char* pt = str; //포인터로 문자열 시작 위치 가리킴
	while (*pt) {	//문자열의 끝이 아니라면 반복
		chStack.push(*(pt++)); //스택에 문자를 넣음
	}

	cout << "역순 단어 출력 : ";
	while (!chStack.chkEmpty()) { //스택이 비어 있지 않으면 반복
		cout << chStack.pop(); //스택에서 인출한 문자를 출력
	}
	cout << endl;
}
```


# friend 함수
- 프렌드를 쓰는 이유는 친구나 동료처럼 수평적인 관계의 클래스간의 멤버 변수를 공유해야 할 경우 주로 쓰인다.
- 예를 들면 하나의 클래스에서 다른 클래스의 내부 데이터에 접근 해야할 경우 프렌드를 써서 권한을 주는 경우를 예로 들수 있다.
- friend를 사용하면 자신의 개체뿐만이 아닌 다른 타입의 개체를 접근하는 것이 가능해지기 때문에 코드의 확장이 수월하게 이루어진다.
- 단 이렇게 프렌드를 사용할 경우 개발자의 입장에서는 개발에 편리하겠지만 캡슐화 파괴의 주범이 되어 설계가 꼬여버리는 경우가 생길 수 있습니다.
- 클래스 내부에서만 써야할 멤버들이 다른곳에서 계속 접근을 허용하게 되면 데이터 보호도 어렵고 캡슐화를 지향하는 객체지향적 설계라고 보기 어렵다.
- 프렌드는 가급적 남발하지 않고 필요한 경우에만 사용하는것이 좋다.

## friend 함수의 사용법
- friend는 함수나 클래스 선언 앞에 선언할 수 있다.
- 이렇게 friend를 선언한 함수나 클래스에서는 접근 제어 지시자(private, public, protected)의 영향을 받지 않는다.

## People.cpp
```c
#include <iostream>
#include <string>
using namespace std;

class People {
private:
	string name;
public:
	People(string name) { this->name = name; } //생성자

	friend void sayName(People p); //friend함수
};

void sayName(People p) {
	//friend 함수이므로
	//접근 제어 지시자의 영향을 받지않고 
	//private 멤버에 직접 접근이 가능
	cout << p.name << endl;
}

void main() {
	People p("홍길동");
	sayName(p);
}
```

# friend 클래스
- 클래스에도 friend 예약어를 사용할 수 있다.
- 이렇게 하면 friend로 선언한 클래스의 전체가 접근 제어 지시자의 영향을 받지 않게 되므로 friend로 선언된 클래스의 모든 멤버와 메서드에 접근이 가능하다.

## Person.cpp
```c
#include <iostream>
#include <string>
using namespace std;

class PeopleA {
private:
	string name;
	int height;
	friend class PeopleB; //friend 클래스 선언
public:
	//생성자
	PeopleA(string name, int height) {
		this->name = name;
		this->height = height;
	}
};

class PeopleB {
public:
	//friend 선언으로 인해 PeopleA의 private 멤버에 접근가능
	void info_A(PeopleA a) {
		cout << "이름 : " << a.name << endl;
		cout << "신장 : " << a.height << endl;
	}
};

void main() {
	PeopleA a("홍길동", 170);
	PeopleB b;
	b.info_A(a);
}
```




















