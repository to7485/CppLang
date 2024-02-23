# Random클래스
- 컴퓨터는 난수를 생성할 때 첫번째 수만 무작위로 정한다.
- 나머지 수들은 그 수를 기반으로 여러가지 수학적 방법을 통해 마치 난수처럼 보이지만 실제로는 무작위로 생성된 것이 아닌 수열들을 만들어내게 된다.
- 무작위로 정해진 첫번째 수를 시드(seed)라고 부르는데, C의 경우 srand를 통히 seed를 설정할 수 있다.
- 난수를 발생시키는 클래스

### 랜덤클래스.cpp
```c
#include <iostream>
#include <random>
using namespace std;

int main(){
	//시드값을 얻기 위한 random_device 객체 생성
	random_device a;

	//random_device를 통해 난수 생성 엔진을 초기화 한다.
	mt19937 gen(a());

	//1부터3까지 균등하게 나타나는 난수열을 생성하기 위해 균등분포 정의
	uniform_int_distribution<int>(1,3);

	for(int i = 0 ; i < 5; i;;){
		cout <<"난수 : " << dis(gen) << endl;
	}


	return 0;
}

```

### 랜덤클래스연습1.cpp
```c
//랜덤클래스를 사용하여
//1 ~ 45 사이의 정수 6개를 랜덤으로 출력

#include <iostream>
#include <random>
using namespace std;

int main(){
	random_device a;
	uniform_int_distribution<int>dis(1,45);
	uniform_int_distribution<double>dis(1.0,2.0);

	for(int i =0; i< 6; i++){
		cout <<dis(a) <<" "<<endl;
	}
}
```

### 랜덤클래스연습2.cpp
```c
//"red", "blue", "green"중 랜덤으로 10회 출력하시오.

#include <iostream>
#include <string>
#include <random>

using namespace std;

random_device a;
uniform_int_distribution<int>dis(0,2);

int main(){

    string color[3] = {"red","blue","green"};

    for(int i = 0 ; i< 10; i++){
        cout << color[dis(a)] <<endl;
    }
    
    return 0;
}

```

### 랜덤클래스연습3.cpp
```c
//최소 10명의 이름을 입력받고
//청소 당번을 랜덤으로 2명 뽑으세요

//이름 >> 홍길동
//이름 >> 박길동
// ....
//홍길동 박길동 ... 십길동

//청소당번1 : 김길동
//청소당번2 : 홍길동

#include <iostream>
#include <string>
#include <random>

using namespace std;

random_device a;
uniform_int_distribution<int>dis(0,9);

int main(){

    string* name = new string[10];
    int count = 0;
    for(int i = 0 ; i <10; i++){
        cout<<"이름 >> ";
        cin >> name[i];
        if(name[i] == "end")break;
        count++;
    }

    random_device rd;
    uniform_int_distribution<int>(0,count-1);

    for(int i = 0; i < count; i++){
        cout <<name[i] <<" ";
    }
    cout <<endl;
    cout <<"청소당번1 : "<< name[dis(rd)] <<endl;
    cout <<"청소당번2 : "<< name[dis(rd)] <<endl;

    
    return 0;
}


```

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
//C++ 표준 라이브러리에는 string이라는 문자열을 저장할 수 있는 클래스를 제공한다.
//사실 이 클래스는 basic_string<char>라는 표준 C++ 라이브러리의 클래스 템플릿으로 선언된 것이다.
//string 클래스는 여러 가지 연산자를 제공하고 있다.
//앞으로는 C 스타일의 문자열 대신 string을 이용하게 될 것이다.
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

## 파생 클래스의 생성자 및 소멸자
- 파생 클래스도 생성자가 필요한 경우가 있다.
- 기초 클래스가 생성자를 가지고 있다면, 파생 클래스의 생성자는 기초 클래스의 생성자를 호출하고 나머지 필요한 부분만 정의한다.

### 파생 클래스의 생성자의 표현
```c
DClassName(fParameterList) : BClassName(bArgsList)

{

	파생 클래스 생성자에서 추가되는 사항

}
※ DClassName : 생성자 - 파생 클래스 이름을 사용
※ BClassName : 기초 클래스 생성자 - 기초 클래스 이름을 사용
※ fParameterList : 파생 클래스 생성자 형식 매개변수 목록
※ bArgsList : 기초 클래스 생성자에 전달할 인수 목록
```
- 기초 클래스의 생성자가 인수를 필요로 하면, 그 인수들을 넘겨주어야 한다.
- 파생 클래스는 기초 클래스를 바탕으로 만들어지는 것이므로, 파생 클래스의 생성자가 수행될 때 기초 클래스의 생성자가 먼저 호출되어 실행되고, 그 다음에 파생 클래스에서 구현한 명령문들이 실행된다.
- 파생 클래스의 소멸자를 정의할 때에는 파생 클래스의 정리작업만 하면 된다.
- 이때 기초 클래스의 소멸자는 자동으로 호출된다.
- 파생 클래스의 소멸자에 나열된 명령문들이 먼저 실행된 다음에 기초 클래스의 소멸자가 실행된다.

