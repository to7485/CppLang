# C++ 언어
- C++는 AT&T 벨 연구소의 비야네 스트롭스트룹이 C언어를 확장해서 만든 객체지향 프로그래밍 언어이다.
- 따라서 C가 가지고 있는 모든 기능이 C++안에 포함되어 있고, 몇 가지 새로운 내용이 C++에 추가되었다.
- C++는 C를 기반으로 개발된 언어이므로 몇 가지를 예외를 제외하고는 C의 문법을 그대로 따른다.
- 그러므로  C처럼 범용 프로그래밍 언어로서 다양한 응용프로그램을 개발하는데 활용될 수 있고, 시스템 프로그래밍을 위한 언어로도 활용할 수 있다.

# C++ 프로그램의 작성 및 빌드

## 소스 프로그램의 작성
- C++로 작성한 프로그램을 담고 있는 파일을 소스파일이라고 한다.
- C++프로그램의 소스 파일에는 C++소스 프로그램 파일과 C++ 헤더(header)파일이 있다.
### C++소스 프로그램 파일
- 간단한 프로그램이라면 하나의 소스 프로그램 파일에 프로그램을 완성할 수도 있다.
- 그러나 프로그램의 규모가 커지거나, 여러 개발자가 프로그램을 분담하여 작성할 경우에는 여러 개의 소스 프로그램 파일을 나누어 프로그램을 작성하게 된다.

### C++헤더(header)파일
- 여러 소스 프로그램 파일에 공통적으로 들어가야 하는 부분이 필연적으로 발생할 수 있다.
- 전역변수나 함수의 원형, 클래스 등의 선언문이 이러한 부분에 해당한다.
- 이런 공통적인 부분을 각각의 소스 프로그램 파일에 중복하여 작성하면 프로그램을 관리하기 어려워지고 이로인해 오류가 발생하기도 한다.
- C++ 헤더 파일은 이와 같은 공통부분을 별도로 작성한 것으로 보통 '.h'라는 확장자를 사용한다.
- 헤더파일은 단독으로 컴파일 되지 않고, #include라는 선행처리기 지시어에 의해 소스 프로그램에 삽입되어 함께 컴파일 된다.

## 프로그램의 빌드
- 소스 파일에 담겨 있는 프로그램은 컴퓨터가 직접 실행할 수 있는 프로그램이 아니다.
- 이것을 컴퓨터가 이해할 수 있는 명령으로 번역해야 한다. 이 과정을 컴파일 이라고 하며 컴파일은 컴파일러가 수행한다.

# C++ 프로그래밍 첫걸음
- 새로운 프로그래밍 언어를 배우는 첫걸음은 입출력을 포함한 간단한 프로그램을 작성해 보는 것이다

## 선행처리
- C++ 소스 프로그램은 컴파일 되기 전에 먼저 선행처리 과정을 거친다.
- 이 과정은 소스 프로그램을 가공하여 실제로 컴파일러가 번역할 소스 프로그램을 만드는데 선행처리기가 이 작업을 수행한다.
- 선행처리기가 어떤 작업을 할 것인가를 지시하는 명령을 선행처리기 지시어라고 한다.
- 선행처리기 지시어는 '#'으로 시작하며, 한 행에 한 문장씩 작성한다.

### #include
- 헤더 파일을 소스 프로그램에 결합하기
- #include<header> //표준 include 경로에 존재하는 파일
- #include "header" //현재 위치에 존재하는 파일

### #define,#undef
- 매크로 선언 및 해제

### #if,#ifdef,#ifndef
- 조건부 컴파일

### FirstStep.cpp
```c
#include <iostream>
//iostream : C++의 스트림 출력을 위한 사항들을 선언하는 내용을 담고 있는 헤더파일

//std::cout<<value;
// << : 삽입연산자

void main() {
	//표준 출력 스트림으로 문장을 출력함
	std::cout << "나의 첫번째 C++프로그램" << std::endl;
	//cout은 콘솔에 출력하는 기능을 제공

	//endl은 행의 끝(end of line)을 알리는 문자를 출력함으로써
	//다음에 출력하는 내용은 아래 행에 출력되게 한다.

	int a = 10;
	std::cout << "a의 값은 ";
	std::cout << a << "입니다." << std::endl;

}
```

## 네임스페이스
- C++에서 하나의 명칭은 한 번만 정의되어야 한다.
- 그런데 여러 개발자가 작성한 프로그램들을 묶어 하나의 프로그램을 만들 경우 같은 이름을 중복하여 정의할 우려가 있다.
- 변수,함수,구조체,클래스 등의 동일한 이름의 충돌 문제를 C++에서 해결하기 위해 사용되는 영역이다.

