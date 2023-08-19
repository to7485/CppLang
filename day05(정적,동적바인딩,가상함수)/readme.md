# 파생 클래스와 포인터
- 기초 클래스의 포인터는 파생 클래스의 객체를 가리킬 수 있다.
- Person에 대한 포인터는 Student에 대한 객체를 가리킬 수 있는 것이다.
- 파생 클래스에 대한 포인터는 기초 클래스의 객체를 가리킬 수 없다.

## StudentMain2.cpp 에 코드 추가하기
```c
#include <iostream>
#include "Person2.h"
#include "Student2.h"
using namespace std;

void main() {
	Student harry("Harry", "Hogwarts");
	cout << harry.getName() << "goes to " << harry.getSchool() << endl;

	Person* pPrsn1 = new Person("Dudley");

	Student* pStdnt1 = new Student("Harry", "Hogwarts");
/////////////////////////////////////
	Person *pPrsn1, *pPrsn2;
	Person dudley("Dudley");
	Student harry("Harry", "Hogwarts");
	Student *pStdnt;
	pPrsn1 = &dudley; //기초 클래스의 포인터가 기초 클래스의 객체를 가리키므로 문제 없음
	pPrsn2 = &harry; //기초 클래스의 포인터가 파생 클래스 객체를 가리키므로 문제 없음
	pStdnt = &dudley; //파생클래스의 포인터가 기초 클래스의 객체를 가리키므로 에러이다.
/////////////////////////////////////
}
```
- 파생 클래스의 포인터를 가지고 접근할 수 있는 멤버에는 기초 클래스에 선언되어 있지 않은 파생 클래스의 특수한 멤버들이 포함될수 있기 때문에 문제가 된다.

- 기초 클래스의 포인터로 객체를 가리키게 하고, 다형성을 활용하여 일반화된 처리를 하는 예는 객체 지향 언어를 사용하는 프로그램에서 흔히 볼 수 있다.

## 객체 포인터 배열
- Person 및 Student 클래스의 객체를 가리키는 포인터를 저장하는 배열을 선언하여 객체를 가리키게 하고
- 배열에 저장된 객체들을 출력하는 함수를 통해 출력하는 프로그램 작성하기

