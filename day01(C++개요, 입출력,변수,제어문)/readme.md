# 프로그래밍 언어
- Algol 60 -> A언어
- Bcpl -> B언어에서 코드블럭({})이 생김
- B 언어 -> 데니스리치, 켄톰슨
- C 언어(1970년대) -> 데니스리치
# C++ 언어
- C++는 1970년대 후반에 AT&T 벨 연구소의 비야네 스트롭스트룹이 C언어를 확장해서 만든 객체지향 프로그래밍 언어이다.
- 처음에는 C with class였는데 후에 이름을 C++로 바꿨다.
- 따라서 C가 가지고 있는 모든 기능이 C++안에 포함되어 있고, 몇 가지 새로운 내용이 C++에 추가되었다.
- C++는 C를 기반으로 개발된 언어이므로 몇 가지를 예외를 제외하고는 C의 문법을 그대로 따른다.
- 그러므로  C처럼 범용 프로그래밍 언어로서 다양한 응용프로그램을 개발하는데 활용될 수 있고, 시스템 프로그래밍을 위한 언어로도 활용할 수 있다.

# 통합개발환경(IDE)
- 프로그래머가 소프트웨어 코드를 효율적으로 개발하도록 돕는 소프트웨어 애플리케이션이다.
- 소프트웨어 편집, 빌드, 테스트, 패키징과 같은 기능을 사용하기 쉬운 하나의 애플리케이션에 통합하여 개발자 생산성을 높인다.
- 이클립스, 인텔리제이, Xcode, VisualStudio 등 다양한 종류의 IDE가 존재한다.

## VisualStudio 설치하기
1. 브라우저를 열고 Visual Studio를 검색한다.

![image](img/vs1.png)

2. 스크롤을 조금 내려서 설치파일을 받는다.(커뮤니티 버전을 다운받는다.)

![image](img/vs2.png)

3. 설치할 때 c++을 이용한 데스크탑 개발을 체크한 후 설치한다.

![image](img/vs3.png)

4. 로그인을 하라고 하는건 건너뛰기를 해주면 된다.

![image](img/vs4.png)

# 프로젝트 생성하기

![image](img/vs5.png)

![image](img/vs6.png)

![image](img/vs7.png)

# 소스 파일의 작성

![image](img/vs8.png)

- 소스파일의 이름을 작성하고 ok를 누른다.

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


### C언어 기반이기 때문에 C의 문법이 가능하다.
### hello.cpp
```c
#include <stdio.h>

void main() {

	printf("hello c world!!\n");
}
```

```c
// C라이브러리를 include 할때는
// 헤더파일명 앞에 c를 붙이고 .h확장자는 생략
#include <cstdio>//#include <stdio.h>
#include <iostream> //입출력에 관련된 C++라이브러리
void main() {

	printf("hello c world!!\n");
	std::cout << "hello C++ world!!!\n";
}
```

### FirstStep.cpp
```c++
#include <iostream>
//iostream : C++의 스트림 출력을 위한 사항들을 선언하는 내용을 담고 있는 헤더파일



void main() {
	
	표준 출력 스트림으로 문장을 출력함
	std::cout << "나의 첫번째 C++프로그램" << std::endl;
	//cout -> 표준 출력 스트림
	//cout은 콘솔에 출력하는 기능을 제공

	//std::cout<<value;
	//std:: -> namespace
	// << -> 스트림연산자

	//endl은 행의 끝(end of line)을 알리는 문자를 출력함으로써
	//다음에 출력하는 내용은 아래 행에 출력되게 한다.

	int a = 10;
	std::cout << "a의 값은 ";
	std::cout << a << "입니다." << std::endl;

}
```