### NameSpace.cpp
```c
#include <iostream>

//'{' 와 '}'로 묶인 영역 내에 정의된 이름은 지정된 명칭공간에 속하게 된다.
namespace NameSpace1 { int a = 10; }
namespace NameSpace2 { int a = 20; }
int a = 30;
namespace NameSpace1 { int b = 50; }
//하나의 명칭 공간은 한 번에 모두 완성해야 하는것은 아니다.

void main() {
	int a = 40;

  //명칭공간 내에 정의된 이름을 그 영역의 밖에서 사용할 때는 영역식별 연산자(::)로 소속된 명칭공간을 지정해야 한다.
  //NameSpace1에 속한 a를 사용하려면 10행에서와 같이 NameSpace1::a라고 표기해야 한다.
	std::cout << NameSpace1::a << std::endl;
  //NameSpace2라는 명칭공간을 지정하였으므로 4행에 정의된 a의 값 20을 출력한다.
	std::cout << NameSpace2::a << std::endl;
  //::a와 같이 영역식별 연산자 앞에 명칭공간 이름이 없으면 전역 명칭공간에 정의된 a를 나타내는 것으로, 5행에 정의된 a가 이에 해당된다.
	std::cout << ::a << std::endl;
  //지역변수 a 출력
	std::cout << a << std::endl;
	std::cout << NameSpace1::b << std::endl;

//C++에 제공되는 표준 라이브러리의 이름들은 모두 std('standard'를 줄인 이름)라는 명칭공간에 포함된다.
//그러므로 NameSpace.cpp의 10~14행에서 사용한 cout과 endl이라는 이름에는 'std::'라고 명칭공간을 지정하였다.
}

```
- 특정 명칭공간에 정의된 이름을 자주 사용할 경우 매번 명칭공간을 지정하는 것이 번거로울 수 있다.
- 이러한 경우 다음과 같은 형식으로 using 구문을 사용하면 편리하다.

```c
using namespace namespace-name;//형식은 특정 명칭공간 내의 모든 이름을 명칭공간을 지정하지 않고 사용할 수 있게 한다.

using namespace-name::name; //특정 명칭공간 내의 특정 이름을 명칭공간을 지정하지 않고 사용할 수 있게 한다.
```

```c
using namespace std;
```
또는
```c
using std::cout;

using std::endl;
```
이라는 문장을 넣으면 다음과 같이 간편하게 작성할 수 있다.
```c
cout << NameSpace1::a << endl;

cout << NameSpace2::a << endl;

cout << ::a << endl;

cout << a << endl;

cout << NameSpace1::b << endl;
```

# 기본자료형
- 기본 자료형이란 프로그래밍 언어에서 데이터를 표현하기 위한 기본적인 표현 형식이다.

## 정수형
- short
- int
- long
- long long

## 문자형
- char  1byte

# 변수
- 프로그램이 실행되는 동안 기억하고 있어야 하는 값들을 저장하는 메모리 영역이다.
- 모든 변수는 사용하기 전에 미리 선언하여야 하며, 이때 만든 이름으로 변수의 값을 읽거나 저장한다.

## 변수의 선언
```c
자료형 변수명;

int  length;	// int형 변수 선언

double mean, stddev;	// double형 변수 선언
```

## 변수의 초기화
```c
형식1. TypeName varName = value;

형식2. TypeName varName(value);

형식3. TypeName varName = {value};
```
- 형식3은 C++11 이후의 표준에서 권장하는 형식으로 ‘=’은 생략할 수 있다.
- 형식1이나 형식2가 value의 값을 TypeName 자료형의 값으로 묵시적 형 변환을 하여 초기화하는 반면
- 형식3은 value의 값을 축소 형 변환(정보의 손실이 발생할 수 있는 변환, 즉, 오차나 잘못된 변환이 일어날 가능성이 있는 변환)을 해야 할 경우 오류로 취급한다.
- C에 비해 자료형을 보다 엄격히 취급하는 C++의 특성이 반영된 것이다.

```c
int a = 10;

int b(20);

int c{30};

int d = 1.5;	// d의 값이 1로 초기화됨

int e{1.5};	// 오류 － 축소 변환이 필요함(C++11)(Demotion)
```

## 자료형의 추론
- 변수를 선언할 때는 그 변수가 저장할 값의 자료형을 지정해야 한다.
- C++11의 자료형 추론을 활용하면 변수를 초기화하는 값의 자료형에 맞게 변수를 선언할 수 있다.
- 자료형 추론은 auto라는 키워드를 사용한다.

