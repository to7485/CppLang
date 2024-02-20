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

# 디폴트 생성자
- 매개변수가 없는 생성자 또는 매개변수가 있지만 모두 디폴트 값이 있는 디폴트 인수만 포함하고 있는 생성자이다. 
- Counter.h 파일의 Counter생성자는 매개변수를 가지고 있지 않으므로 디폴트 생성자이다.
- 만일 클래스를 선언할 때 생성자를 선언하지 않으면 컴파일러는 아무런 일도 하지 않는 디폴트 생성자를 만든다.
- 그러나 생성자를 하나라도 선언하면 이와 같은 디폴트 생성자를 자동으로 만들지 않으며
- 객체를 정의할 때에는 선언된 생성자에 맞는 형식으로 정의하여야만 한다.
- CounterM.h 파일의 클래스 CounterM은 8행에 1개의 매개변수를 갖는 생성자를 선언하였으므로 디폴트 생성자가 자동으로 만들어지지 않는다.
- 그러므로 CounterM 클래스의 객체를 정의할 때에는 반드시 다음의 1번과 같이 인수를 포함하여야 한다.
- Counter 클래스에서처럼 ②와 같이 인수가 없는 형식으로 객체를 정의하는 것은 허용되지 않는다.

```c
CounterM  cnt1(999); 	// 1. 0부터 999까지 카운트하는 계수기 객체 정의

CounterM  cnt2; 	// 2. 에러 : CountM은 디폴트 생성자가 없음
```


# 복사 생성자
- 같은 클래스의 객체를 복사하여 객체를 만드는 생성자이다.

## 컴파일러가 자동적으로 제공하는 복사 생성자. 
- 만일 복사 생성자를 명시적으로 선언하지 않으면 컴파일러는 원본 객체의 멤버들을 그대로 복사하여 객체를 정의하는 복사 생성자를 자동으로 만든다.
- 
```c
CounterM cnt4(99); 		// 1. 0부터 99까지 카운트하는 계수기 객체

CounterM cnt5(cnt4); 	// 2. cnt4를 복사한 계수기 객체 cnt5 정의

CounterM cnt6 = cnt4; 	// 3. cnt4를 복사한 계수기 객체 cnt6 정의
```
- cnt4는 데이터 멤버 maxValue와 value가 각각 99와 0인 상태로 정의될 것이다.
- 그런데 CounterM에는 2번과 같이 CounterM 클래스의 객체가 인수인 생성자는 명시적으로 선언하지 않았다.
- 컴파일러가 데이터 멤버를 1:1로 복사해 주는 복사 생성자를 자동적으로 만들어 놓기 때문에 2번과 같은 문장이 가능하며
- cnt5 역시 maxValue는 99, value는 0이라는 값으로 초기화된다.
- 3번의 문장은 대입이 아닌 초기화 문장이며, 이 경우에도 복사 생성자가 동작한다.
- 다음의 문장과는 다르다.
```c
CounterM cnt7(99);

cnt7 = cnt4;  	// 4. 에러! 이 문장은 초기화 문장이 아닌 대입 연산자임
```
- 문장4는 대입 연산자가 적용되는 문장이다.
- 컴파일러는 3과 같은 초기화와 4와 같은 대입을 다른 방법으로 취급한다.
- const 데이터는 초기화는 가능하지만 대입은 할 수 없다.

## 명시적으로 복사 생성자 정의하기
- 주의할 점은 형식 매개변수가 그 클래스의 참조형이어야 한다는 것이다.
- 문제는 객체에 필요한 메모리를 동적으로 할당받고 반납하도록 되어 있는 클래스에서 발생할 수 있다.
```c
class  CounterM {

……

  CounterM(const CounterM& c)

    : maxValuec.maxValue, valuec.value

……

 };
```

## vecF 클래스
- 벡터 객체를 만들 수 있는 VecF 클래스를 정의해보자.
- VecF 객체는 float 값의 수를 인수로 지정하여 생성된다.
- 저장할 값의 배열이 제공될 경우 그 값으로 초기화 한다.
- 인수로 전달된 VecF 개겣와 덧셈한 결과를 반환할 수 있다.
- 객체에 저장된 벡터를 출력할 수 있다.

### VecF클래스의 메서드
|메서드|비고|
|----|-------|
|VecF(int d, float* a)|생성자|
|~VecF()|소멸자|
|VecF add(const VecF& fv)|벡터의 덧셈을 한다.|
|void print() | 벡터를 출력한다|

### VecF클래스의 속성
|속성|비고|
|----|-------|
|int n|벡터의 크기를 저장한다.|
|float* arr|벡터의 저장공간 포인터|

