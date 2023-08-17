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
	pPrsn1 = &dudley; //기초 클래스의 포인터기 기초 클래스의 객체를 가리키므로 문제 없음
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