```c
auto i{ 10 };
```
- 변수 i의 자료형을 지정하지 않았지만 초기화에 사용된 값인 10이 int형이므로 변수 i의 자료형이 int형이라고 추론한다.

# 상수
- 변수의 값이 항상 고정된 값을 갖게 하려면 변수를 선언할 때 const라는 키워드를 사용한다.
- 예를 들어 원주율(π)은 고정된 값으로, 값이 변하지 않는다. 이러한 상수를 다음과 같이 정의할 수 있다.

```c
const  double  PI {3.14159};   // 원주율 정의
```

### CircleArea.cpp
```c
#include <iostream>

using namespace std;

void main() {
	const double PI{ 3.14159 };
	double radius;

	cout << "원의 반경을 입력하시오 : ";
	cin >> radius;//cin은 키보드를 통해 입력하는 기능을 제공한다. cin에 추출 연산자(extractor) '>>'를 사용한다.
	double area = radius * radius * PI;
	cout << "원의 면적= " << area << endl;
}
```

# 연산자
- 연산을 하는 기능이 있는 키워드이다.
- 연산자는 연산의 대상이 되는 피연산자를 사용한다.
- 필요한 피연산자의 개수에 따라 단항, 이항, 삼항 연산자라고 부른다.
- 산술,논리, 관계,대입 등 다양한 형태의 연산을 제공하고 있다.

## 산술연산자
- 숫자 데이터에 가감승제 및 관련 연산을 지시하는 연산자이다.
- +, -, *, / 의 사칙연산과 나머지를 구하는 %연산자가 있다.
```c
#include <iostream>

using namespace std;

void main() {
	int a = 5;
	int b = 3;
	int c;

	c = a + b;
	cout << "a + b = " << c << endl;
	c = a - b;
	cout << "a - b = " << c << endl;
	c = a * b;
	cout << "a * b = " << c << endl;
	c = a / b;
	cout << "a / b = " << c << endl;
	c = a % b;
	cout << "a % b = " << c << endl;
}
```
## 증감연산자
- 변수의 값을 1씩 증가 또는 감소시키는 단항 연산자이다.
- ++, --
### 선행증감연산자
- 변수의 앞에서 사용된다. ++a, --a
### 후행증감연산자
- 변수의 뒤에서 사용된다. a++, a--

```c
#include <iostream>

using namespace std;

void main() {
	int a = 10;

	cout << "++a : " << ++a << endl;
	//11

	cout << "--a : " << --a << endl;
	//10

	int b = 10;

	cout << "b++ : " << b++ << endl;
	//10

	cout << "b-- : " << b-- << endl;
	//11

	cout << "b: " << b << endl;
	//10
}
```

## 논리연산자
- 참－거짓을 판별하는 논리합(||), 논리곱(&&), 부정(!) 연산이 이에 해당된다.
- 논리곱과 논리합은 앞뒤에 논리형 데이터가 오는 이항 연산자이다.

### 논리곱(&&)
- 앞 뒤 피연산자가 모두 참이어야 참을 반환한다.

### 논리합(||)
- 앞 뒤 피연산자중 하나만 참이어도 참을 반환한다.

### 부정(!)
- 연산자 뒤에 피연산자가 하나 오는 단항연산자이다.
- 참을 거짓으로 거짓을 참으로 만든다.

```c
#include <iostream>

using namespace std;

void main() {
	int a = 10;
	int b = 7;

	int result = a > 0 && b > 0;

	// 논리곱(&&)
	cout << "a > 0 && b > 0 : " << result << endl;

	result = a > 0 || b > 0;

	// 논리합(||)
	cout << "a > 0 || b > 0 : " << result << endl;

	// not(!)
	cout << "!result : " << !result << endl;
}
```

## 관계연산자
- 좌측 피연산자를 기준으로 우측의 피연산자와의 비교를 크다(>), 작다(<), 크거나 같다(>=), 작거나 같다(<=), 같다(==), 같지 않다(!=)
- 관계에 대한 참－거짓을 계산한다.

```c
#include <iostream>

using namespace std;

void main() {
	int a = 10;
	int b = 7;
	int result;
	// 크다(>)
	result = a > b;
	cout << "a > b : " << result << endl;

	// 작다(<)
	result = a < b;
	cout << "a < b : " << result << endl;

	// 크거나 같다(>=)
	result = a >= b;
	cout << "a >= b : " << result << endl;

	// 작거나 같다(<=)
	result = a <= b;
	cout << "a <= b : " << result << endl;

	// 같다(==)
	result = a == b;
	cout << "a == b : " << result << endl;

	// 같지 않다(!=)
	result = a != b;
	cout << "a != b : " << result << endl;
	
}
```

