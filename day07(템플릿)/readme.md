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
- 벡터 또는 배열의 원소를 크기에 따라 분류하는 함수 sort()도 자주 사용되는 함수 템플릿의 일종이다.

## SortFT.h
```c
#ifndef SORTFUNC_H_INCLUDED
#define SORTFUNC_H_INCLUDED
#include "SwapFT.h"

//bubble sort algorithm
template<typename T> void sortFT(T arr[], int size) {
	bool doAgain = true;
	for (int i = 1; doAgain; i++) {
		doAgain = false;
		for (int j = 0; j < size - i; j++) {
			if (arr[j] > arr[j + 1]) {
				swapFT(arr[j], arr[j + 1]), doAgain = true;
			}
		}
	}
}
#endif // !SORTFUNC_H_INCLUDED
```

## SortMain.cpp
```c
#include <iostream>
#include "SortFT.h"
using namespace std;

void main() {
	int x[10] = { 6,0,3,1,2,9,4,5,7,8 };
	sortFT(x, 10);
	for (auto i : x) {
		cout << x[i] << " ";
	}
	cout << endl;
	cout << endl;
	double y[10] = { 6.6,0.0,3.3,1.1,2.2,9.9,4.4,5.5,7.7,8.8 };
	sortFT(y, 10);
	for (auto i : y) {
		cout << i << " ";
	}
	cout << endl;
}
```
## 함수 템플릿의 다중정의
- 함수 템플릿은 일반 함수와 마찬가지로 다중정의를 허용한다.
- 즉, 함수 템플릿과 이름이 같은 다른 함수가 존재할 수도 있다.
- 이름이 같은 함수가 여러 개 있을 때에는 함수 이름과 인수들의 데이터 형이 정확하게 일치하는 함수가 먼저 선택되어 호출되고,
- 그러한 함수가 없을 때에는 호출한 함수와 정확하게 일치시키는 변환이 가능한 함수 템플릿이 호출된다.

## 표준 템플릿 라이브러리
- 표준 템플릿 라이브러리(Standard Template Library)는 C++ 언어가 제공하는 템플릿을 사용한 컨테이너 클래스를 제공하는 라이브러리다.
- 벡터, 리스트, 큐 그리고 스택 등의 클래스 템플릿과 그 안에 저장된 데이터를 검색하고 정렬하는 등의 처리를 위한 함수들이 제공된다.
- 프로그래머들이 많이 사용하는 공통적인 컨테이너 클래스를 제공하는 것이다.
- 매우 유용하고, 최적화가 잘 되어 있어 유사한 기능을 사용자가 직접 작성하는 것에 비해 효율적이니 STL을 충분히 활용하는 것이 좋다.

## STL의 구성요소
- STL은 여러 가지 요소로 구성되어 있는데, 그중 핵심적인 것은 컨테이너, 반복자(iterator), 알고리즘이다.

### 컨테이너
- 컨테이너는 데이터를 저장하는 것으로, 저장하는 데이터는 int나 float과 같은 기본 자료형의 값일 수도 있고 사용자가 정의한 클래스의 객체들이 될 수도 있다.
- 물론 C++에서 제공하는 배열 역시 컨테이너와 같은 역할을 한다.
- STL에서 제공하는 컨테이너들을 사용하게 되면 각각의 용도에 따라 효과적인 알고리즘을 제공받을 수 있어 편리하고 효율적이다.
- 컨테이너에는 순차(sequence) 컨테이너, 연상(associative) 컨테이너, 무순서(unordered) 연상 컨테이너 등의 기본 컨테이너와 컨테이너 어댑터(container adaptor)가 있다.
    - 순차 컨테이너 : 동일한 자료형의 객체들을 선형적인 구조로 저장하는 것이다. vector, list, deque 등이 이에 해당된다.
    - 연상 컨테이너 : 탐색 트리와 같은 인덱스 구조를 이용하는 컨테이너로서 키(key)를 이용한 검색기능을 제공하는 것이다. set, multiset, map, multimap 등이 연상 컨테이너에 해당되는 것들이다. 
    - 무순서 연상 컨테이너 : 연상 컨테이너처럼 키를 이용한 검색기능을 제공하나, 해시함수를 이용함으로써 데이터 검색 시간이 데이터 수에 관계없이 일정하다.
    - 컨테이너 어댑터 : 기본 컨테이너를 기반으로 특정 용도에 맞게 유도된 컨테이너이다. queue, priority_queue, stack 등이 이에 해당된다.
 
### 반복자
- 반복자는 포인터의 개념이 일반화된 것으로, 컨테이너에 저장된 원소들을 액세스하는 공통적인 방법으로 사용된다.
- 컨테이너의 유형에 따라 서로 다른 형태의 반복자가 사용된다.
- 순방향 반복자(forward iterator)는 컨테이너의 앞쪽으로만 움직일 수 있으며, ++ 연산자가 이를 위해 사용된다.
- 양방향 반복자(bidirectional iterator)는 앞과 뒤로 움직일 수 있으며, 이를 위해 ++, -- 연산자가 정의되어 있다.
- 랜덤 액세스 반복자는 양방향 반복자의 기능과 함께 임의의 위치로 이동할 수 있다.