![image](https://github.com/to7485/CppLang/assets/54658614/985feab0-4724-4d8b-8910-e647a3b06449)

## Person.h
```c
#ifndef PERSON_H_INCLUDED
#define PERSON_H_INCLUDED
#include <iostream>
#include <string>
using namespace std;

class Person {
	string name;
public:
	Person(const string&n):name(n){}
	string getName() const { return name; }
	void print() const { cout << name; }
};

#endif // !PERSON_H_INCLUDED
```

## Student.h
```c
#ifndef STUDENT_H_INCLUDED
#define STUDENT_H_INCLUDED
#include <iostream>
#include <string>
#include "Person.h"

//Person의 파생 클래스 Student를 정의한다.
class Student : public Person {
	string school;
public:
	Student(const string&n, const string&s) :
		Person(n), school(s) {}
	string getSchool() const { return school; }
	void print() const {
		Person::print();
		cout << " goes to" << school;
	}
};

#endif // !STUDENT_H_INCLUDED
```

## PArrMain.cpp
```c
#include <iostream>
#include "Person.h"
#include "Student.h"
using namespace std;

void PrintPerson(const Person* const p[], int n) {
	for(int i = 0; i < n; i++) {
		p[i]->print();
		cout << endl;
	}
}

void main() {
	Person dudley("Dudley");
	Student harry("Harry", "Hogwarts");
	Student ron("Ron", "Hogwars");

//Person클래스와 Student클래스의 객체를 통해 멤버함수 print()를 호출하고 있다.
	dudley.print(); cout << endl;
	harry.print(); cout << endl << endl;

//기초클래스인 Person의 포인터배열 pPerson을 선언하여 Person객체인 dudley와
//파생클래스 Student의 객체인 harry와 ron의 포인터를 저장하였다.
//pPerson은 기초 클래스인 Person클래스의 객체를 가리키는 포인터 배열이므로 Person객체나
//Student의 객체를 모두 가리킬 수 있다.
	Person *pPerson[3];
	pPerson[0] = &dudley;
	pPerson[1] = &harry;
	pPerson[2] = &ron;
	PrintPerson(pPerson, 3);
}

Dudley
Harry goes toHogwarts

Dudley
Harry
Ron
```

- 기초클래스의 포인터를 이용하여 객체를 사용할 경우 기초 클래스의 멤버만을 사용되는 것을 볼 수 있다.
- 그런데 실제로 연결된 객체는 파생 클래스의 객체일 수도 있으며, 이 경우 호출된 print()는 객체에 맞는 것이 아니다.
- 즉, 포인터에 연결된 실제 객체에 맞게 멤버 함수가 선택됨으로써 다음과 같이 원하는 결과가 나오길 바랄뿐이다.

![image](https://github.com/to7485/CppLang/assets/54658614/66a7881b-29ba-4d58-98b5-3dbb95bde7a7)



# 가상함수
- 앞에서 기초 클래스의 포인터는 파생 클래스를 가리킬 수 있다고 하였다.
- 기초 클래스와 파생 클래스의 멤버함수가 같은 이름을 갖고 있고, 기초 클래스의 포인터를 통해 이름이 같은 함수를 호출할 때,
- 어느 클래스의 함수가 동작하는지에 관한 문제가 남아 있다.

## 정적 연결
- 포인터 혹은 참조에 의하여 함수가 호출될 때, 일반적으로 포인터(또는 참조)의 유형(type)에 따라 호출되는 멤버함수가 결정된다.
- 이러한 방식을 정적 연결(static binding)이라고 하며, 프로그램이 컴파일될 때 실행할 멤버함수가 결정된다.

## SBinding.cpp
```c
#include <iostream>
#include "Person.h"
#include "Student.h"
using namespace std;

void main() {
	Person *p1 = new Person("Dudley");
	p1->print(); //Person::print()호출

	cout << endl;
	Person *p2 = new Student("Harry", "Hogwarts");
	p2->print(); cout << endl; //Person::print()호출
	((Student*)p2)->print(); //Student::print() 호출
	cout << endl;
}
```
- 이 예에서는 Person 클래스의 포인터에 Student 객체가 연결되어 있다는 사실을 알고 있으니까 문제가 없었지만
- 실제로 활용되는 응용에서는 포인터에 연결된 것이 Person 객체일 수도 있고 Student 객체일 수도 있기 때문이다.
- Student 객체가 연결되어 있는데 13행과 같이 형 변환하여 멤버함수를 호출하면 실행과정에서 심각한 문제가 발생할 수 있다.

## 동적 연결과 가상함수
- 프로그램이 실행될 때 실제 객체에 따라 멤버함수를 결정하는 방법을 동적 연결(dynamic binding)이라고 한다.
- 정적 연결에서는 컴파일할 때 포인터의 유형에 따라 미리 결정된 클래스의 멤버함수가 수행되는 반면
- 동적 연결에서는 실행 중에 포인터가 가리키는 실제의 객체의 클래스가 보유하고 있는 멤버함수가 선택되어 수행된다.
- C++ 언어에서 동적 연결을 구현하는 방법이 가상함수(virtual function)이다.
- 가상함수는 다음과 같이 기초 클래스의 함수 앞에 예약어 virtual을 붙여 표현한다.
```c
virtual ReturnType functionName(fParameterList);

※ ReturnType : 멤버함수의 반환 자료형

※ functionName : 멤버함수의 이름

※ fParameterList : 멤버함수의 형식 매개변수 목록
```

## Person.h의 코드 수정하기
```c
#ifndef PERSON_H_INCLUDED
#define PERSON_H_INCLUDED
#include <iostream>
#include <string>
using namespace std;

class Person {
	string name;
public:
	Person(const string&n):name(n){}
	string getName() const { return name; }
////////////////////////////////////////////////
	virtual void print() const { cout << name; }
////////////////////////////////////////////////
};

#endif // !PERSON_H_INCLUDED

Dudley

Harry goes to Hogwarts

Dudley

Harry goes to Hogwarts

Ron goes to Hogwarts
```
- PArrMain.cpp 의 함수 PrintPerson()에서 형식 매개변수 p[]가 Person 클래스의 포인터의 배열이므로
- 실제로 연결된 객체가 무엇이든 관계없이 Person 클래스의 print()가 호출되었다.
- 그러나 이제는 p[]의 원소들이 가리키는 객체가 Person 객체라면 Person 클래스의 print()가, Student 객체라면 Student 클래스의 print()가 호출된 것을 볼 수 있다.
- 이와 같이 가상함수를 사용하여 동적 연결을 하면 포인터에 연결된 객체에 맞게 자동적으로 멤버함수가 선택된다.

# 캐스팅
- 객체지향에서 캐스팅이란 형변환을 의미한다.
- 업 캐스팅, 다운 캐스팅은 상속 관계에 있는 객체 간에 일어난다.
- 상속 관계의 클래스들은 다형성에 의하여 서로의 객체 포인터가 다른 타입의 객체를 가리킬 수 있다.

## 업캐스팅
- 파생 클래스를 기초 클래스로 변환하는 것을 업캐스팅(up-casting)이라고 한다.

```c
상위클래스* 클래스명 = new 하위클래스;
```

## Upcasting.cpp
```c
#include <iostream>
using namespace std;

class Base {
public:
	Base() {}
	void print_base() {
		cout << "Base\n";
	}
};

class Derived : public Base {
public:
	Derived() { }

	void print_derived() {
		std::cout << "Derived\n";
	}
};

int main() {
	Derived derived;
	Derived* pDerived = &derived;

	Base* pBase = pDerived;
	//기초 클래스가 파생 클래스 객체를 가리키고 있다고 파생 클래스를 그 자체로 사용할 수 있는 것은 아니다.
	//pBase 객체가 Base의 포인터라고 알고 있기 때문에 Base의 멤버만 접근하게 된다.
	//pBase->print_derived();
}
```

## 다운캐스팅
- 기초 클래스를 파생 클래스로 변환하는 것을 다운캐스팅(down-casting)이라고 한다.

```c
하위클래스* 클래스명 = (하위클래스*)업캐스팅된 상위클래스변수;
```

## DownCasting.cpp
```c
#include <iostream>

class Base {
public:
	Base() { }

	void base_func() {}		// +
	virtual void print() {
		std::cout << "Base\n";
	}
};

class Derived : public Base {
public:
	Derived() { }

	void derived_func() {}	// +
	void print() {
		std::cout << "Derived\n";
	}
};

int main() {
	Derived derived;
	Derived* pDerived = &derived;

	Base* pBase = pDerived;
	pBase->print();
	//pBase 객체는 Derived 클래스의 멤버를 사용할 수 없다. 
	//pBase 객체가 derived_func() 함수를 호출하려 하면 에러를 일으킨다.
	pBase->derived_func();

	//이때 pBase 객체에 Derived * 타입으로의 명시적 형변환을 달고 Derived *로 강제 형변환을 해줄 수 있는데,
	//이것이 다운 캐스팅이다.
	Derived* down_object = (Derived*)pBase;

	//이렇게 되면 pBase에서 접근할 수 없었던 Derived 클래스의 멤버에 접근할 수 있게 된다.
	down_object->derived_func();
}
```
# 멤버함수의 재정의
- 다음 멤버들은 고유한 처리를 담당하기 때문에 상속의 대상에서 제외된다.
    - 생성자와 소멸자
    - 대입 연산자
    - 정적 멤버
    - friend 관계지정
## 메서드 오버라이딩의 규칙
-기초 클래스에서와 동일한 이름, 매개변수, 반환 자료형을 갖는 멤버함수를 파생 클래스에서 정의해야 한다.

```c
#include <iostream>
using namespace std;

class Base {
public:
	void f() {
		cout << "Base::f() called" << endl;
	}
};

class Derived : public Base {
public:
	void f() {
		cout << "Derived::f() called" << endl;;
	}
};



void main() {
	Derived d, *pDer;//파생클래스 Derived의 객체 d생성, 포인터 pDer선언
	pDer = &d; //파생 클래스 포인터 -> 파생클래스 객체

	pDer->f(); //Derived의 멤버 f()호출
	pDer->Base::f(); //Base의 멤버 f()호출

	//업캐스팅을 통한 기본클래스의 포인터 -> 파생클래스 객체
	Base *pBase = pDer; //업캐스팅
	pBase->f();

}
```

# 추상화
- 계층구조를 갖는 객체들을 클래스로 표현할 때 실제로 객체가 존재하지 않는 추상적인 개념에 대한 클래스가 필요한 경우가 있다.
- 클래스 계층구조를 설계할 때 상위 계층의 클래스는 일반적인 데이터 멤버와 멤버함수를 포함하도록 하고, 하위 계층의 클래스는 상위 계층의 클래스가 가지고 있는 데이터 멤버와 멤버함수 외에 그 클래스에 필요한 고유한 데이터 멤버와 멤버함수를 포함하도록 한다.

## 순수 가상함수
- 동일한 개념의 동작을 정의한 멤버함수이지만 객체가 속한 클래스에 따라 고유한 동작을 할 수 있어야 하는 경우 가상함수를 이용할 수 있다.
- 기초 클래스에서 이러한 가상함수를 만들 때 몸체가 없이 선언만 할 수도 있다. 이처럼 몸체가 없는 가상함수를 순수가상함수(pure virtual function)라고 한다.
```c
virtual 반환형 함수명(매개변수) = 0;
```
- 문장의 끝에 '= 0'이라고 작성한다. 이것은 0이라는 값과 관계가 있는 것은 아니고, 단지 이 함수가 순수가상함수라는 것을 나타내기 위한 문법적 표현에 불과하다.
 
## 추상 클래스
- 어떠한 클래스가 순수가상함수를 포함하고 있다면 그 클래스는 객체를 만들 수 없다.
- 아직 정의되지 않은 멤버함수가 있기 때문이다. 이러한 클래스를 추상 클래스(abstract class)라고 한다.
- 만일 파생 클래스에서 재정의하지 않은 기초 클래스의 순수가상함수가 있다면, 또는 그 파생 클래스에서 새로운 순수가상함수를 선언하여 아직 구체적으로 정의하지 않은 멤버함수가 존재한다면 그 파생 클래스 역시 추상 클래스이다.

## 상세 클래스
- 모든 순수가상함수가 재정의되어 더 이상 순수가상함수가 포함되지 않은 파생 클래스는 이제 실체가 있는(concrete) 대상을 표현하는 클래스로서, 이를 상세 클래스(concrete class)라고 한다.

## virtual.cpp
```c
#include <iostream>
using namespace std;

class AClass { //추상클래스
public:
	virtual void vf() const = 0; //순수가상함수
	void f1() const {
		cout << "Abstract" << endl;
	}
};

class CClass : public AClass { //상세 클래스
public:
	void vf() const {
		cout << "순수가상함수 구현" << endl;
	}
	void f2() const {
		cout << "Concrete" << endl;
	}
};

void main() {
	//AClass objA; 추상클래스는 객체를 만들 수 없다
	CClass objC;// 상세 클래스인 CClass의 객체를 만듦

	objC.vf();	// 재정의한 함수 vf() 호출

	objC.f1();	// 기초 클래스인 AClass의 멤버함수 호출

	objC.f2();	// CClass의 멤버함수 호출
}
```
## 추상클래스의 활용
- 클래스 계층구조를 설계할 때 기초 클래스에서 구체적으로 구현할 수는 없다.
- 파생 클래스에서 반드시 구현하여 객체가 그 기능을 반드시 가지고 있게 만들 필요가 있다면
- 기초 클래스에서 이를 순수가상함수로 선언하여 추상 클래스로 만들면 된다.

## 도형 그리기
- 원, 삼각형 등은 모두 도형이다.
- 도형은 원이나 삼각형과 같은 구체적인 도형들을 통칭하는 것이므로 단지 도형이라고 하면 실체가 없는 추상적인 개념이다.
- 도형은 그 모양이 있으므로 원이나 삼각형 등의 구체적인 도형은 그 모양을 그리는 방법을 정할 수 있다.
- 추상적인 개념인 도형은 실제로 그리는 방법을 정할 수는 없지만 도형에 속하는 원이나 삼각형이 각각 자신을 그리는 방법을 정의해야 한다는 것을 정해 놓을 수는 있다.
    - 기초클래스 : '도형'
    - 파생클래스 : '원,'삼각형'
- '도형' 클래스는 '그린다'라는 행위가 필요하지만 아직 그 방법을 정의할 수는 없으므로 이를 순수가상함수로 선언하며, '도형' 클래스는 추상 클래스가 된다.
- 파생 클래스인 '원'이나 '삼각형'은 기초 클래스의 순수가상함수로 선언된 '그린다'라는 행위를 실재로 구현하는 '그린다'라는 함수를 재정의해야 하기 때문에 상세 클래스가 된다.
- 그렇지 않으면 그 클래스에 여전히 구현되지 않은 순수가상함수가 있으므로 추상 클래스가 되어 그 클래스의 객체를 만들 수 없다.

## 예제
- 2차원 도형에 해당되는 원을 나타내는 클래스와 삼각형을 나타내는 클래스를 선언하고자 한다.
- 이 클래스들은 모두 공통적으로 도형이므로 도형을 그리기 위한 선의 색과 도형 내부를 채워 칠하기 위한 색을 속성으로 가지고 있어야 한다.
- 이러한 속성을 이용하여 그리는 방법을 설명할 수 있어야 한다. 또한 그래픽 객체를 (dx, dy)만큼 이동할 수 있으며, 2차원 좌표 원점을 기준으로 확대/축소하는 크기 조정을 할 수 있다.
- 프로그램은 현재 속성이라는 객체가 있다.
- 속성 객체는 선의 색과 내부 영역을 칠하기 위한 색을 표현하며, 그 값을 설정하거나 읽어낼 수 있다.
- 도형 객체를 만들면 현재 속성에 따라 만들어진다. 또한 도형 객체의 선 색 및 채우기 색을 변경할 수 있다.

## GrAttrib 클래스
### GrAttrib클래스의 메서드
|메서드|비고|
|-----|-----|
|GrAttrib()|생성자. 선 색은 검정, 내부 영역은 흰색이 디폴트이다.|
|void setLineColor(const string&)|지정된 색으로 선 색 지정|
|void setFillColor(const string&)|지정된 색으로 내부 영역 색 지정|
|string getLineColor()|선 색 반환|
|string getFillColor()|내부 영역 색 반환|

### GrAttrib클래스의 속성
|속성|비고|
|-----|-----|
|string lineColor|선 색 속성|
|string fillColor|내부 영역 색 속성|

### GrAttrib.h
```c
#ifndef GRATTRIB_H_INCLUDED
#define GRATTRIB_H_INCLUDED
#include <string>
using namespace std;

class GrAttrib {	//그래픽 속성
	string lineColor; //선 색 속성
	string fillColor; //내부 영역 색 속성
public:
	GrAttrib() : lineColor("검정색"), fillColor("흰색") {};
	GrAttrib(const string&lc, const string &fc) :lineColor(lc), fillColor(fc) {};

	//속성 지정 멤버함수
	void setLineColor(const string &lc) {
		lineColor = lc;
	}

	void setFillColor(const string &fc) {
		fillColor = fc;
	}

	//속성 값을 읽는 멤버함수
	string getLineColor() const{
		return lineColor;
	}

	string getFillColor() const {
		return fillColor;
	}
};

extern GrAttrib curAttrib; //현재 속성을 나타내는 전역 객체

#endif // !GRATTRIB_H_INCLUDED
```

## Figure 클래스
- 원과 삼각형은 모두 2차원 도형에 속하는 도형 유형이다.
- 공통적으로 각각의 도형마다 그리기 위한 속성(선 색, 내부 영역 색)이 있고 이러한 속성 값을 지정할 수 있다.
- 도형을 이동하고 크기 조정을 하고 그릴 수 있다.
- 원과 삼각형을 일반화한 '도형'을 나타내는 Figure라는 기초 클래스를 선언한다.
- 이때 도형의 이동, 크기 조정, 그리기 등은 그 도형이 구체적으로 어떠한 도형인지가 결정되어야 구현할 수 있다.
- 도형이라면 그러한 메소드가 반드시 필요하기 때문에 도형의 파생 클래스로 선언할 원과 삼각형 클래스에서는 이러한 메소드를 반드시 구현하게 하려고 한다.
- 이 메소드들을 Figure 클래스의 순수가상함수로 선언한다.

### Figure클래스의 메서드
|메서드|비고|
|-----|-----|
|Figure()|생성자. 현재 속성에 따라 도형을 생성|
|void setLineColor(const string&)|지정된 색으로 선 색 지정|
|void setFillColor(const string&)|지정된 색으로 내부 영역 색 지정|
|void move( double dx, double dy)| 도형을 (dx,dy)만큼 이동|
|void scale(double s) | 도형을 원점 기준으로 s배 크기 조정|
|void draw()|도형을 그림|

### Figure클래스의 속성
|속성|비고|
|-----|-----|
|GrAttrib attrib|도형의 그래픽 속성|

### Figure.h
```c
#ifndef FIGURE_H_INCLUDED
#define FIGURE_H_INCLUDED
#include <iostream>
#include "GrAttrib.h"

class Figure {
protected:
	GrAttrib attrib; //그래픽 속성
public:
	//현재 그래픽 속성에 따라 도형객체 생성
	Figure() : attrib(curAttrib) {};

	//선 색 속성 지정
	void setLineColor(const string &c) {
		attrib.setLineColor(c);
	}

	//내부 영역의 색 지정
	void setFillColor(const string &c) {
		attrib.setFillColor(c);
	}

	//도형의 이동, 원점 기준 크기 조정, 그리기 멤버함수
	virtual void move(double dx, double dy) = 0;
	virtual void scale(double s) = 0;
	virtual void draw() const = 0;
};
#endif // !FIGURE_H_INCLUDED
```

## Circle클래스
- 원을 나타내는 Circle 클래스는 Figure로부터 상속받는 속성 외에 원의 중심 좌표와 반경을 나타내는 속성이 필요하다.
- Figure 클래스에서 순수가상함수로 선언하여 놓은 move(), scale(), draw()를 각각 재정의하여 구현한다.
### Circle클래스의 메서드
|메서드|비고|
|-----|-----|
|Circle(double x, double y, double r)|생성자|
|void move(double dx, double dy) | 원을(dx,dy)만큼 이동|
|void scale(double s) | 원을 원점 기준으로 s배 크기 조정|
|void draw()|원을 그림|

### Circle클래스의 속성
|속성|비고|
|-----|-----|
|double cx, cy|원의 중심좌표|
|double radius|원의 반경|

### Circle.h
```c
#ifndef CIRCLE_H_INCLUDED
#define CIRCLE_H_INCLUDED
#include <iostream>
#include "Figure.h"
using namespace std;

class Circle : public Figure {
	double cx, cy; //원의 중심 좌표
	double radius; //원의 반경
public:
	//현재 그래픽 속성에 따라 원 객체 생성
	//(x,y):중심 좌표
	//r:반경
	Circle(double x, double y, double r) :cx(x), cy(y), radius(r) {};

	//원의 이동, 원점 기준 크기 조정, 그리기 멤버 함수
	void move(double dx, double dy);
	void scale(double s);
	void draw() const;
};
#endif // !CIRCLE_H_INCLUDED
```
### Circle.cpp
```c
#include <iostream>
#include <string>
#include "Circle.h"
using namespace std;

//원의 중심 좌표를 (dx,dy)만큼 이동
void Circle::move(double dx, double dy) {
	cx += dx;
	cy += dy;
}

//좌표 원점을 기준으로 s배 크기 조정
void Circle::scale(double s) {
	cx *= s;
	cy *= s;
	radius *= s;
}

//원을 그리는 방법 출력
void Circle::draw() const {
	cout << "원 그리기" << endl;
	cout << "(" << cx << ", " << cy << ")에서부터 ";
	cout << radius << "만큼 떨어진 모든 점들을 ";
	cout << attrib.getLineColor() << "으로 그리고" << endl;
	cout << "내부를 " << attrib.getFillColor();
	cout << "으로 채운다" << endl;
}
```
## Triangle 클래스
- Triangle 클래스는 Figure로부터 상속받는 속성 외에 삼각형의 세 꼭짓점 좌표를 나타내는 속성이 필요하다.
- Figure 클래스에서 순수가상함수로 선언하여 놓은 move(), scale(), draw()를 각각 재정의하여 구현한다.

### Triangle클래스의 메서드
|메서드|비고|
|-----|-----|
|Triangle(double v[3][2])|생성자|
|void move(double dx, double dy) | 삼각형을을(dx,dy)만큼 이동|
|void scale(double s) | 삼각형을 원점 기준으로 s배 크기 조정|
|void draw()|삼각형을 그림|

### Triangle클래스의 속성
|속성|비고|
|-----|-----|
|double x1,y1|삼각형의 꼭짓점 좌표|
|double x2,y2|
|double x3,y3|
### Triangle.h
```c

```
### Triangle.cpp
```c

```