### information.cpp
```c
//실행 예
//제임스
//35세
//인천시 부평구
//취미는 축구
//좋아하는 음식은 피자

#include <iostream> //입출력에 관련된 C++라이브러리
void main() {

	std::cout << "홍길동"<< endl;
	std::cout << 35 << "세" <<endl;
	std::cout << "인천시 부평구" <<endl;
	std::cout << "취미는 축구" <<endl;
	std::cout << "좋아하는 음식은 피자" <<endl;

}

//namespace를 매번 써줘야 하는게 귀찮다.
using namespace std; //추가시 std namespace만 생략 가늗

//using
//namespace를 사용하겠다.
//namespace std에 포함된 모든것을 사용하겠다
//라는 의미

#include <iostream> //입출력에 관련된 C++라이브러리
void main() {

	cout << "홍길동"<< "\n";
	cout << 35 << "세" <<"\n";
	cout << "인천시 부평구" << "\n";
	cout << "취미는 축구" << "\n";
	cout << "좋아하는 음식은 피자" << "\n";

}
```

## 네임스페이스
- C++에서 하나의 명칭은 한 번만 정의되어야 한다.
- 그런데 여러 개발자가 작성한 프로그램들을 묶어 하나의 프로그램을 만들 경우 같은 이름을 중복하여 정의할 우려가 있다.
- 변수,함수,구조체,클래스 등의 동일한 이름의 충돌 문제를 C++에서 해결하기 위해 사용되는 영역이다.

### a.h
```c
#include <stdio.h>

void print();
```

### b.h
```c
#include <stdio.h>

void print();
```

### a.c
```c
#include "a.h"

void print() {
	printf("a.h의 print\n");
}
```

### b.c
```c
#include "b.h"

void print() {
	printf("a.h의 print\n");
}
```

### test.cpp
```c
#include "a.h"
#include "b.h"

//namespace
//include한 두개이상의 헤더파일에 동일한 이름의 함수가 있을때
//둘을 구분하기 위한 도구 장치

void main() {

	print();

}
```
### NameSpace.cpp
```c++
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

## 논리형
- bool

## 문자열
- string 클래스

### 자료형.cpp
```c
#include<iostream>

//c++의 string클래스
#include<string>
//c에서은 string 헤더파일
#include <cstring>//#include <string.h>

using namespace std;

void main() {
	char a;
	short b;
	int c;
	long d;
	float e;
	double f;

	//c언어에서는 참은 1 거짓은 0이었다.
	//c++에서는  bool이라는 새로운 키워드가 생겼다.
	bool g = true; //참 -> true, 거짓 -> false

	//string 클래스를 사용하려면 라이브러리를 import해줘야 한다.
	//문자열을 저장하는 string클래스 타입
	string s = "apple";
}
```

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

### 변수.cpp

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

# 표준입력
- 표준입력장치를 통해 값을 읽어와 화면에 출력을 하는것

### 표준입력1.cpp
```c
#include <iostream>
#include <string>
using namespace std;

void main() {

	int a;

	//cin -> 표준입력 스트림
	//>> -> 추출연산자
	cin >> a;

	cout << a << endl;

	string s;
	cin >> s;
	cout << s << endl;
}
```

### 표준입력연습2.cpp

```c
실행예
이름 >> james(엔터)
나이>> 21(엔터)
신장>> 175.8(엔터)
혈액형>> AB(엔터)

james 21세 AB형 175.8cm

#include <iostream>
#include <string>
using namespace std;

void main() {

	int age;
	string name, bloodType;
	double height;

	cout << "이름>>";
	cin >> name;

	cout << "나이>>";
	cin >> age;

	cout << "신장>>";
	cin >> height;

	cout << "혈액형>>";
	cin >> bloodType;

	cout << name << " " << age << "세 " << bloodType << "형 " << height << "cm";
	
}

```

### 표준입력연습2.cpp
```c
#include <iostream>
#include<string>
using namespace std;
void main() {

	int time ,hour, minute, second;

	cout << "초>>"; cin >> time;
	second = time % 60;
	time = time / 60;

	minute = time % 60;
	time = time / 60;

	hour = time;

	//to_string(정수 또는 실수) : 숫자 -> 문자열, 123 -> "123"
	//string타입의 문자열을 + 연산자로 결합 가능함 ex) "a" +"bc" -> "abc"
	string s = to_string(hour)+"시간 "+ to_string(minute)+"분 "+to_string(second)+"초";

	cout << s << endl;
}
```

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

# 형변환
- 한 자료형에서 다른 자료형으로 변환하는 것.

## 묵시적 형변환
- 자료형에는 형 변환 관련 순위가 있다.
- 묵시적 형변환은 수식에 여러 자료형의 값들이 포함되어 있을 경우 가능한 변환이 자동적으로 적용된다.
- 묵시적 형 변환을 할 때에는 순위가 낮은 자료형의 값이 순위가 높은 자료형의 값으로 변환된다.
```c
bool < char < short < int < long < long long

