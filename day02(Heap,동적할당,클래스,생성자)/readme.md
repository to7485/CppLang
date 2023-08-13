# 구조체
- 지금까지 사용한 int, float 등과 같은 자료형은 C++에 미리 정의되어 있는 기본 자료형이다.
- 이와는 별개로 프로그래머가 필요에 따라 새로운 자료형을 정의할 수 있다.
- 구조체를 사용자 정의 자료형이라고 할 수 있는데, 기본 자료형이나 앞서 선언된 사용자 정의 자료형을 이용하여 정의한다.

### 구조체 생성 방법
```c
struct 구조체명{
   Type1 변수명1;
   Type2 변수명2;
   ...
}
```
- Type1, Type2는 기본 자료형의 하나일 수도 있고, 먼저 정의된 사용자 정의 자료형일 수도 있다.

### 구조체 변수의 선언
```c
구조체명 변수명;
```

## 구조체 실습
- 원의 면적을 구하기
- 두 개의 원이 중첩되는지 검사하고 원의 정보 출력하기

### Circles1.cpp
```c
#include <iostream>

using namespace std;

const double PI = 3.141593;

struct C2dType { //2차원 좌표 구조체
	double x, y; 
};

struct CircleType {
	C2dType center; //중심좌표
	double radius; //반지름
};

//원의 면적을 구하는 함수
double circleArea(CircleType c) {
	return c.radius * c.radius * PI;
}

//두 원이 겹치는지 구하는 함수
bool chkOverlap(CircleType c1, CircleType c2) { //두 개의 원을 매개변수로 받는다.
	double dx = c1.center.x - c2.center.x;
	double dy = c1.center.y - c2.center.y;
	double dCntr = sqrt(dx*dx + dy * dy);
	return dCntr < c1.radius + c2.radius;
}

//원의 정보를 출력하는 함수
void dispCircle(CircleType c) {
	cout << " 중심 : (" << c.center.x << ", " << c.center.y << ")";
	cout << " 반경 : " << c.radius << endl;
}

void main() {
	CircleType c1 = { {0,0},10 }; //중심(0,0) 반지름 10으로 초기화
	CircleType c2 = { {30,10},5 }; //중심(0,0) 반지름 10으로 초기화

	cout << "원1" << endl;
	dispCircle(c1);
	cout << " 원1의 면적 : " << circleArea(c1) << endl;
	cout << "원2" << endl;
	dispCircle(c2);
	cout << " 원2의 면적 : " << circleArea(c2) << endl << endl;

	//두 원의 중첩 여부 출력
	if (chkOverlap(c1, c2)) {
		cout << "두 원은 중첩됩니다." << endl;
	}
	else {
		cout << "두 원은 중첩되지 않습니다." << endl;
	}

}
```

# 메모리 영역
- C++의 프로그램에서 사용하는 메모리는 일반적으로 4가지 영역으로 나눈다.

### Code영역
- 컴파일된 프로그램이 저장되는 영역

### Data영역
- 전역 변수, 정적(static) 변수가 저장되는 영역

### Heap영역
- 동적 할당된 변수가 할당되는 영역

### Statck영역
- 지역 변수, 매개변수 및 기타 함수 관련 정보가 저장되는 영역

# Heap영역
- Heap영역은 동적 메모리 할당에 사용되는 메모리를 추적한다.

## Heap 영역의 특징
- 힙에 메모리를 할당하는 것은 비교적 느리다.
- 할당된 메모리는 명시적으로 할당 해제하거나 응용 프로그램이 종료될 때까지 유지된다. (메모리 누수 주의)
- 동적으로 할당된 메모리는 포인터를 통해 접근한다: 포인터를 역참조하는 것은 변수에 직접 접근하는 것보다 느리다.
- 힙은 큰 메모리 풀이므로 큰 배열, 구조체 또는 클래스를 할당할 수 있다.

## 메모리의 동적할당
- 필요할 때 기억공간을 할당하고 더 이상 그 공간이 필요하지 않으면 반환할 수 있게 하는 것.
- 그런데 동적 메모리 할당으로 생성된 저장공간은 이름이 없어 변수처럼 그 이름을 통해 액세스할 수 없다.
- 동적으로 할당된 저장공간을 포인터 변수가 가리키게 하면 그 포인터를 이용하여 액세스할 수 있다.

## new와 delete

