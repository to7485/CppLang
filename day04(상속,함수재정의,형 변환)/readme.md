# 상속
- 객체지향 언어에서 상속(inheritance)은 계층관계를 사용하여 클래스 간의 속성 및 함수를 공유할 수 있도록 지원하는 매우 중요한 개념이다.
- 한 클래스가 다른 클래스의 한 가지 구체적인 예에 해당할 때 이 클래스 간에 'is-a' 관계가 있다고 말한다.

![image](https://github.com/to7485/CppLang/assets/54658614/71c2ee7c-aa10-461f-8661-291c75805554)

- 사람 클래스는 학생 클래스와 직장인 클래스의 공통된 특징을 모두 보유하고 있는 클래스이다.
- 이러한 클래스를 기초 클래스(base class, superclass, parent class)라고 한다.
- 학생이나 직장인 클래스와 같이 보다 더 자세한 속성을 갖는 클래스를 파생 클래스(derived class, subclass, child class)라고 한다.
- 파생 클래스는 기초 클래스의 특성을 모두 포함하고 있으며  (또는 상속받으며), 자신의 고유 특성을 추가로 갖고 있을 수 있다.
- 파생 클래스를 표현할 때 기초 클래스의 특성들을 반복하여 표현할 필요가 없다.

## 파생클래스의 선언
```c
class 파생클래스명 : 접근제어자 기초클래스명 {

  접근제어자:
  
    데이터 멤버 또는 멤버함수 리스트;
  
  접근제어자:
  
    데이터 멤버 또는 멤버함수 리스트;

};
```

## 파생클래스의 활용
- 사람을 나타내는 클래스 Person과 이로부터 파생되어 학생을 표현하는 클래스 Student를 선언하라.
- Person은 이름을 표현할 수 있으며, 이름의 지정 기능과 이름을 알려 주는 기능이 포함된다.
- Student는 Person의 기능을 상속받으면서 학교 이름을 지정하거나 저장된 학교 이름을 알리는 기능이 추가된다.

![image](https://github.com/to7485/CppLang/assets/54658614/93605792-8164-431d-a7c4-026d515d67c8)


### Person클래스의 메서드
|메서드|비고|
|-----|-----|
|void setName(string n)|n의 문자열로 이름을 초기화|
|string getName() | 이름을 문자열로 반환|
|void print()|이름을 콘솔에 출력|

### Student클래스의 메서드
|메서드|비고|
|-----|-----|
|void setSchool(string s)|s의 문자열로 학교이름을 초기화|
|string getSchool() | 학교이름을 문자열로 반환|
|void print()|학교와 이름을 콘솔에 출력|

## Person1.h
```c
#ifndef PERSON1_H_INCLUDED
#define PERSON1_H_INCLUDED
#include <iostream>
#include <string>
using namespace std;

class Person {
	string name;

public:
	void setName(const string &n) {
		name = n;
	}
	string getName() const {
		return name;
	}

	void print() const {
		cout << name;
	}
};
#endif
```
## Student1.h
```c
#ifndef STUDENT1_H_INCLUDED
#define STUDENT1_H_INCLUDED
#include <iostream>
#include <string>
#include "Person1.h"

class Student : public Person {
	string school;
public:
	void setSchool(const string &s) {
		school = s;
	}
	string getSchool() const {
		return school;
	}
	void print() const {
		Person::print();
		cout << "goes to " << school;
	}
};
#endif
```

## StudentMain1.cpp
```c
#include <iostream>
#include "Person1.h"
#include "Student1.h"
using namespace std;

void main() {
	Person dudley;
	dudley.setName("Dudley");
	Student harry;
	harry.setName("Harry");
	harry.setSchool("Hogwarts");
	dudley.print();
	cout << endl;
	harry.print();
	cout << endl;
	harry.Person::print();
	cout << endl;
}
```

## 클래스의 계층
- 파생 클래스는 그 자신도 기초클래스가 될 수 있다.
- 예를 들면 도형, 다각형, 삼각형, 사각형 클래스를 다음과 같이 정의할 수 있다.
```c
class Figure { int color; ... };

class Polygon : public Figure { int nOfVertices; ... };

class Triangle : public Polygon

{ int a; int b; int c; ...  };

class Rectangle : public Polygon

{ int width; int height; ... };
```
- 클래스 간의 관계를 클래스 계층이라고 한다.