### 알고리즘
- STL 알고리즘들은 반복자를 사용하여 컨테이너의 원소에 대해 적용할 수 있는 여러 가지 연산을 제공한다.
- 알고리즘에는 컨테이너의 반복자가 인수로 전달되어 그 알고리즘이 어떤 객체 또는 범위에 대해 동작을 할 것인지를 지정한다.

|알고리즘|목적|
|------|-----|
|search|지정된 값과 동일한 첫 번째 원소를 반환|
|count|지정된 값을 갖는 원소의 수를 반환|
|swap|컨테이너 안의 값을 교환|
|sort|컨테이너의 값들을 지정된 순서에 따라 정렬|
|merge|두 정렬된 영역의 원소들을 합병|
|reverse|list의 원소들을 역순으로 나열|
|remove|컨테이너에서 지정된 값을 제거|
|replace|지정된 값을 다른 값으로 대체|
|unique|인접 위치에 있는 중복된 값을 제거|
|for_each|지정된 함수를 컨테이너의 모든 원소에 적용|

## vector
- 지금까지는 연속적인 공간에 동일한 자료형의 값들을 저장하고자 할 때 배열을 사용하였다.
- vector는 배열의 개념을 구현한 컨테이너로, 일반적인 배열의 기능을 포함하면서 여러 가지 유용한 멤버함수 및 관리 기능이 도입되어 있다.
- 특히 배열처럼 크기가 고정되어 있지 않고 필요에 따라 저장공간을 확장할 수 있다.

### vector 객체의 선언
- vector를 사용하기 위해서는 #include 명령으로 헤더 파일 <vector>를 프로그램에 삽입한다.
```c
vector<ClassName> objName(n);

※ n : 벡터에 저장할 객체의 수
```
- 예를 들어 10개의 float 값을 저장하기 위한 vector는 다음과 같이 선언한다.
```c
vector<float> fVecor(10);
```
- 이렇게 생성된 fVector는 배열처럼 사용할 수 있다.
- 그러나 10개의 float만 저장할 수 있는 것으로 고정된 것이 아니라 필요에 따라 저장공간을 확장할 수 있다.
- vector는 배열처럼 [] 연산자를 이용하여 객체 저장공간을 사용할 수 있다.
```c
vector fVector(10);

fVector[2] = 10.0f;

cout << fVector[2];
```
- 첨자를 지정하여 fVector의 개별 공간에 값을 저장하거나 읽을 수 있다.

### vector의 멤버함수
- size() : 논리적 크기를 알려 준다.
- capacity() : 실제로 확보되어 있는 메모리의 크기(물리적 크기)를 알려 준다.
- push_back() : 벡터의 끝에 데이터를 추가
- insert() : 지정된 위치에 데이터를 삽입한다.
- pop_back() : 마지막 데이터를 제거
- erase() : 지정된 위치의 데이터를 삭제한다.

## Vector1.cpp
```c
#include <iostream>
#include <vector>
using namespace std;

void main() {
	vector <int> intVec(5); //벡터 객체 생성

	for (int i = 0; i < intVec.size(); i++) {
		intVec[i] = i + 1;
	}
	cout << "벡터의 논리적 크기 : " << intVec.size() << endl;
	cout << "벡터의 물리적 크기 : " << intVec.capacity() << endl;
	cout << "저장된 데이터 : ";
	for (int i = 0; i < intVec.size(); i++) {
		cout << intVec[i] << " ";
	}

	cout << endl << endl << "1개의 데이터 push_back" << endl;
	intVec.push_back(11);

	cout << "벡터의 논리적 크기 : " << intVec.size() << endl;
	cout << "벡터의 물리적 크기 : " << intVec.capacity() << endl;
	cout << "저장된 데이터 : ";
	for (int i = 0; i < intVec.size(); i++) {
		cout << intVec[i] << " ";
	}

	cout << endl << endl << "5개의 데이터 push_back" << endl;
	for (int i = 1; i <= 5; i++) {
		intVec.push_back(i+11);
	}

	cout << "벡터의 논리적 크기 : " << intVec.size() << endl;
	cout << "벡터의 물리적 크기 : " << intVec.capacity() << endl;
	cout << "저장된 데이터 : ";
	for (int i = 0; i < intVec.size(); i++) {
		cout << intVec[i] << " ";
	}

	cout << endl << endl << "3개의 데이터 pop_back" << endl;
	for (int i = 0; i < 3; i++) {
		intVec.pop_back();
	}
	cout << "벡터의 논리적 크기 : " << intVec.size() << endl;
	cout << "벡터의 물리적 크기 : " << intVec.capacity() << endl;
	cout << "저장된 데이터 : ";
	for (int i = 0; i < intVec.size(); i++) {
		cout << intVec[i] << " ";
	}
	cout << endl;

}
```

