### new
- 동적 메모리 할당은 다음과 같이 new 연산자를 이용한다.
- new 연산자와 malloc()함수의 차이점은 '메모리의 크기를 정하지 않는다'라는것이다.
```c
1. 포인터변수 = new 자료형;
2. 포인터변수 = new 자료형[n];
```
- 1번은 지정된 자료형의 데이터 1개를 저장할 수 있는 공간을 할당하며, 그 주소를 포인터 변수에 넣는다.<br>이 때 자료형은 포인터의 자료형과 일치해야 한다.
- 번은 지정된 자료형의 데이터를 n개 저장할 수 있는 배열을 할당한다. n은 양의 정수값을 내는 수식이면 되며, 상수가 아니어도 된다.


### delete
- delete 연산자는 new 연산자의 형식1로 할당한 메모리 공간을 반환할 때 사용한다.
- delete [] 연산자는 형식2의 방법으로 new 연산자를 사용하여 할당한 메모리 공간을 반환할 때 사용한다.

```c
1. delete 포인터변수;
2. delete [] 포인터변수;
```

```c
#include <iostream>

using namespace std;

void main() {
	int* a = new int;
	int* b = new int(2); //동적할당을 받으면서 값을 초기화 할 수 있다.

	*a = 1;

	cout << *a << endl;
	cout << *b << endl;

	delete a;
	delete b;
}
```

# 클래스
- 구조체(struct)는 프로그램으로 표현하고자 하는 대상에 대한 데이터의 구조만을 정의하고 있다.
- 클래스(class)는 마치 객체의 설계도와 같다.
- 클래스에는 객체가 포함할 속성에 대한 명세와 메소드들이 정의되어 있어
- 클래스를 이용하여 동일한 속성 및 메소드를 갖는 객체들을 생성한다.

# 객체
- 객체는 실세계의 문제영역에 존재하는 대상물을 그 대상물의 속성(attribute)과 메소드(method)로 모델링한 것이다.
- 객체의 속성은 그 객체의 상태를 나타내는 데이터이며, 객체의 메소드는 내부의 데이터를 사용하여 정해진 동작을 하는 함수이다.
- 사실 세상에 존재하는 모든 것들이 전부 대상이며, 어느 것은 객체로 표현되고 어느 것은 안 된다는 식으로 한계를 지을 수 없다.

# 메세지
- 하나의 프로그램에 객체가 하나만 존재하는 경우는 거의 없다. 일반적으로 여러 개의 객체가 포함되어 있으며, 프로그램이 필요한 기능을 해내기 위해서는 이들이 상호작용해야만 한다.
- 한 객체가 다른 객체의 기능을 필요로 한다면 그 객체는 상대방 객체에게 필요한 작업이 이루어지도록 요구한다.
- 이와 같이 상대방 객체에게 필요한 작업을 실행하도록 요구하는 것을 객체지향 프로그래밍에서는 메시지를 보낸다고 한다.
- 객체지향 언어에서 메시지를 보내는 것은 그 메시지를 처리할 메소드, 즉 함수를 호출하는 것을 의미한다.