float < double < long double
```
## 명시적 형변환
- 프로그래머가 명시적으로 자료형 변환을 지정하여 주는 것을 말한다.
- '(자료형)데이터' 로 형변환을 할 수 있다.

# 제어문
- 프로그램의 흐름을 제어할 수 있는 문법

## 조건문
- 지정된 조건에 따라 실행 흐름을 제어하는 문장이다.
- if문과 switch문이 있다.
### if문
- if문장은 조건의 참, 거짓에 따라 문장을 선택적으로 실행할 수 있도록 하는 구문이다.
```c
if (조건)

    문장1;    // 조건이 참일 때 실행할 문장

else

    문장2;    // 조건이 거짓일 때 실행할 문장
```

### switch문
- 만일 정수 자료형에 해당되는 수식의 값에 따라 해당되는 처리를 하고자 할 때 는 switch문을 사용하면 편리하다.
```c
switch (정수형_수식) {

    case 값1 :
        문장1;    // 정수형 수식의 값이 값1일 때 실행할 문장들을 나열
        break;   // swtich문을 빠져 나가게 함

    case 값2 :
		문장2;    // 정수형 수식의 값이 값2일 때 실행할 문장들을 나열
		break;   // swtich문을 빠져 나가게 함

  ……

    default :    // 정수형 수식의 값과 일치하는 case 값이 없을 때
		문장n;    // 실행할 문장들을 나열
		break;
}
```

## 반복문
- 반복문은 일정 범위의 문장을 반복하여 실행하고자 할 때 사용하는 구문
- for문과 while문이 있다.

### for문
- for문의 기본 형식은 다음과 같다.
```c
for (초기화 ; 반복조건 ; 증감)
    문장;
```

### 반복문연습문제1.cpp
```c
배열에 정수 5개를 입력받아서
모두 출력하고 최대값과 최소값을 출력
(실행 예)
정수입력 >> 2 5 3 7 1

#include <iostream>
#include<string>
using namespace std;

int main(){
    int arr[5] = {0};

    cout << "정수입력>> ";
    for(int i = 0; i < 5; i++){
        cin >> arr[i];
    }

    int max,min;

    max = min = arr[0];

    for(int i = 1; i < 5; i++){
        if(max < arr[i]) max = arr[i];
        if(min > arr[i]) min = arr[i];
    }

    string s = "[ ";
    for(int i = 0; i < 5; i++){
       s = s + to_string(arr[i]) + " ";
    }
    s = s +"]";
    cout << s << endl;
    cout <<"max="<<max<<", min="<<min<<endl;

    return 0;
}
```

### 위 코드에서 오름차순으로 정렬하기
```c
#include <iostream>
#include <string>
#include <algorithm>
using namespace std;

int main(){
    int arr[5] = {0};

    cout << "정수입력>> ";
    for(int i = 0; i < 5; i++){
        cin >> arr[i];
    }

    //sort(배열의 시작주소, 배열의 마지막 주소)
    sort(arr,arr+5);

    for(int i = 0; i <5; i++){
        cout <<arr[i] <<" ";
    }

    return 0;
}
```

### 범위기반 for문
- 배열과 같이 여러 원소로 구성된 데이터 집합에 대해 첫 원소부터 마지막 원소까지 반복하여 실행하도록 지시하는 것
```c
#include <iostream>

using namespace std;

void main() {
	int arr[10] = { 1,2,3,4,5,6,7,8,9,10 };
	int sum = 0;
	for (int a : arr) {
		sum += a;
	}
	cout << "합계 = " << sum << endl;

}
```
