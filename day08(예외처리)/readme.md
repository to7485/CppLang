# 예외처리
- 예외(exception)란 프로그램이 수행되는 도중에 정상적이지 않은 사건이 발생하는 것을 말한다.
- 예를 들어 프로그래머는 기억장치를 할당하는 함수 new를 사용할 때 항상 그 함수가 성공적으로 수행될 것으로 기대한다.
- 그러나 남아 있는 기억장치가 부족하거나 혹은 다른 어떤 이유로 new 함수가 프로그래머가 원하는 크기만큼의 기억장치를 할당하지 못하는 경우도 일어날 수 있다.
- 이와 같이 프로그래머가 생각하는 것과 다르게 주변 여건에 의하여 프로그램을 정상적으로 처리하지 못하는 상황이 예외이다.
- 능숙한 프로그래머일수록 프로그램을 작성할 때 자신이 의도한 정상적인 흐름 이외에 발생할 수 있는 예외에 대처할 수 있도록 프로그램을 작성하는 능력이 뛰어나다.

## 예외가 발생할수 있는 상황
### 변수의 값에 따라 연산을 수행하지 못하는 경우
- 수식 a/b를 계산하는 경우를 생각하여 보자.
- C++ 문법에 따라 a/b로  작성하였다면 컴파일러는 오류가 없는 것으로 판단하고 번역할 것이다.
- 그렇지만 연산을 수행할 때 우연히 b의 값이 0이었다면 이 수식을 계산할 수 없다.
```c
#include <iostream>
 
using namespace std;
 
int main()
{
	int a, b;
 
	cout << "두 개의 정수를 입력하세요: ";
	cin >> a >> b;
	cout << a << "를 " << b << "로 나눈 몫은 " << a/b << "입니다." << endl;
	return 0;
}
 ```
- 기존에는 이런 상황을 아래와 같이 처리를 했다.
```c
#include <iostream>
 
using namespace std;
 
int main()
{
	int a, b;
 
	cout << "두 개의 정수를 입력하세요: ";
	cin >> a >> b;
	if (b == 0) // 나누는 수가 0이라면,
		cout << "나누는 수가 0이 될 수 없습니다." << endl;
	else
		cout << a << "를 " << b << "로 나눈 몫은 " << a/b << "입니다." << endl;
	return 0;
}
```
- 이런 방식으로 예외 처리를 하게 되면, 예외가 발생할때마다 이렇게 처리를 해주어야 하기 때문에 똑같은 코드가 늘어나 코드만 차지하거나, 예외 처리의 구분이 명확하지 않습니다.

 ### 사용자의 입력이 프로그래머가 기대한 것과 다른 경우
 - 사용자가 비정상적인 값을 입력하는 경우도 있기 때문이다.
 - 예를 들어 날짜의 월을 저장하는 변수 month에 사용자가 1보다 작거나 12보다 큰 수를 입력하는 경우\
 - 프로그래머가 의도한 값이 입력된 것이 아니므로 이러한 경우도 예외이다.

 ```c
 int getMonth(){
   int month;
   bool exit_flag = false;
   while(!exit_flag){
     cout <<"월을 입력하세요: ";
     cin >> month;
     if (month < 1 || month > 12){
       cout<<"1 ~ 12사이의 값을 입력하세요.\n";
     }
     else {
        exit_flag = true;
     }
 ```
### 프로그램에서 자원을 필요로 할 때 컴퓨터가 자원이 부족한 경우
- 메모리의 할당에 실패하는 경우도 매우 중요한 예외 상황이다.
- 지금까지 new 연산자를 사용하면서 메모리의 할당에 실패하는 상황은 고려하지 않았지만
- 이러한 상황이 발생하는 경우는 프로그램이 정상적으로 동작할 수 없으므로 이러한 예외에 대비할 수 있어야 한다.

```c
int *p = new(nothrow) int[100000000];

if (!p) {

cerr << "메모리 할당 오류" << endl;  // 표준 오류 스트림으로 출력

exit(EXIT_FAILURE);

}
```
## C++ 언어의 예외처리 체계
- C++ 언어에서는 데이터의 오류, 시스템 자원(기억장치 공간, 입출력 장치)의 부족 등과 같은 비정상적이지만 예측할 수 있는 예외에 대처할 수 있는 예외처리 방법을 지원한다.

## 예외처리의 기본
- C++ 언어의 예외처리 체계는 try 블록과 catch 블록, 그리고 throw 문장으로 구성된다.

```c
try { // 예외가 발생하는 영역
   if (예외 조건) throw 예외 객체; // 예외가 발생하면 예외를 던지는 영역
} catch (예외 객체) { // 던져진 예외를 잡는 영역
   // 예외 처리 영역
}
```
- 예외가 발생할만한 영역을 try로 감싸주고
-  그 뒤에 try 영역 내에서 예외 조건이 만족하면, throw로 그 예외를 던집니다.
- 그러면 catch가 그 예외를 잡아 처리해줍니다.

