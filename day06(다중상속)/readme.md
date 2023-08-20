# 다중 상속
- 파생 클래스는 1개 이상의 기초 클래스를 가질 수 있다.
- 2개 이상의 기초 클래스를 상속받는 것을 다중상속(multiple inheritance)이라고 한다.
- 비록 다중상속을 사용하는 경우가 드물기는 하지만 두 가지 이상의 다른 클래스 유형의 특성을 결합하는 클래스를 정의할 때 유용하게 사용될 수 있다.
-  '다중 상속이라는 것이 있다'는 것과 '어떻게 사용을 할 수 있었다' 정도만 기억하고, 다중 상속을 사용해야겠다는 절실한 필요성을 느낄 때 다시 찾아볼 수 있을 정도를 목표로 다중 상속에 대해 배웁니다.
- c++에서는 쉼표(,)를 사용하여 상속받을 여러 개의 기초 클래스를 명시하는 것으로 간단히 다중 상속을 구현할 수 있다.
```c
class 파생클래스이름 : 접근제어지시자 기초클래스이름, 접근제어지시자 기초클래스이름, 접근제어지시자 기초클래스이름, ...

{

    // 파생 클래스 멤버 리스트

}
```

## 다중 상속의 문제점
- 다중 상속은 여러 개의 기초 클래스가 가진 멤버를 모두 상속받을 수 있다는 점에서 매우 강력한 상속 방법입니다.
- 하지만 이러한 다중 상속은 단일 상속에는 없는 여러 가지 새로운 문제를 발생시킵니다.

1. 상속받은 여러 기초 클래스에 같은 이름의 멤버가 존재할 가능성이 있습니다.
2. 하나의 클래스를 간접적으로 두 번 이상 상속받을 가능성이 있습니다.
3. 가상 클래스가 아닌 기초 클래스를 다중 상속하면, 기초 클래스 타입의 포인터로 파생 클래스를 가리킬 수 없습니다.

## 다중상속 예제
- n다스 m자루 형태로 연필의 개수를 표현하는 클래스를 정의한다.
- 낱개의 수를 1개 증가시키는 전위 및 후위 표기 ++ 연산자를 포함한다.
- 연필의 수량을 출력하는 메소드를 포함한다.
- 학생을 나타내는 클래스 Student
- 직장인을 나타내는 클래스 Employee
- 학생이면서 직장인인 시간제 학생 클래스 Parttime을 선언하라.

