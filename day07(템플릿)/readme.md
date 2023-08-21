# 템플릿
- 함수나 클래스를 개별적으로 다시 작성하지 않아도, 여러 자료 형으로 사용할 수 있도록 하게 만들어 놓은 틀.
- 템플릿은 보관할 자료형을 인수로 하여 컨테이너 클래스나 함수를 생성할 수 있게 함으로써 개별적 자료형 각각에 대해 프로그램을 중복적으로 작성하는 노력을 줄일 수 있게 한다.

## 컨테이너와 템플릿
- 컨테이너(container) 클래스란 다른 객체를 저장하는 클래스로서, 그 예로 스택, 큐, 배열, 리스트 등을 들 수 있다.
- 컨테이너 클래스는 프로그램 작성에 매우 유용하다.
- 그러나 동일한 유형의 컨테이너일지라도 저장하고자 하는 객체의 유형이 다르면 이에 맞게 새로운 클래스를 선언하여야 한다.

## Stack1.h
```c
#ifndef STACK1_H_INCLUDED
#define STACK1_H_INCLUDED
typedef int STACK_ITEM;
class Stack {
	enum{MAXSTACK=20};
	int top;
	STACK_ITEM item[MAXSTACK];
	STACK_ITEM pop();
};
#endif // !STACK1_H_INCLUDED
```
- 정의된 클래스는 정수 자료를 저장할 수 있다.
- 그러나 스택이란 push(), pop() 동작에 의하여 LIFO(Last-In First-Out) 방식으로
- 자료를 보관하는 개념적인 자료구조로서 반드시 정수만 보관하라는 법은 없다.
- 스택에 다른 자료형 혹은 어떤 클래스의 객체를 보관하려면 프로그램을 어떻게 수정하여야 하는가?
- stack1.h 코드의 3행을 다음과 같이 수정하는 것이다.

```c
typedef float STACK_ITEM;
```

- 이제 스택은 float형 값을 저장하는 스택 클래스가 된다.
- 이처럼 자료형 이름을 대표하는 명칭을 typedef로 정의하는 문장을 이용하면 프로그램의 다른 부분을 수정하지 않고도
- 코드를 재사용할 수 있다.
- 그러나 이 방법으로 문제가 해결되지 못하는 상황도 있다.
- 만일 1개의 프로그램 안에 두 가지 자료형을 지원하는 스택을 사용하려면 어떻게 하여야 하는가?
- 예를 들어 정수형 스택과 실수형 스택이 모두 필요한 경우라면, 클래스 이름이 같기 때문에 다음과 같은 코드를 두 번 작성하여야 할 것이다.

```c
typedef int  INT_ITEM;

class StackInt {

  ……

};


typedef float  FLOAT_ITEM;

class StackFloat {

  ……  // StackInt와 동일한 코드

  ……  // 단, INT_ITEM만 FLOAT_ITEM으로 변경

};

```

- 두 클래스의 선언 부분뿐만 아니라 함수를 정의한 부분도 중복하여 작성하여야 한다.
- 예를 들어 멤버함수 push()의 경우 함수의 내용은 같더라도 함수가 속하는 클래스 이름과 자료형을 나타내는 부분은 다르다.

```c
void StackInt::push(INT_ITEM n) {

  item[--top] = n;

}



void StackFloat::push(FLOAT_ITEM n) {

  item[--top] = n;

}
```

- 이러한 코드 중복을 방지할 수 있는 방법이 템플릿(template)이다.
- 템플릿은 클래스를 선언할 때 객체의 자료형을 고려하지 않고, 객체의 자료형을 인수(argument)로 처리한다.
- 컨테이너 클래스를 선언할 때 특정 자료형이 아닌 일반적인 자료형을 대상으로 하는 템플릿을 작성한다.
- 특정 자료형을 위한 컨테이너 객체가 필요할 때 그 자료형을 템플릿의 매개변수로 전달하여 그 자료형에 해당되는 클래스가 자동적으로 선언되게 한다.
- 이것은 일반화 프로그래밍(generic programming) 패러다임에 해당되는 C++ 언어의 매우 유용한 기능에 해당된다.
- C++ 언어는 클래스 템플릿과 함수 템플릿을 지원한다.