## HMean.cpp
```c
#include <iostream>
 
using namespace std;
 
int main()
{
	int a, b;
 
	cout << "두 개의 정수를 입력하세요: ";
	cin >> a >> b;
 
	try {
		if (b == 0) throw b;
		cout << a << "를 " << b << "로 나눈 몫은 " << a/b << "입니다." << endl;
	} catch (int exception) {
		cout << "예외 발생, 나누는 수는 " << b << "가 될 수 없습니다." << endl;
	}
	return 0;
}
```
- 예외가 발생하지 않으면 catch 영역은 실행되지 않습니다.

## 함수 예외처리
- 이름이 func인, 두개의 정수를 인자로 받는 함수를 정의해봅시다
- 그 함수 내에 b가 0이면(나누는 수가 0이면) 예외를 던지도록 하고, 그 아래 몫을 출력하도록 해봅시다.
- 수에서 예외가 발생했을 경우에 어떻게 예외가 처리되는지 살펴보자.
```c
#include <iostream>
 
using namespace std;
 
void func(int a, int b)
{
	if (b == 0) throw b;
	cout << a << "를 " << b << "로 나눈 몫은 " << a/b << "입니다." << endl;
}
 
int main()
{
	int a, b;
 
	cout << "두 개의 정수를 입력하세요: ";
	cin >> a >> b;
 
	try {
		func(a, b);
	} catch (int exception) {
		cout << "예외 발생, 나누는 수는 " << b << "가 될 수 없습니다." << endl;
	}
	return 0;
}
```
- 함수 내부를 살펴보시면, 전달받은 b의 값이 0일 경우에 예외를 던지고 있습니다.
- 그런데, func 함수 내에는 예외를 처리하는 영역이 없기 때문에, func 함수가 호출된 영역으로 예외를 전달합니다.

## 예외처리 클래스
- 예외의 처리는 발생한 예외에 맞게 선별적으로 이루어져야 한다.
- 예외 객체의 자료형(클래스)이므로 문자열이나 정수 등 일반적인 자료형의 예외 객체를 사용하는 것보다는 예외의 요인별로 정의된 예외 클래스의 객체를 사용하는 것이 좋다.
- 이를 위해 예외처리 클래스를 사용할 수 있다.
- 예외처리 기능은 단순히 일반 함수에서 예외조건을 검사할 때에만 사용되는 것이 아니다.
- 클래스를 설계할 때에도 사용하면 예외처리의 강력한 진가를 더욱 발휘할 수 있다.
- 클래스에 예외처리 기능을 포함시키면, 객체에서 예외가 발생할 때 어느 부분에서 예외가 발생하였는지, 어떤 원인에 의하여 예외가 발생하였는지 쉽게 식별할 수 있다.

## IntArray1.h
```c
#ifndef INTARRAY1_H_INCLUDED
#define INTARRAY1_H_INCLUDED
#include <iostream>
using namespace std;

const int DefaultSize = 10;
class Array {
	int *buf;
	int size;
public:
	Array(int s = DefaultSize);
	~Array() { delete[] buf; }
	int& operator[](int offset);

	const int& operator[](int offset)const;
	int getsize() const { return size; };
	friend ostream& operator<<(ostream&, Array&);
	class BadIndex; //예외처리를 담당할 예외처리 클래스인 BadIndex
	//예외가 발생한 경우는 예외처리 담당 클래스 BadIndex를 생성하여 던진다
};
#endif // !INTARRAY1_H_INCLUDED
```

## IntArray1.cpp
```c
#include <iostream>
#include <cstring>
#include "IntArray1.h"
using namespace std;

Array::Array(int s) :size(s) {
	buf = new int[s];
	memset(buf, 0, sizeof(int)*s);
}

int& Array::operator[](int offset) {
	if (offset < 0 || offset >= size) {//예외조건 검사
		throw BadIndex(); //예외처리 생성 및 전달
	}
	return buf[offset];
}

const int& Array::operator[](int offset) const {
	if (offset < 0 || offset >= size) {//예외조건 검사
		throw BadIndex(); //예외처리 생성 및 전달
	}
	return buf[offset];
}

ostream& operator << (ostream &os, Array &arr) {
	for (int i = 0; i < arr.getsize(); i++) {
		if (i % 5 == 0) {
			os << endl;
		}
		os << "[" << i << "] = " << arr[i] << " ";
	}
	return os;
};
```
## IA1Main.cpp
```c
#include <iostream>
#include "IntArray1.h"
using namespace std;

void main() {
	Array arr(10);
	try {
		for (int i = 0; i <= 10; i++) { //마지막 반복에서 예외 발생
			arr[i] = i;
		}
	}
	catch (const Array::BadIndex &e) {
		cerr << "인덱스 범위 오류" << endl;
	}
	cout << arr << endl;
}
```
-  IntArray1.cpp의 operator[]를 수행하는 도중에 14행에 의하여 검출되고
-  15행에서 클래스 Array에 내포되어 있는 예외처리 전담 클래스인 BadIndex를 생성하여 던진다.