![image](https://github.com/to7485/CppLang/assets/54658614/b282820d-3590-4530-a6bf-2aacb26cd3fb)

# 캡슐화
- 캡슐화(encapsulation)는 객체의 사용자 측면의 사항과 설계자 측면의 사항을 분리하는 것이다.
- 객체의 사용자에게는 객체에게 어떤 메시지를 어떻게 보냄으로써 그 객체가 동작하게 할 수 있는가에 대한 정보가 필요하다.
- 세부적인 객체 내부 상태의 표현이나 행위의 구현에 대해서는 알 필요가 없다.
(우리가 약을 먹는데 약이 어떤 재료로 이루어져있는지 까지는 알 필요가 없다.)
- 반면 설계자는 객체가 동작하게 하기 위한 세부적인 구현 부분을 정의해야 할 것이다.
- 객체지향 프로그래밍 언어에서는 캡슐화를 위해 공개할 멤버와 공개하지 않을 멤버를 구분할 수 있다.

![image](https://github.com/to7485/CppLang/assets/54658614/aca7d707-1d1d-4767-ad03-33bdc6ab285c)

## 클래스 선언과 객체의 정의
- 클래스는 C++ 언어에서 객체지향 개념을 구현하기 위한 도구로서
- 프로그램에서 사용하고자 하는 객체들에 대한 형판을 정의한 것으로 볼 수 있다.
- 클래스와 객체의 관계는 자료형과 변수의 관계와 유사하다.
- 그러나 클래스는 각각의 객체를 표현하는 속성과 함께 그 객체들에 대한 메시지를 처리하는 메소드를 정의해 놓은 것이다.
- C++ 언어에서는 클래스 안에 정의한 속성들을 데이터 멤버(data member)라고 부르고, 메소드를 멤버함수(member function)라고 부른다.

```c
class 클래스명{
  접근제어자:
      멤버변수 또는 멤버함수;
  접근제어자:
      멤버변수 또는 멤버함수;
  ...

}
```

## 접근제어자
- 그 다음에 나열되는 데이터 멤버나 멤버함수들이 외부에 공개되는 범위를 나타낸다.
- 다음 접근제어자가 나올 때까지 유효하다.
- 접근제어자 뒤에는 클래스의 데이터 멤버 및 멤버함수들이 나열된다.

### 접근제어자의 종류
|접근제어자| 공개되는 범위|
|----|-------|
|private|소속 클래스의 멤버함수<br>friend 클래스의 멤버함수 및 friend 함수|
|public| 전범위|

## 멤버의 선언
- 데이터 멤버는 일반적으로 private 멤버로 선언한다.
- 데이터 멤버를 공개하는 것이 불가능한 것은 아니다.
- 객체의 모델링은 객체의 행위를 중심으로 이루어지며,
- 데이터 멤버는 그러한 행위와 관련된 객체 내부의 상태로 보는 것이 일반적이어서 주로 private로 선언하는 것이다.

## 멤버 함수의 선언
- 멤버함수는 클래스 선언문 내에 선언하거나, 클래스 선언문에는 멤버함수의 원형만 선언해 놓고 외부에 별도로 멤버함수를 정의한다.

## 객체의 정의 및 사용
- 클래스 선언문은 클래스에 해당되는 객체가 갖게 되는 멤버들에 대하여 선언한 것이며, 실제 객체는 일반 자료형의 변수를 정의하듯 별도로 정의하여야 한다.
- 객체를 정의한다는 것은 실제 객체를 만드는 것을 의미한다.

### 객체 선언 방법
```c
클래스명 객체명;

클래스명 객체명1, 객체명2...;
```

## Counter.h
- 멤버로 숫자를 가지고 있다.
- 멤버 함수로 값을 1씩 증가시키는 count(), 현재 값을 얻어오는 getValue(), 값을 0으로 만드는 reset()이 있다.

```c
#ifndef COUNTER_H_INCLUDED

#define COUNTER_H_INCLUDED

class Counter {
	int value; //접근제어자가 지정되지 않았지만, 디폴트인 private이 지정된다.

public:
	void reset() {
		value = 0;
	}

	void count() {
		value += 1;
	}

	int getValue() const {
		return value;
	}
};

	// const 멤버 함수 : 멤버 함수 내에서 데이터 멤베덜의 값을
	//변경하지 않겠다.

	//만일 const 멤버함수 내에서 데이터 멤버의 값을 수정하면 
	//컴파일러가 오류 메세지를 보낸다.


#endif 
```

- 일반적으로 클래스를 선언할 때는 2개의 파일을 만든다.
- 클래스 선언문이 포함된 헤더 파일
- 클래스의 멤버함수들의 정의를 담고 있는 소스 프로그램 파일

## CounterMain.cpp
```c
#include <iostream>

#include "Counter.h"
using namespace std;

void main() {
	Counter cnt; //Counter 객체의 정의
	cnt.reset(); //숫자를 0으로 만듦
  	//cout << "현재 값 : " << cnt.value << endl; 불가능
	cout << "현재 값 : " << cnt.getValue() << endl;
	cnt.count();
	cnt.count(); //값을 1증가시킴
	cout << "현재 값 : " << cnt.getValue() << endl;
}
```

# 생성자
- 객체의 속성(즉, 데이터 멤버)은 객체의 현재 상태를 나타내는 값이다.
- 그러므로 처음 객체가 만들어졌을 때에는 그 객체의 초기 상태를 적절히 지정하여 두어야 한다.
- Counter 객체가 만들어진 직후 value의 값은 적절한 초기화 과정을 거치지 않았기 때문에 우리가 예측할 수 없는 임의의 값이다.
- 반드시 멤버함수 reset()을 먼저 호출하여 카운터의 초깃값을 0으로 만든 후 객체를 사용해야 올바른 결과를 얻을 수 있다.
- CntMain.cpp의 7행에서 객체를 정의한 후 8행에서 곧바로 reset()을 호출하여 초기화를 한 것은 이러한 이유 때문이다.
- 이와 같이 반드시 수행해야 하는 초기화 과정을 실수로 누락하면 프로그램은 올바로 동작하지 않는다.
- 이러한 실수가 발생하지 않도록 초기화 과정을 자동화할 수 있다면 불필요한 에러를 유발하지 않을 수 있다.
- 이러한 목적으로 사용할 수 있는 것이 생성자(constructor)이다.
- 생성자는 객체가 생성될 때 수행할 작업을 정의하는 특수한 멤버함수로서, 객체를 정의하는 문장에 의해 자동적으로 호출된다.

## 생성자의 선언
```c
class 클래스명{
   ...

public:
  클래스명(매개변수){ -> 생성자

  }

  ...
}
```
- 생성자는 기본적으로 일반 멤버함수와 유사하다.
- 매개변수를 통해 인수를 전달받을 수 있으며, 여러 가지 방법으로 사용될 수 있도록 생성자를 다중정의할 수도 있다.

### 생성자 생성 규칙
- 생성자는 클래스의 이름을 사용하여 선언한다.
- 생성자의 머리부에는 반환 자료형을 표시하지 않는다.
- 또한 몸체 내에서 return 명령으로 값을 반환할 수 없다.
- public으로 선언한 생성자만 클래스 외부에서 객체를 만드는 데 사용할 수 있다.

### Counter.h
```c
#ifndef COUNTER_H_INCLUDED

#define COUNTER_H_INCLUDED

class Counter {
	int value;

public:

	Counter() {
		value = 0; -> 생성자
	}
//이와 같이 생성자를 선언하면 Counter 객체를 정의할 때 생성자가 자동적으로 동작하며,
//이에 따라 value의 값이 0인 상태로 객체가 만들어진다.
//CntMain.cpp의 main() 함수에서 객체를 정의한 후 멤버함수 reset()을 일부러 호출할 필요가 없다.

	void reset() {
		value = 0;
	}

	void count() {
		value += 1;
	}

	int getValue() const {
		return value;
	}

	// const 멤버 함수 : 멤버 함수 내에서 데이터 멤베덜의 값을
	//변경하지 않겠다.

	//만일 const 멤버함수 내에서 데이터 멤버의 값을 수정하면 
	//컴파일러가 오류 메세지를 보낸다.
};

#endif 
```

### CounterMain.cpp
```c
#include <iostream>

#include "Counter.h"
using namespace std;

void main() {
	Counter cnt; //Counter 객체의 정의 - value는 자동으로 0이됨
	//cnt.reset(); //숫자를 0으로 만듦
	cout << "현재 값 : " << cnt.getValue() << endl;
	cnt.count();
	cnt.count(); //값을 1증가시킴
	cout << "현재 값 : " << cnt.getValue() << endl;
}
```

## 초기화 리스트
- 생성자가 하는 주요 작업으로는 데이터 멤버에 적절한 초깃값을 넣는 것이다.
- 예를 들어 Counter 클래스의 생성자에서는 value에 0을 대입하여 객체 상태의 초깃값을 설정하고 있다.
- 이와 같이 생성자에서 데이터 멤버의 값에 초깃값을 대입하는 것은 초기화 리스트(initializer list)를 이용하여 간결하게 표현할 수 있다.
- 초기화 리스트는 <b>함수의 머리에 콜론(:)을 기입하고 '변수명{초깃값}' 또는  '변수명(초깃값)' 형태로 초깃값을 지정</b>한 것이다.

### Counter.h 생성자 수정하기
```c
Counter(): value{0} {}
```

- 만일 초기화 항목이 여러 개 있을 때에는 쉼표(,)로 구분한다.

### CounterM.h 생성하기
```c
#ifndef COUNTERM_H_INCLUDED
#define	COUNTERM_H_INCLUDED

class CounterM{ //클래스 CounterM의 선언 시작
	const int maxValue; //private 값의 최대값 멤버변수
	int value; //private 멤버 변수

public:
//최대값을 매개변수를 통해 전달받는다.
	CounterM(int mVal) :maxValue{ mVal }, value{ 0 }{} //생성자
	void reset() { //값을 0으로 만듦
		value = 0;
	}
	void count() { //값을 1 증가시킴
		value = value < maxValue ? value + 1 : 0;
	}

	int getValue() const{ //현재 값을 반환함
		return value;
	}
};

#endif 
```
### CntMMain.cpp
```c
#include <iostream>
#include "CounterM.h"
using namespace std;

void main() {
//CounterM 클래스의 생성자는 1개의 int 값이 인수로 전달되어야 하므로
//객체를 선언할 때 이에 해당되는 실 매개변수를 포함해야 한다.
	CounterM cnt(9);

	cout << "현재 값 : " << cnt.getValue() << endl;
	for (int i = 0; i < 12; i++) {
		cnt.count();
//최댓값인 9까지 카운트하고 난 후 다시 계수를 하면 0으로 리셋되는 것을 볼 수 있다.
		cout << "현재값 : " << cnt.getValue() << endl;
	}
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

6. 상속을 통해 파생 클래스를 정의하는 경우 virtual을 지정하여 가상함수가 되도록 하는 것이 좋다.2)

## Person 클래스 만들기
- 사람을 나타내는 클래스를 선언하고자 한다. 사람 객체는 ‘…에 사는 …입니다’라고 자신을 알릴 수 있으며, 주소를 변경할 수 있다.

### Person 클래스가 가진 멤버변수
|속성|비고|
|----|-------|
|char* name|이름을 저장한다.|
|char* addr|주소를 저장한다.|

### Person 클래스가 가진 메서드
|메서드|비고|
|----|-------|
|Person(char* name, char* addr)|생성자|
|~Person()|소멸자|
|void print()|'~에 사는 ~입니다'라고 출력한다|
|vod chAddr(char* newAddr) | 주소를 변경한다.|

### Person.h
- Person.h에서는 멤버함수들의 원형만 선언하고 있다.
```c
#ifndef PERSON_H_INCLUDED
#define PERSON_H_INCLUDED

class Person {
	char* name;
	char* addr;

public:
	Person(const char* name, const char* addr);
	~Person();
	void print() const;
	void chAddr(const char* newAddr);
};

#endif
```

### Person.cpp
- Person.cpp는 Person 클래스의 멤버함수들을 정의한다.
```c
#include <iostream>
#include <cstring>
//Person.h를 삽입함으로써 Person 클래스가 선언되도록 하고 있다.
#include "Person.h"

using namespace std;

//Person 클래스의 멤버함수들이 정의되는데, 이제는 Person 클래스 선언문의 외부에 위치하므로
//각각의 함수들이 Person 클래스의 멤버임을 알려야 한다.

//이를 위해 멤버함수 이름 앞에 'Person::'을 기입하고 있다.

Person::Person(const char* name, const char* addr) {
//함수 내부에서 name이나 addr를 사용할 경우 형식 매개변수인 name이나 addr를 의미하게 된다
//이 이름들이 데이터 멤버를 지칭하게 하고 싶다면 구체적으로 소속을 알려야 하는데, 한 가지 방법은 this라는 포인터를 사용하는 것이다.
	//이름을 저장할 공간 할당
	this->name = new char[strlen(name) + 1]; //this는 객체 자기 자신을 가리키는 포인터이다.
	//데이터 멤버 name에 이름을 복사
	strcpy(this->name, name);
	//주소를 저장할 공간 할당
	this->addr = new char[strlen(addr) + 1];
	//데이터 멤버 addr에 주소를 복사
	strcpy(this->addr, addr);
	cout << "Person 객체 생성함(" << name << ")" << endl;
}

Person::~Person() {
	cout << "Person 객체 제거함(" << name << ")" << endl;
	delete[]name; //이름 저자공간 반납
	delete[]addr; //주소 저장공간 반납
}

void Person::print() const {
	cout << addr << "에 사는 " << name << "입니다" << endl;
}

void Person::chAddr(const char* newAddr) {
	delete[] addr;
	addr = new char[strlen(newAddr) + 1];
	strcpy(addr, newAddr);
}


```

### PersonMain.cpp
```c
#include <iostream>
#include "Person.h"
using namespace std;

void main() {
	Person* p1 = new Person("이철수", "서울시 종로구");
	Person* p2 = new Person("박은미", "강원도 동해시");
	p1->print();
	p2->print();
	cout << endl << "주소 변경: ";
	p2->chAddr("대전시 서구");
	p2->print();
	delete p1;
	delete p2;
}
```