## 클래스 템플릿
```c
template <템플릿 매개변수 목록> class 클래스 템플릿 이름 {

  ……

};
```
- template은 키워드이고 <템플릿 매개변수 목록>는 이 템플릿으로 전달되는 인수를 받기 위한 템플릿 매개변수를 선언하는 부분이다.
- 자료형을 받는 매개변수는 일반적으로 'typename T'또는 'class T'라고 기록하는데, 여기에서 'T'는 클래스 템플릿에 인수로 전달되는 자료형을 나타낸다.
- 클래스 템플릿 내에서는 ‘T’를 이 자료형의 이름 대신 사용한다는 의미이다.
- T는 예약어가 아니라 사용자 정의어이므로 'T'대신에 'typename ANY'와 같이 다른 이름을 적어도 무방하다.
- 이 경우에는 클래스 템플릿 안에서 임의의 자료형을 표현하기 위하여 ‘ANY’라고 적으면 된다.
- 템플릿 매개변수는 2개 이상일 경우도 있으며, 자료형의 이름이 아닌 일반적인 값도 템플릿 매개변수를 통해 받을 수 있다.
- 클래스 템플릿에 속한 멤버함수를 클래스 템플릿 선언 외부에서 하는 형식은 다음과 같다.
```c
template <templateParameters>
반환형 ClassTemplateName<templateParameters로 전달된 인수>::함수명(멤버함수의 형식 매개변수 목록)

{

  ……

}
```
- 클래스에서와 마찬가지로 멤버함수 이름 앞에 어느 클래스 템플릿에 소속된 것인가를 알리게 되어 있는 것을 볼 수 있다.
- 멤버함수를 구현할 때에도 인수로 전달되는 자료형 또는 클래스 이름이 필요한 곳에 ‘T’와 같은 템플릿 매개변수를 사용한다.

## 스택 클래스 템플릿의 선언
- 스택 템플릿의 선언은 다음과 같다.
```c
#ifndef STACK_TEMPLATE_H_INCLUDED
#define STACK_TEMPLATE_H_INCLUDED
#include <iostream>
using namespace std;

template <typename T>
class Stack {
	T *buf; //buffer pointer
	int top; //stack의 탑
	int size; //스택의 크기
public:
	Stack(int s); //생성자
	virtual ~Stack(); //소멸자
	bool full() const;
	bool empty() const;
	void push(const T& a);
	void push(T&& a);
	T&& pop();
};

template<typename T> Stack<T>::Stack(int s) :size(s), top(s) {
	buf = new T[s];
}
template<typename T> Stack<T>::~Stack() {
	delete[] buf;
}
template<typename T> bool Stack<T>::full() const {
	return !top;
}
template<typename T> bool Stack <T>::empty() const {
	return top == size;
}
template<typename T> void Stack <T>::push(const T& a) {
	buff[--top] = a;
}
template<typename T> void Stack <T>::push(T&& a) {
	buf[--top] = move(a);
}

template<typename T> T&& Stack<T>::pop() {
	return move(buf[top++]);
}
#endif // !STACK1_H_INCLUDED

```
- 'typename T'라는 하나의 템플릿 매개변수를 사용하는 클래스 템플릿 Stack을 선언한 것이다.
- T *buf;은 템플릿 매개변수에 지정된 'T'라는 이름으로 이 클래스 템플릿이 취급하는 임의의 자료형에 대한 포인터를 선언한 것이다.
- void push(const T& a);, void push(T&& a);, T&& pop(); 에서도 스택 템플릿이 보관할 자료형을 표현하는 부분에서 ‘T’를 사용하였다.
- 멤버함수 push를 2개 다중정의한 것을 볼 수 있는데
- void push(const T& a);에서의 push는 l-value가 인수로 전달되었을 때 buf의 항목에 복사를 하도록 한 것이고
- void push(T&& a);는 r-value가 인수로 전달되었을 때는 임시 객체의 내용을 buf의 항목에 이동하여 효율적으로 동작할 수 있도록 한 것이다.

