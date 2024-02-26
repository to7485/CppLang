# 상속
- 계층관계를 사용하여 클래스 간의 속성 및 함수를 공유할 수 있도록 지원하는 매우 중요한 개념이다.
## 객체지향의 특징
- 캡슐화 -> 클래스
- 정보은닉 -> 접근지정자
- 상속 -> 이미 작성된 클래스를 확장해서 새로운 클래스를 만든다.

### 상속1.cpp
```c
#include <iostream>
#include <string>
using namespace std;

class Student {
public:
	int id;
	string name;
	int kor, eng, math;
	void print() {
		cout << id << " " << name << endl;
		cout << kor << " " << eng << " " << math << endl;
	}
	
};

//아래의 클래스들을 보면 공통된 부분이 많이 존재하는걸 볼 수 있다.
//공통된 부분이 많다면 매번 클래스를 새로 정의하는것보다
//기존의 클래스를 확장하고, 새로운부분먼 정의를 하는것이 생산성이 좋다.

class MusicStudent {
public:
	int id;
	string name;
	int kor, eng, math;
	int piano, sing;
	void print() {
		cout << id << " " << name << endl;
		cout << kor << " " << eng << " " << math << endl;
	}

};

class ArtStudent {
public:
	int id;
	string name;
	int kor, eng, math;
	int painting, carving;
	void print() {
		cout << id << " " << name << endl;
		cout << kor << " " << eng << " " << math << endl;
	}

};

class SportStudent {
public:
	int id;
	string name;
	int kor, eng, math;
	int running, swimming;
	void print() {
		cout << id << " " << name << endl;
		cout << kor << " " << eng << " " << math << endl;
	}

};

int main() {



	return 0;
}
```

### 위 코드 수정하기
```c
#include <iostream>
#include <string>
using namespace std;

class Student {
public:
	int id;
	string name;
	int kor, eng, math;
	void print() {
		cout << id << " " << name << endl;
		cout << kor << " " << eng << " " << math << endl;
	}
	
};
//상속할 클래스의 모든 자원(필드,메서드)을 가져온다.
//class 클래스명 : public 상속할 클래스{
//
//}
class MusicStudent : public Student {
public:

	int piano, sing;
};

class ArtStudent : public Student {
public:
	int painting, carving;
};

class SportStudent : public Student {
public:
	int running, swimming;
};

int main() {

	SportStudent a;
	a.id = 20240001;
	a.name = "james";
	a.kor = a.eng = a.math = 90;
	a.print();

	a.running = 100;
	a.swimming = 99;


	return 0;
}d
```
- 상속은 클래스의 재활용라고 할 수 있다.

### 상속2.cpp
```c
#include <iostream>
#include <string>
using namespace std;

//부모클래스, 기반(base)클래스, super클래스
class Parent {
public:
	int a = 10;;

	void method() {
		//int a = 20;
		//cout <<"지역변수 a : "<< a << endl;
		//cout <<"멤버변수 a : " << this->a << endl;

		cout << "parent의 메서드" << endl;
	}
};

//자식 클래스, 파생(derived)클래스, sub클래스
class Child : public Parent {
public:
	int a = 30;
	void method2() {
		int a = 20;
		cout << "지역변수 a : " << a << endl;
		cout << "멤버변수 a : " << this->a << endl;
		cout << "부모변수 a : " << __super::a << endl; //super클래스의 멤버변수 a
	}

	//메서드 오버라이딩(중복정의)
	//부모의 메서드를 재정의
	//C++에서 자식 클래스는 상속을 받을 때 명시한 접근 제어 권한에 맞는
	//부모 클래스의 모든 멤버를 상속받는다.
	//이렇게 상속받는 멤버함수는 그대로 사용해도 되고, 필요한 동작을 위해
	//재정의 하여 사용할 수도 있다.
	//오버라이딩이란 멤버함수의 동작만을 재정의하는것으로,
	//함수의 원형은 기존 멤버 함수의 원형과 같아야 한다.
	void method() {
		cout << "child의 method호출" << endl;
	}
};


int main() {

	//Parent a;
	//a.method();

	Child b;
	b.method2();
	cout << "-------------" << endl;
	b.method();


	return 0;
}
```

### 상속3.cpp
```c
#include <iostream>
#include <string>
using namespace std;

class A {
private:
	int a = 10;//자식 클래스는 private멤버는 접근할 수 없다.
protected:
	int b = 20;
	//protected멤버는 다른 곳에서는 접근할 수 없고
	//상속한 자식클래스에서는 접근이 가능하다.
public:
	int c = 30;
};

class B : public A {
public:
	void method() {
		//out << a << endl;
		cout << b << endl;
	}
};

int main() {
	A b;
	//b.a = 10;
	//b.b = 5;
	//b.c


	return 0;
}
```

### 상속4.cpp
```c
#include <iostream>
#include <string>
using namespace std;

class A {
public:
	A() {
		cout << "A클래스의 생성자" << endl;
	}
};

class B :public  A{
public:
	B() {
		A();
		//클래스를 상속할 경우,
		//부모 클래스의 생성자를 자식클래스의 생성자에서 호출함.
	}
};


int main() {

	B a;


	return 0;
}
```

### 상속5.cpp
```c
#include <iostream>
#include <string>
using namespace std;

class A {
public:
	A(int a) {
		cout << "A클래스의 생성자" << endl;
	}
};

class B :public  A{
public:
	B(int a) : A(a) {
		//부모클래스의 생성자가 매개변수를 받는 경우
		//부모클래스의 생성자의 형식에 맞게 자식 클래스에서
		//반드시 초기화 리스트를 이용하여 호출을 해야 한다.
		
	}
};




int main() {

	//B a;
	B a(1);


	return 0;
}
```

### 상속연습1.cpp
```c
#include <iostream>
#include <string>
using namespace std;

//Calculator 클래스
//메서드 : calc(int x, int y); -1 반환

//CalPlus클래스
//Calculator클래스를 상속
//calc()메서드를 오버라이딩 하여
//두 수를 더하여 반환

//CalMinus클래스
//Calculator클래스를 상속
//calc()메서드를 오버라이딩 하여
//두 수를 빼서 반환
class Calculator {
public:
	int calc(int x, int y) {
		return -1;
	}

};

class CalPlus : Calculator {
public:
	int calc(int x, int y) {
		return x + y;
	}
};

class CalMinus : Calculator {
public:
	int calc(int x, int y) {
		return x - y;
	}
};
int main() {

	CalPlus cp;

	cout << cp.calc(2, 3) << endl;

	CalMinus cm;

	cout << cm.calc(2,3) << endl;


	return 0;
}
```






