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

class CalPlus : public Calculator {
public:
	int calc(int x, int y) {
		return x + y;
	}
};

class CalMinus : public Calculator {
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
## 다중 상속
- 파생 클래스는 1개 이상의 기초 클래스를 가질 수 있다.
- 2개 이상의 기초 클래스를 상속받는 것을 다중상속(multiple inheritance)이라고 한다.
- java나 c#과 같은 다른 객체지향언어에서는 다중상속을 허용하지 않는다.

- c++에서는 쉼표(,)를 사용하여 상속받을 여러 개의 기초 클래스를 명시하는 것으로 간단히 다중 상속을 구현할 수 있다.
```c
class 파생클래스이름 : 접근제어지시자 기초클래스이름, 접근제어지시자 기초클래스이름, 접근제어지시자 기초클래스이름, ...

```

### 상속6.cpp
```c
#include <iostream>
using namespace std;

class A {
public:
	int a;
};

class B {
public:
	int a;
	int b;

};

class C : public A, public B {

};


int main() {

	C a;
	//다중상속을 할 때 멤버의 이름이 겹치게 되면
	//어느 클래스에서 온 멤버인지 네임스페이스로 명시해야 한다.
	a.A::a = 10;
	a.B::a = 15;
	a.b = 20;

	cout << a.A::a << endl;
	cout << a.B::a << endl;
	cout << a.b << endl;

	return 0;
}
```

# 다형성
- 부모클래스의 포인터변수로 sub클래스들을 지정

### 다형성1.cpp
```c
 #include <iostream>
#include <string>
using namespace std;

class Shape {
public:
	//가상함수 -> virtual 수식어가 붙은 함수
	virtual void draw() {
		cout << "Shape Draw" << endl;
	}
};

class Circle : public Shape {
public:
	void draw() {
		cout << "Circle Draw" << endl;
	}
};

class Rectangle : public Shape {
public:
	void draw() {
	cout << "Rectangle Draw" << endl;
	}
};

int main() {

	Shape* p = new Circle();
	//((Circle*)p)->draw();
	p->draw();

	Shape* p2 = new Rectangle();

	//((Rectangle*)p2)->draw();
	p2->draw();

	return 0;
}

```

## 가상함수
- 파생클래스에서 재정의할것으로 기대되는 멤버함수
- 자식 클래스에서 가상함수를 재정의하면 이전에 정의되었던 내용들은 모두 새롭게 정의된 내용들로 교체된다.
- 부모 클래스에서 virtual 키워드를 사용해 가상 함수를 선언하면 자식 클래스에서 재정의된 멤버함수도 자동으로 가상함수가 된다.

### 가상함수 호출방식
- c++에서는 가상 함수가 아닌 일반 멤버 함수들의 호출은 컴파일할 때 고정된 메모리 주소로 변환된다 -> 정적바인딩
- 가상 함수를 호출할 때는 컴파일러가 어떤 함수를 호출해야 하는지 미리 알 수 없다.
- 가상함수는 프로그램이 실행될 때 객체를 결정하므로 컴파일 시점에 해당 객체를 특정할 수 없기 때문이다 -> 동적바인딩

### 다형성연습1.cpp
```c
#include <iostream>
#include <string>
using namespace std;

class Animal {
public:
	virtual void speak() {};
};
//Cat, Dog, Cow클래스 -> Animal클래스 상속

class Cat :public Animal {
public:
	void speak() { cout << "야옹" << endl; }
};
class Dog :public Animal {
public:
	void speak() { cout << "멍멍" << endl; }
};
class Cow :public Animal {
public:
	void speak() { cout << "음메" << endl; }
};

int main() {

	Animal* a = new Cat();
	Animal* a = new Dog();
	Animal* a = new Cow();

	a->speak(); //야옹
	b->speak(); //멍멍
	c->speak(); //음메

	return 0;
}

```

### 다형성3.cpp
```c
#include <iostream>

using namespace std;

//추상(abstract)클래스
class A {
public:
	//순수가상함수 -> body가 없는 가상함수(프로토타입 X)
	virtual void func() = 0;
};

class B : public A{
public:
	//순수 가상함수를 재정의해야 한다.
	void func() { cout << "B의 func()" << endl; };
};

int main() {

	//A a; 추상클래스는 객체를 생성할 수 없음

	//추상클래스를 상속한 클래스의 생성자로 객체를 생성
	A* a = new B();
	a->func();


	return 0;
}

```

### 다형성연습2.cpp
```c
#include <iostream>
#include <string>
using namespace std;

//추상클래스 Animal 생성

class Animal {
public:
	virtual void move() = 0;
	virtual void eat() = 0;
	virtual void speak() = 0;
};

class Lion : public Animal {
public:
	void move() { cout << "달린다" << endl; };
	void eat() { cout << "고기" << endl; };
	void speak() { cout << "으르렁" << endl; };
};

int main() {

	Animal* a = new Lion();
	a->move();// "달린다"출력
	a->eat();// "고기"출력
	a->speak();//"으르렁"출력

	Lion c;
	Animal& b = c;
	b.move();
	b.eat();
	b.speak();


	return 0;
}

```