# 입출력 스트림
- cin과 cout을 사용하여 기본적인 입력과 출력을 수행하였다.
- C++ 언어에서는 스트림 객체를 통하여 입출력을 수행하는데, 입출력과 관련이 있는 클래스들에 대하여서는 <iostream>에 정의되어 있다.

## 스트림
- C++ 프로그램은 입력과 출력을 ‘문자의 흐름(스트림)’으로 취급한다.
- 컴퓨터의 입출력에서 주로 취급하는 정보의 단위는 문자이다.
- 물론 컴퓨터 내부에서는 정수와 실수도 사용하지만, 키보드나 화면으로 입출력될 때에는 문자로 변환되어 입력되거나 출력된다.
- 스트림이란 시냇물과 같이 문자들이 계속 흘러간다는 의미이다.
- 입력장치와 프로그램 사이 그리고 프로그램과 출력장치 사이에 문자가 흘러갈 수 있는 통로가 있다고 생각하는 것이다.

![image](https://github.com/to7485/CppLang/assets/54658614/4b4c5114-6b12-4c89-8c67-f344e1242d51)

- 컴퓨터는 입출력 장치를 모두 디스크의 파일과 동일하게 취급한다.

### 입력스트림
- 프로그램에서 사용하는 입력장치에서 프로그램으로 들어온다.

### 출력스트림
- 프로그램에서 출력장치로 문자가 흐른다.

### 프로그램의 흐름
- 프로그램에서 데이터를 입력할 때에는 자료가 흘러오는 장치와 무관하게 입력 스트림에서 문자를 받아 가면 되고,
- 데이터를 출력할 때에는 출력될 장치와 무관하게 출력 스트림으로 데이터를 보내 주면 된다.
- 프로그램은 입출력 장치마다 하나씩 입력 혹은 출력 스트림을 연결하여 사용할 수 있다.

## 버퍼
- 프로그램과 입출력 장치 간에 데이터를 원활하게 전송할 목적으로 버퍼를 사용한다.
- 버퍼는 물을 저장하는 물탱크와 같은 역할을 수행한다

![image](https://github.com/to7485/CppLang/assets/54658614/2f571048-087c-45e2-86bc-be1a4f98654d)

- 물탱크에 물은 불규칙적으로 공급된다.
- 한 번 공급되면 물탱크를 채워놓고, 사용자들은 필요한 만큼 일정한 속도로 물을 사용할 수 있다.
- 일반적으로 디스크는 데이터 블록 단위로 데이터를 전송하지만 프로그램은 한 번에 한 바이트씩 데이터를 전송한다.
- 입출력 장치는 컴퓨터에 비하여 속도가 느리다.
- 파일에 데이터를 기록하는 경우 출력할 때, 프로그램에서 1개의 문자를 기록할 때마다 디스크에 기록하는 것은 시간낭비이다.
- 따라서 프로그램은 버퍼에 데이터를 기록하고 버퍼가 다 채워지면 한 번에 디스크로 기록함으로써 프로그램을 빠르게 실행할 수 있다.
- 버퍼는 운영체제가 프로그램을 빠르게 실행시키기 위하여 지원하는 기능이므로 프로그래머는 그 존재를 인식할 필요가 없다.

## 표준 입출력 클래스
- C++ 언어는 입출력 스트림과 버퍼를 사용할 수 있도록 표준 입출력 클래스를 지원하고 있다.
- 그중에서 프로그래머가 알아 두면 도움이 되는 클래스들이 있다.
 - ios_base 클래스 : 스트림 클래스의 기초 클래스로 입력 및 출력 스트림에 공통적인 저장공간 및 멤버함수들을 포함한다.
 - ios 클래스 : ios_base 클래스를 상속받으며, streambuf 객체 포인터를 멤버로 보유하고 있다.
 - streambuf 클래스 : 기억장치의 버퍼를 액세스하고 관리하는 속성 및 멤버함수를 지원한다.
 - istream 클래스 : ios 클래스를 상속받으며, 입력을 위한 멤버함수를 제공한다.
 - ostream 클래스 : ios 클래스를 상속받으며, 출력을 위한 멤버함수를 제공한다.
 - iostream 클래스 : istream 클래스와 ostream 클래스를 모두 상속받으며, 입력을 위한 멤버함수와 출력을 위한 멤버함수를 모두 제공한다.
 - fstream, ifstream, ofstream 클래스 : 파일 입출력을 위한 스트림이다.
- 프로그램에 <iostream>을 포함시키면 다음과 같은 4개의 스트림 객체가 자동으로 생성된다.
- 그러므로 이 객체들은 프로그래머가 명시적으로 선언하지 않고 사용할 수 있다.