## 멤버함수의 선언 위치
- 지금까지 클래스를 선언할 때 클래스 선언문은 헤더 파일(.h)에 작성하고, 클래스 선언문 외부에서 정의하는 멤버함수들은 .cpp 파일에 작성하였다.
- 그러나 클래스 템플릿의 경우는 다르다.
- 선언문 외부의 멤버함수들도 헤더 파일에 작성한다.
- 클래스 템플릿은 클래스가 아니라 클래스를 만드는 템플릿이며, 멤버함수의 선언도 직접 함수를 정의하지 않고 함수에 대한 템플릿을 선언만 하는 것이기 때문이다.
- 비록 클래스 템플릿 선언문 외부에 작성한다 할지라도 이 역시 헤더 파일에 작성하는 것이 올바른 방법이다.

## 객체의 정의
- 클래스 템플릿에 대한 객체를 정의할 때에는 실제 클래스 템플릿이 취급할 변수형 또는 클래스의 이름을 인수로 전달하여 정의한다.
```c
ClassTemplateName<ClassName> objName(constrArgs);

※ ClassName : 템플릿 매개변수에 전달할 클래스 또는 자료형 이름

※ objName : 정의할 객체의 이름

※ constrArgs : 생성자에 전달할 인수
```
- ClassName에는 문자형이나 정수형 등 기본 자료형뿐 아니라 사용자 정의 자료형을 사용할 수도 있다.

## StackTMain.cpp
```c
#include <iostream>
#include "StackT.h"
using namespace std;

void main() {
	Stack<char> sc(100); //char 스택 선언
	sc.push('a'); //char 스택 사용
	sc.push('b');
	cout << "CHAR STACK : ";
	while (!sc.empty()) {
		cout << sc.pop();
	}
	cout << endl;

	Stack<int> si(50); //int 스택 선언
	si.push(5); //int 스택 사용
	si.push(10);
	cout << "INT STACK : ";
	while (!si.empty()) {
		cout << si.pop() << " ";
	}
	cout << endl;
}
```
## 함수 템플릿
- 클래스 템플릿을 사용한다는 것은 멤버함수 템플릿을 사용한다는 것을 암시적으로 포함하고 있다.
- 그러나 클래스의 멤버함수가 아닌 전역함수에 대한 템플릿도 만들 수 있으며, 이것을 함수 템플릿이라고 한다.
- 함수 템플릿도 여러 가지 클래스의 객체에 대하여 동작한다.
- 함수 템플릿을 선언하는 방법은 클래스 템플릿의 멤버함수를 선언하는 방법과 동일하다.
```c
template <templateParameters>

ReturnType funcName(fParameterList) {
  ……      // 함수 몸체

}
```
- 매개변수 2개의 값을 서로 교환하는 함수를 임의의 자료형에 대하여 사용할 수 있도록 함수 템플릿으로 만든 코드는 다음과 같다.

## SwapFT.h
```c
#ifndef SWAP_FUNCTION_TEMPLATE_H_INCLUDED
#define SWAP_FUNCTION_TEMPLATE_H_INCLUDED
#include <utility>
using namespace std;
template <typename ANY>
void swapFT(ANY &a, ANY &b) {
	ANY temp = move(a);
	a = move(b);
	b = move(temp);
}
#endif // !SWAP_FUNCTION_TEMPLATE_H_INCLUDED
```

## SwapFTMain.cpp
```c
#include <iostream>
#include "SwapFT.h"
using namespace std;

void main() {
	int x = 10, y = 20;
	cout << "x = " << x << ", y = " << y << endl;
	swapFT(x, y);
	cout << "값 교환 후--> ";
	cout << "x=" << x << ",y = " << y << endl << endl;

	double num1 = 10.52, num2 = 20.24;
	cout << "num1 = " << num1 << ", num2 = " << num2 << endl;
	swapFT(num1, num2);
	cout << "값 교환 후--> ";
	cout << "num1=" << num1 << ",num2 = " << num2 << endl << endl;
}
```
- int형 변수를 함수 템플릿으로 전달하여 호출하였다.
- 이처럼 함수 템플릿을 호출하는 문장을 사용하면 사용된 인수의 자료형에 해당되는 swapFT() 함수가 자동적으로 정의되어 컴파일된다.
- 만일 함수 템플릿을 사용하지 않는다면, 다음과 같이 함수의 내부가 처리하는 객체의 형만 다른 2개의 함수를 각각 정의해야 하는 이중의 노력이 필요하다.

```c
void swapInt(int &a, int &b)  { …… }

void swapDouble(double &a, double &b)  { …… }
```