## VecF.h
```c
#ifndef VEC_F_H_INCLUDED
#define VEC_F_H_INCLUDED
#include <iostream>
#include <cstring>
using namespace std;

class VecF {
	int n; //
	float *arr;

public:
//함수 호출 시 인자를 넣지 않으면, "미리 정해준 인자 값"이 대신 들어가게 됩니다.
//반대로, 인자로 값을 넣게 되면 "미리 정해둔 인자 값"이 아니라 지금 넣는 인자가 들어가게 됩니다.
//1) 디폴트 값을 지정해줄 때는 뒤에서부터 지정한다.
//2) 중간에 비어있는 게 있으면 절대 안 된다.
//void addNum(int a = 10, int b); 만약 이게 가능하다고 한다면,
//addNum(100)에서 100의 값이 a를 가리키는지 b를 가리키는지 혼동이 올 수 있습니다.

	VecF(int d, float* a = nullptr) :n{ d } {
		arr = new float[d];
		if (a) {
			memcpy(arr, a, sizeof(float)*n); //n개의 float값을 저장할 수 있는 공간을 동적으로 할당하여 객체를 생성한다.
		}
	}
	~VecF() { //할당된 메모리를 반납한다.
		delete[] arr;
	}
	VecF add(const VecF& fv) const {
		VecF tmp(n); //벡터의 덧셈 결과를 저장할 임시 객체
		for (int i = 0; i < n; i++) {//객체가 저장하고 있는 벡터와 형식 매개변수 fv를 통해 전달되는 벡터를 더한다.
			tmp.arr[i] = arr[i] + fv.arr[i];
		}
		return tmp; //덧셈 결과를 반환함
	}

//'[',와']' 사이에 벡터의 값들을 출력한다.
	void print() const {
		cout << "{ ";
		for (int i = 0; i < n; i++) {
			cout << arr[i] << " ";
		}
		cout << "]";
	}

};

#endif // !VEC_F_H_INCLUDED
```

## VFMain1.cpp
```c
#include <iostream>
#include "VecF.h"
using namespace std;

void main() {
	float a[3] = { 1,2,3 };
	VecF v1(3, a); //1,2,3을 저장하는 벡터
	VecF v2(v1); //v1을 복사하여 v2를 만듦
	v1.print();
	cout << endl;
	v2.print();
	cout << endl;
}
```
- 실행하면 에러남

### 얕은복사
- VecF 클래스에는 복사 생성자가 없으므로, 객체의 데이터 멤버를 그대로 복사하도록 묵시적으로 선언된 복사 생성자가 동작한다.
- 프로그램을 빌드하여 실행시키면 v1의 값과 v2의 값이 출력된다. 그 다음 main을 마치면서 v1과 v2의 소멸자가 실행된다.
- 그런데 컴파일러가 묵시적으로 만드는 복사 생성자는 데이터 멤버를 그대로 복사하는 것 외에 다른 처리는 하지 않으므로, v2의 arr는 v1의 arr를 그대로 복사한다.
- 이때 arr는 포인터이므로 이 복사에 의해 v2의 arr는  v1의 arr가 가리키는 공간을 함께 가리키는 것으로 처리된다.
- 데이터가 올바르게 출력되나 소멸자가 동작할때 발생한다.
-  v1과 v2는 별개의 객체처럼 보이지만, 사실은 데이터를 저장하는 공간을 함께 가리키므로 완전히 별개의 객체라고 할 수 없다.
-  main() 함수의 종료시점이 되면 지역변수인 객체 v1과 v2가 제거되며, 이때 소멸자가 동작한다.
-  소멸자에서는 arr가 가리키는 공간을 반납한다. 만일 v1이 먼저 제거되는 대상이라면 v1의 arr가 가리키는 공간이 반납된 후 v1이 제거된다.
-  이 상태에서 v2의 소멸자가 동작하면 v2의 arr가 가리키는 공간을 반납하려 할 것이다. 그런데 v2의 arr가 가리키는 공간은 이미 v1의 소멸자를 통해 반납되어 더 이상 이 프로그램의 영역이 아니다. 그러므로 이 반납명령은 거부되며, 그 결과 에러가 발생하는 것이다.