![image](https://github.com/to7485/CppLang/assets/54658614/e605f764-43b3-4003-a111-e006b7270646)

-  Student와 클래스 Employee로부터 다중상속을 받는 클래스 Parttime의 관계이다.

## MIStudent.h
```c
#ifndef MISTUDENT_H_INCLUDED
#define MISTUDENT_H_INCLUDED
#include <iostream>
#include <string>
using namespace std;

class Student {
	string school;

public:
	Student(const string &s) : school(s) {}
	void printSchool() const { cout << school << endl; }
};
#endif // !MISTUDENT_H_INCLUDED
```

## MIEmployee.h
```c
#ifndef MIEMPLOYEE_H_INCLUDED
#define MIEMPLOYEE_H_INCLUDED
#include <iostream>
#include <string>
using namespace std;

class Employee {
	string company;
public:
	Employee(const string &c) :company(c) {}

	void printCompany() const { cout << company << endl; }
};

#endif // !MIEMPLOYEE_H_INCLUDED
```

## MIParttime.h
```c
#ifndef MIPARTTIME_H_INCLUDED
#define MIPARTTIME_H_INCLUDED
#include <string>
#include "MlStudent.h"
#include "MIEmployee.h"

using namespace std;

class Parttime : public Student, public Employee {
public:
	Parttime(const string& s, const string& c) 
		:Student(s), Employee(c) {};

};

#endif // !MIPARTTIME_H_INCLUDED
```

## MIMain.cpp
```c
#include "MIParttime.h"

void main() {
//Parttime 객체 chulsoo 선언
	Parttime chulsoo("ABC Univ.", "DEF.Co");
//Parttime이 Student와 Employee로부터 다중상속을 받으므로 두 클래스의 멤버함수를 모두 사용할 수 있다.
	chulsoo.printSchool(); //Student의 멤버함수 호출
	chulsoo.printCompany(); //Employee의 멤버함수 호출
}
```
- 현재 2개의 기초 클래스에 있는 함수의 이름이 다르다.
- 하지만 2개의 함수의 이름이 동일하다면 어떤일이 일어날까??

```c
class Student {

  // ...
  
  void print() const { cout << school << endl; }

};

 

class Employee {
  // ...
  
  void print() const { cout << company << endl; }

};
```

-  main()에서 Student::print()를 호출하는지 Employee::print()를 호출하는지 모호하므로 이러한 호출은 오류가 난다.

```c
Parttime chulsoo("ABC Univ.", "DEF Co.");
chulsoo.print();   // error: ambiguous
```
- 이런 경우 print() 함수를 사용하려면 어느 기초 클래스의 함수를 사용하는지 명확하게 지정해줘야 한다.

```c
chulsoo.Student::print();   // OK
chulsoo.Employee::print();  // OK
```

## 공통 기초 클래스의 중복 상속
- 1개 이상의 기초 클래스를 상속받을 수 있기 때문에 1개의 기초 클래스가 두 번 상속되는 경우가 있다.

## Parttime 클래스
- Student와 Employee가 모두 Person으로부터 파생된 클래스로 선언하라.

![image](https://github.com/to7485/CppLang/assets/54658614/42d7a986-4295-4da8-9910-f7cfa23740c0)

- Parttime 클래스에서는 Person 클래스의 멤버가 이중으로 상속되는 결과가 나타난다.

## CBPerson.h
```c
#ifndef CBPERSON_H_INCLUDED
#define CBPERSON_H_INCLUDED
#include <iostream>
#include <string>

using namespace std;

class Person {
	string name;
public:
	Person(const string &n) :name(n) {}
	void print()const { cout << name; };
};

#endif // !CBPERSON_H_INCLUDED
```

## CBStudent.h
```c
#ifndef CBSTUDENT_H_INCLUDED
#define CBSTUDENT_H_INCLUDED
#include <iostream>
#include <string>
#include "CBPerson.h"
using namespace std;

//파생 클래스 Student를 Person을 상속받아 정의한다.
class Student : public Person {
	string school;
public:
	Student(const string& n, const string& s) : Person(n), school(s) {}
	void print() const {
		Person::print();
		cout << " goes to " << school << endl;
	}
};
#endif // !CBSTUDENT_H_INCLUDED
```

## CBEmployee.h
```c
#ifndef CBEMPLOYEE_H_INCLUDED
#define CBEMPLOYEE_H_INCLUDED
#include <iostream>
#include <string>
#include "CBPerson.h"
using namespace std;

class Employee : public Person {
	string company;
public:
	Employee(const string & n, const string &c) :Person(n), company(c) {}
	void print() const {
		Person::print();
		cout << " is employed by " << company << endl;
	}
};
#endif //CBEMPLOYEE_H_INCLUDED
```

## CBParttime.h
```c
#ifndef CBPARTTIME_H_INCLUDED
#define CBPARTTIME_H_INCLUDED
#include "CBStudent.h"
#include "CBEmployee.h"

class Parttime : public Student, public Employee {
public:
	Parttime(const string &n, const string & s, const string &c)
        : Student(n, s), Employee(n, c) {}
};
#endif // !CBPARTTIME_H_INCLUDED
```

## CBMain.cpp
```c
#include <iostream>
#include "CBParttime.h"
using namespace std;

void main() {
	Parttime chulsoo("Chulsoo", "ABC Univ.", "DEF Co.");
	chulsoo.Student::print();
	chulsoo.Employee::print();
}
```
- Parttime 클래스는 Student와 Employee를 모두 상속받았다.
- 이 경우 Person 클래스의 객체가 2개 만들어진다.

![image](https://github.com/to7485/CppLang/assets/54658614/6cec7a0c-0684-4682-9d2b-6849a301df55)

- Person::print()에서 실제로 할당되는 Person의 데이터 멤버 주소를 확인하기 위하여 다음과 같이 수정하여 프로그램을 실행하여 보자.

```c
virtual void print() const { cout << &name; };
```
- 출력되는 결과는 다음과 같다.
```c
005AFAD0 goes to ABC Univ.

005AFB0C is employed by DEF Co.
```
- 이 결과에서 어느 경로를 통해 print()를 호출하였는가에 따라 name의 주소가 다른 것을 볼 수 있다.
- 이를 통해 클래스 Person의 객체가 Student와 Employee에 대해 중복하여 각각 할당된 것을 확인할 수 있다.

# 가상 상속(Virtual Inheritance)
-  다이아몬드 처럼 다중 상속 기반 클래스가 같은 기반클래스를 상속 받아서 구현된 경우 동일 멤버를 이중 속상 받지 않도록 가상 상속을 사용해야 함
## 가상 기초 클래스
- 기초 클래스가 한 번만 나타나게 하는 방법은 가상 기초 클래스를 사용하는 것이다.
- 가상 기초 클래스를 사용하는 방법은 파생 클래스를 상속할 때 키워드 virtual을 사용한다.

![image](https://github.com/to7485/CppLang/assets/54658614/17a28e40-ef15-4629-a4bf-a29976181c5e)

```c
파생클래스명 : virtual public 기초클래스명 {

    ……

};
```
## 가상 기초 클래스의 활용
- Student와 Employee를 위한 기초 클래스 Person의 객체가 하나만 생성되도록 클래스를 선언하라.

## VBPerson.h
```c
#ifndef VBPERSON_H_INCLUDED
#define VBPERSON_H_INCLUDED
#include <iostream>
#include <string>
using namespace std;

class Person {
	string name;
public:
	Person(const string &n) :name(n) {}
	void print() const { cout << name; }
};
#endif
```
## VBStudent.h
```c
#ifndef VBSTUDENT_H_INCLUDED
#define VBSTUDENT_H_INCLUDED
#include <iostream>
#include <string>
#include "VBPerson.h"
using namespace std;

//Person을 가상 기초 클래스로 상속
class Studnet : virtual public Person {
	string school;
public:
	Studnet(const string &n, const string &s) 
		: Person(n), school(s) {}
	void print() const {
		Person::print();
		cout << " goes to " << school << endl;
	}
};

#endif // !VBSTUDENT_H_INCLUDED
```

## VBEmployee.h
```c
#ifndef VBEMPLOYEE_H_INCLUDED
#define VBEMPLOYEE_H_INCLUDED
#include <iostream>
#include <string>
#include "VBPerson.h"
using namespace std;

//Person을 가상 기초 클래스로 상속
class Employee : virtual public Person {
	string company;
public:
	Employee(const string &n, const string &c) :Person(n), company(c) {}
	void print() const {
		Person::print();
		cout << " is employed by " << company << endl;
	}
};
#endif // !VBEMPLOYEE_H_INCLUDED
```

## VBParttime.h
```c
#ifndef VBPARTTIME_H_INCLUDED
#define VBPARTTIME_H_INCLUDED
#include "VBStudent.h"
#include "VBEmployee.h"

//Student와 Employee를 상속
class Parttime : public Student, public Employee {
public:  
	//Person의 생성자를 명시적으로 호출함
	Parttime(const string& n, const string &s, const string &c)
		:Person(n), Student(n, s), Employee(n, c) {}
	void print() const {
		Student::print();
		Employee::print();
	}
};
#endif // !VBPARTTIME_H_INCLUDED
```
## VBMain.cpp
```c
#include "VBParttime.h"
#include <iostream>
using namespace std;

void main() {
	Parttime chulsoo("Chulsoo", "ABC Univ.", "DEF Co.");
	chulsoo.print();
}
```
- 기초 클래스 Person을 가상 기초 클래스로 상속함으로써 Person의 멤버인 name은 하나만 생성된다. 
- 그러나 이 경우에는 클래스 Parttime의 생성자에서 Person의 생성자를 명시적으로 호출하여 주어야 한다.
- 이렇게 함으로써 Student의 생성자와 Employee의 생성자가 호출될 때 별도로 Person의 생성자를 호출하지 않는다.
- Person::print()에서 실제로 할당되는 데이터 멤버 name의 주소를 확인하기 위하여 다음과 같이 수정하여 프로그램을 실행하여 보자.

```c
virtual void print() const { cout << &name; };
```

- 출력 결과는 다음과 같이 Person의 데이터 멤버인 name이 실제로 하나만 할당된 것을 확인할 수 있다.

```c
008FFD34 goes to ABC Univ.
008FFD34 is employed by DEF Co.
```