## Person2.h
```c
#ifndef PERSON1_H_INCLUDED
#define PERSON1_H_INCLUDED
#include <iostream>
#include <string>
using namespace std;

class Person {
	string name;

public:
	Person(const string &n) {
		cout << "Person의 생성자" << endl;
		name = n;
	}
	~Person() {
		cout << "Person의 소멸자" << endl;
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

## Student2.h
```c
#ifndef STUDENT1_H_INCLUDED
#define STUDENT1_H_INCLUDED
#include <iostream>
#include <string>
#include "Person1.h"

class Student : public Person {
	string school;
public:
	Student(const string &n, const string &s) :Person(n) {
		cout << "Student의 생성자" << endl;
		school = s;
	}

	~Student() {
		cout << "Student의 소멸자" << endl;
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

## StudentMain2.cpp
```c
#include <iostream>
#include "Person2.h"
#include "Student2.h"
using namespace std;

void main() {
	Student harry("Harry", "Hogwarts");
	cout << harry.getName() << "goes to " << harry.getSchool() << endl;
}
```

## 액세스 제어
- 클래스는 다른 클래스들이나 외부 함수가 자신의 멤버함수들을 액세스하는 것을 허가하거나 금지함으로써 클래스 자신을 보호하는 기능이 있다.
- 일반적으로 파생 클래스를 갖지 않는 경우에는 클래스는 private 멤버와 public 멤버를 갖도록 설계한다.
- 예를 들어 현금인출기를 생각해 보자.
- 현금인출기의 기능 중에는 인출기를 사용하는 사람에게 공개하여야 할 기능들이 있고, 사용자에게 공개할 필요가 없는 기능 또는 공개하지 말아야 할 기능이 있을 수 있다.
```c
class 현금인출기 {

private:     // 사용자에게 공개할 필요가 없거나 하지 말아야 할 기능

	출금할 돈을 채운다.

	명세서를 출력한다.

	계좌의 잔고를 계산한다.

	전산망에 연결하여 계좌의 정보를 가져온다.

public:      // 사용자에게 공개하여야 할 기능

	돈을 인출한다.

	계좌번호와 비밀번호를 입력한다.

};
```
### 접근제어자와 공개영역
|접근제어자|사용범위|
|-----|-----|
|private|- 소속 클래스의 멤버함수<br>- 친구 클래스의 멤버함수 및 친구함수|
|protected |- 소속 클래스의 멤버함수<br>- 친구클래스의 멤버함수 및 친구함수<br>- 파생 클래스의 멤버함수<br>- 파생클래스의 친구 클래스의 멤버함수 및 친구함수|
|public|- 전범위|

### 접근제어의 상속
|접근제어자 상속|B의 public 멤버는| B의 protected멤버는|
|-----|-----|-------------|
|class D1 : private B{....};|D1의 private 멤버| D1의 private 멤버|
|class D2 : protected B{....}; |D2의 protected 멤버| D2의 protected 멤버|
|class D3 : public B{....};|D3의 public 멤버| D3의 protected 멤버|

## Access.cpp
```c
#include <iostream>
using namespace std;

class BaseC {
	int a; //private 멤버
protected:
	int b; //protected 멤버
public:
	int c; //public 멤버
	int geta() const { return a; } //OK : 소속 멤버 사용
	void set(int x, int y, int z) { a = x, b = y, c = z; } //OK
};

class Drvd1 : public BaseC {
	//class BaseC의 멤버변수 a는 class Drvd1이 액세스 할 수 없음
	//class BaseC의 멤버변수 b는 class Drvd1의 protected로 취급되고
	//class BaseC의 멤버변수 c는 class Drvd1의 public으로 취급됨
	//b와 c는 이 클래스가 액세스 할 수 있음
public:
	//int sum() const { return a + b + c; }//error : a를 사용
	void printbc() const { cout << b << ' ' << c << '\n'; }//OK
};

class Drvd2 : protected BaseC {
	//class BaseC의 멤버변수 a는 class Drvd2가 액세스 할 수 없음
	//class BaseC의 멤버변수 b와 c는 class Drvd2의 protected로 취급됨
	//b와 c는 class Drvd2가 액세스 할 수 있음
public:
	//int sum() const {return a + b + c;}
	void printbc() const { cout << b << ' ' << c << '\n'; }//OK

};
class Drvd3 : protected BaseC {
	//class BaseC의 멤버변수 a는 class Drvd3가 액세스 할 수 없음
	//class BaseC의 멤버변수 b와 c는 class Drvd3의 private로 취급됨
	//b와 c는 class Drvd3가 액세스 할 수 있음
public:
	//int sum() const {return a + b + c;}
	void printbc() const { cout << b << ' ' << c << '\n'; }//OK

};

void main() {
	Drvd1 d1;
	d1.a = 1; //private 멤버를 액세스 하므로 error
	d1.b = 2; //protected 멤버를 액세스 하므로 error
	d1.c = 3; //public멤버를 액세스 하므로 OK

	Drvd2 d2;
	d2.a = 1; //private 멤버를 액세스 하므로 error
	d2.b = 2; //protected로 취급되는 멤버를 액세스 하므로 error
	d2.c = 3; //protected로 취급되는 멤버를 액세스 하므로 error

	Drvd3 d3;
	d3.a = 1; //private 멤버를 액세스 하므로 error
	d3.b = 2; //protected로 취급되는 멤버를 액세스 하므로 error
	d3.c = 3; //protected로 취급되는 멤버를 액세스 하므로 error

}
```