![image](https://github.com/to7485/CppLang/assets/54658614/6e909a2d-0c69-4c8b-8fd7-cc1a92c95b33)
  

### 깊은복사
- 이 문제를 해결하는 방법은 클래스 VecF에 복사 생성자를 명시적으로 선언하되, arr가 가리킬 공간을 별도로 할당하고 내용을 복사하게 하는 것이다.
- VecF 클래스는 복사되는 객체의 이름을 저장하기 위한 공간을 별도로 할당받아 이름을 복사하도록 복사 생성자를 선언하고 있다.

### VecF.h 코드 추가하기
```c
#ifndef VEC_F_H_INCLUDED
#define VEC_F_H_INCLUDED
#include <iostream>
#include <cstring>
using namespace std;

class VecF {
...

	VecF(const VecF& fv) :n{ fv.n } {
		arr = new float[n];
		memcpy(arr, fv.arr, sizeof(float)*n);
	}

...

};

#endif // !VEC_F_H_INCLUDED


```
- 이와 같이 하면 복사 생성자를 통해 v1을 복사한 객체 v2를 생성한 결과 각각 별개의 저장공간을 가리키게 된다.
- v1과 v2는 완전히 별개의 객체가 되어, v1이 소멸되더라도 v2는 온전한 상태로 남아 있어 문제가 되지 않는다.
![image](https://github.com/to7485/CppLang/assets/54658614/c75f8067-74e4-4bba-a8a9-1871458ed851)

# 이동생성자
- 깊은 복사를 하는 복사 생성자를 이용하면 안전하지만 실행 효율이 떨어지는 원인이 되기도 한다.

```c
#include <iostream>
#include "VecF.h"
using namespace std;

void main() {
	float a[3] = { 1,2,3 };
	float b[3] = { 2,4,6 };
  //VecF 클래스의 객체 v1과 v2를 만들었다.
	VecF v1(3, a);
	VecF v2(3, b);

  //v1과 v2의 합을 구한 결과로 v3을 만들었다.
	VecF v3(v1.add(v2));
	v1.print();
	cout << " + ";
	v2.print();
	cout << " = ";
	v3.print();
	cout << endl;
}
```
- VecF 클래스의 add 함수는 덧셈 결과를 저장할 임시 객체 tmp를 만들어 덧셈을 한 후 함수의 결과로 반환한다.
- 이렇게 반환된 객체는 복사 생성자를 이용하여 v3에 깊은 복사를 한 후 소멸된다.
- 그런데 반환된 객체는 소멸될 객체이므로 굳이 깊은 복사를 하는 것은 비효율적이다.
- 특히 크기가 큰 벡터일수록 이러한 비효율에 의한 손실이 크다. 이동 생성자(move constructor)를 이용하면 이러한 상황의 효율을 개선할 수 있다.

- 이동생성자는 r-value 참조를 사용한다.
```c
int a = 3;
a 는 메모리 상에서 존재하는 변수다.
a 의 주소값을 & 연산자를 통해 알아 낼 수 있다.
```
- 주소값을 취할 수 있는 값을 Lvalue 라고 부른다.
- '3' 은 주소값을 취할 수 있는게 아니다.
- '3' 은 위 표현식을 연산할 때만 잠깐 존재하고 연산후에는 사라지는 값이다. 즉, '3' 은 실체가 없는 값입니다.
- 주소값을 취할 수 없는 값을 Rvalue 라고 부른다.

- Lvalue가 식의 왼쪽 오른쪽 모두 올 수 있는 반면, Rvalue는 식의 오른쪽에만 존재해야 한다.
- L-value 참조는 '&' 기호로 선언하는 반면
- R-value 참조는 '&&' 기호로 선언한다.
```c
class ClassName {

  ……
  
  ClassName(ClassName&& obj) {  // 이동 생성자
  
  ……  // 생성되는 객체에 obj의 내용을 이동
  
  }

};
```
- 여기에서 '&&'는 r-value 참조를 나타낸다.

### VecF.h 코드 추가하기
```c
#ifndef VEC_F_H_INCLUDED
#define VEC_F_H_INCLUDED
#include <iostream>
#include <cstring>
using namespace std;

class VecF {

...

	VecF(const VecF& fv) :n{ fv.n } {
		arr = new float[n];
		memcpy(arr, fv.arr, sizeof(float)*n);
	}
//fv는 VecF 클래스의 객체에 대한 r-value 참조로, 이를 통해 받은 객체의 arr를 생성되는 객체의 arr에 복사하였다.
//fv에 nullptr를 저장함으로써 소멸자에 의해 객체만 소멸되게 하였다.
//그 결과 fv가 소유하고 있던 동적으로 할당된 메모리가 새로 생성되는 객체에 이동됐다.
	VecF(VecF&& fv) : n{ fv.n }, arr{ fv.arr }{
		fv.arr = nullptr;
		fv.n = 0;
	}

...

};

#endif // !VEC_F_H_INCLUDED
```
- VFMain2.cpp에서 VecF v3(v1.add(v2));에 v1.add(v2);의 연산 결과는 tmp 이고 tmp는 각 배열의 요소의 합이기 때문에 R-value가 되어
- 이동 생성자가 동작하게 된다.

![image](https://github.com/to7485/CppLang/assets/54658614/4bc81394-d201-4afd-86f8-d8c26b468ade)

- VecF(VecF&& fv)에서 temp의 값 {3,6,9}를 arr이 가리키게 하고  return된 값의 arr 항목은 nullptr로 만들어준다.
- v1.add(v2)는 v3로 값을 이동하고 v1.add(v2)는 사라진다고 보면 된다.


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




