## 비트단위 연산자
- 피연산자의 각 비트 단위로 논리합(|), 논리곱(&), 배타적 논리합(^), 부정(~) 연산을 한다.

### 논리곱(&)
- 둘다 1이면 1 , 아니면 0을 반환한다.

### 논리합(|)
- 둘 중 하나만 1이어도 1, 아니면 0을 반환한다.

### 배타적 논리합(^)
- 둘 이 달라야 1, 같으면 0을 반환한다.

### 부정(~)
- 1을 0으로 , 0을 1로 반환한다.

```c
#include <iostream>

using namespace std;

void main() {
	int a = 10;
	int b = 7;
	int result;
	// 논리곱(&)
	result = a & b;
	cout << "a & b : " << result << endl;

	// 논리합(|)
	result = a | b;
	cout << "a | b : " << result << endl;

	// 배타적 논리합(^)
	result = a ^ b;
	cout << "a ^ b : " << result << endl;

	// 부정(~)
	cout << " ~a : " << ~a << endl;	
}
```

## 시프트 연산자
-  좌측 피연산자의 각 비트를 우측 피연산자로 지정된 비트 수만큼 좌(<<) 또는 우(>>)로 이동(shift)한다.

### 좌측 시프트 연산 (<<)
- 좌측 이동 연산을 하는 경우 우측의 비는 비트에는 0이 채워진다.

### 우측 시프트 연산 (>>)
- 우측 이동의 경우 좌측 비트가 비는데 저장된 값이 음이 아닌 값이라면 이 자리에 0이 채워진다.

```c
#include <iostream>

using namespace std;

void main() {
	int a = 7;
	int result = a << 2;
	cout << " a << 2 : " << result << endl;

	result = a >> 2;
	cout << " a >> 2 : " << result << endl;

}
```

## 대입연산자
- 대입 연산자(=)는 우측 피연산자에 해당되는 수식의 값을 좌측 피연산자에 저장한다.

### 복합 대입연산자
- +=, -=, *=, /=, %=, <<=, >>=, &=, |=, ^=
- 좌측과 우측 피연산자를 대상으로 지정된 연산을 한 후 그 결과를 좌측 피연산자에 대입한다.

```c
#include <iostream>

using namespace std;

void main() {
	int b = 7;
	

	cout << "a += 2 : " << (b+=2) << endl;


	cout << "a -= 2 : " << (b -= 2) << endl;

}
```

## 조건(삼항연산자)
- 지정된 조건이 참인가 거짓인가에 따라 값을 선택할 수 있게 한다.
```c
#include <iostream>

using namespace std;

void main() {
	int a = 10;
	int b = 7;

	int result = a > b ? 1 : 0;

	cout << "a > b ? 1 : 0 의 결과 " << result << endl;

}
```

## 연산자 우선순위

|순위| 연산자| 우선순위가 같은 경우에 식을 계산하는 방법|
|----|------|---------------------------------|
|1 |( ) [ ] -> .|	왼쪽에서 오른쪽으로(From left to right)|
|2|sizeof &(주소) ++(전위) - -(전위) ~ ! *(역참조) +(부호) -(부호) 형변환|	오른쪽에서 왼쪽으로(From right to left)|
|3|*(곱셈) / %	|왼쪽에서 오른쪽으로(From left to right)|
|4|+(덧셈) -(뺄셈)	|왼쪽에서 오른쪽으로(From left to right)|
|5|« »	왼쪽에서 |오른쪽으로(From left to right)|
|6|< > <= >=	|왼쪽에서 오른쪽으로(From left to right)|
|7|== !=	|왼쪽에서 오른쪽으로(From left to right)|
|8|&(비트연산)	|왼쪽에서 오른쪽으로(From left to right)|
|9| ^	|왼쪽에서 오른쪽으로(From left to right)|
|10| \|	|왼쪽에서 오른쪽으로(From left to right)|
|11|&&	|왼쪽에서 오른쪽으로(From left to right)|
|12| \|\| |왼쪽에서 오른쪽으로(From left to right)|
|13|?(삼 항 연산자) |오른쪽에서 왼쪽으로(From right to left)|
|14|= += -= *= /= %= &= ^= |= «=  »= | 오른쪽에서 왼쪽으로(From right to left)|
|15|,(Comma) | 왼쪽에서 오른쪽으로(From left to right)|






