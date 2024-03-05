# friend함수
- 친구나 동료처럼 수평적 관계의 클래스간의 멤버 변수를 공유해야 할 경우 주로 쓰입니다.
- c++에서 객체의 private멤버에는 해당 객체의 public멤버 함수를 통해서만 접근할 수 있다.
- 하지만 경우에 따라서 해당 객체의 멤버 함수가 아닌 함수도 private멤버에 접근해야만 할 경우가 발생할 수 있다.
- c++에서는 friend라는 새로운 접근 제어 키워드를 제공하여 지정한 대상에 한해 해당 객체의 모든 멤버에 접근할 수 있는 권한을 부여해준다.
- 이러한 friend 키워드는 전역 함수, 클래스, 멤버 함수의 세 가지 형태로 사용할 수 있다.

## friend함수 선언
```c
friend 클래스이름 함수명(매개변수명);
```

### 프렌드함수1.cpp
```c

#include <iostream>
using namespace std;

class A{
    int a = 10;
public:
    //friend함수
    //클래스 안에서 원형을 선언하기는 하지만
    //클래스의 멤버는 아닌 함수
	friend void func(A a);
    static void method();
    void method2(){};
};


//friend함수의 정의는 클래스 밖에서 한다.
void func(A a){
		cout <<"프렌드함수"<<endl;
        cout << a.a<<endl;
}

void A::method(){
    cout <<"static메서드"<<endl;
    //cout << a <<endl;
}

int main(){

    A a;

    a.method2(); //멤버함수 호출

    func(a); //프렌드 함수 호출

    A::method(); //정적 함수 호출

    return 0;

}
```

### friend함수의 사용용도
- 두가지 객체를 비교할 때 사용한다.

### 프렌드함수2.cpp
```c
#include <iostream>
#include <string>

using namespace std;

class Student{
    friend int equal(Student a, Student b);
private:
    int id;
    string name;
public:
    Student(int x, string y): id(x),name(y){ };
};

int equal(Student a, Student b){
    if(a.id == b.id && a.name== b.name){
        return 1;
    } else {
        return 0;
    }
}

int main(){
    Student a(20240001,"james");
    Student b(20240001,"james");
    Student c(20240002,"wade");

    if(equal(a,b) == 1) cout <<"같은 학생입니다."<<endl;
    
    if(equal(a,c) == 0) cout <<"다른 학생입니다."<<endl;

    return 0;

}

```

### 프렌드함수연습1.cpp
```c
#include <iostream>
#include <string>

using namespace std;

//Date클래스
//멤버 : 연,월,일
//생성자에서 매개변수로 멤버 초기화

//compare는 Data의 두 객체를 비교하여 같으면 true
//다르면 false 반환

class Date{
    int year;
    int month;
    int day;

    friend bool compare( Date a, Date b);

public:
    Date(int x, int y,int z) : year(x),month(y),day(z){};
};

bool compare(Date a, Date b){
        if(a.year == b.year && a.month == b.month && a.day == b.day){
            return true;
        } else {
            return false;
        }
    }

int main(){

    Date a(2024,2,23);
    Date b(2024,2,23);
    Date c(2024,2,26);

    cout <<compare(a,b) <<endl;
    cout <<compare(b,c) <<endl;
    return 0;

}
```

### 프렌드함수연습2.cpp
```c
#include <iostream>
#include <string>

using namespace std;

//Rect클래스
//필드 : 가로 세로
//생성자에서 매개변수로 가로,세로 값 초기화하기
//프렌드함수 : add(Rect a, Rect b);

//add() -> 두 사각형의 면적을 더한 값을 출력

class Rect{
private:
    int w = 0;
    int h = 0;
public:
    Rect(int x, int y) : w(x),h(y){}
    friend void add(Rect, Rect);
     
};

void add(Rect a, Rect b){
    cout << a.w * a.h + b.w*b.h <<endl;
}


int main(){

    Rect a(3,5);
    Rect b(5,5);

    add(a,b);

    return 0;

}

```

# 연산자 오버로딩
- 연산자의 오버로딩은 함수의 오버로딩과 거의 차이가 없다.
- 연산자의 기능을 메서드처럼 재정의하는것

### 연산자오버로딩1.cpp
```c

#include <iostream>
#include <string>

using namespace std;

class Rect{
private:
    int w = 0; 
    int h = 0;
public:
    Rect(int x, int y) : w(x),h(y){}
   
   //연산자 오버로딩
   //연산자의 기능을 메서드처럼 정의
   //객체가 피연산자가 될 수 있다.
   double operator+(Rect a){
        int result = w * h + a.w * a.h;
        return (double)result;
   }
     
};

int main(){

    Rect a(3,5);
    Rect b(5,5);

    cout << a+b;

    return 0;

}
```

### 연산자오버로딩연습1
- 위 코드 활용하기
```c
#include <iostream>
#include <string>

using namespace std;

class Rect{
private:
    int w = 0; 
    int h = 0;
public:
    Rect(int x, int y) : w(x),h(y){}
   
   double operator+(Rect a){
        int result = w * h + a.w * a.h;
        return (double)result;
   }

    void print(){
        cout<<"가로 : " << w <<", 세로 : " << h <<endl;
    }

   Rect operator*(int a){
        w = w * a;
        h = h * a;

        return *this;
   }
     
};

int main(){

    Rect a(3,5);
    Rect b(5,5);
   
    cout << a+b <<endl;

    a.print();
    a = a * 2; //* 정수 정수배만큼 가로세로길이를 늘린 객체를 반환
    a.print();

    return 0;

}
```

### 연산자오버로딩연습2
- 위 코드 활용하기
```c
#include <iostream>
#include <string>

using namespace std;

class Rect{
private:
    int w = 0; 
    int h = 0;
public:
    Rect(int x, int y) : w(x),h(y){}

   double operator+(Rect a){
        int result = w * h + a.w * a.h;
        return (double)result;
   }

    // void print(){
    //     cout<<"가로 : " << w <<", 세로 : " << h <<endl;
    // }

    void operator>(int a){
       cout<<"가로 : " << w <<", 세로 : " << h <<endl;
    }

   Rect operator*(int a){
        w = w * a;
        h = h * a;

        return *this;
   }
     
};

int main(){

    Rect a(3,5);
    Rect b(5,5);
   
    cout << a+b <<endl;

    //a.print();
    a > 1; // -> 가로 : X, 세로 : X 출력하기
    a = a * 2; //* 정수 정수배만큼 가로세로길이를 늘린 객체를 반환
    //a.print();
    a > 1;

    return 0;

}
```

### 연산자오버로딩2.cpp
```c
#include <iostream>
#include <string>

using namespace std;

class Num{
private:
    int a;
public:
    Num(int x):a(x){};

    int operator^(int b){
        int temp = 1;
        for(int i = 0 ; i<b ;i++){
            temp = temp * a;
        }
        
        return temp;
    }

    int operator-(int b){
        return a + b;
    }
};


int main(){

    Num b(2);

    //1)b의 3제곱
    int a = b^3;

    cout << a <<endl; //8


    //2)2 + 3;
    a = b - 3; 
    cout << a << endl; //5

    return 0;
}
```

### 연산자오버로딩3.cpp
```c

```