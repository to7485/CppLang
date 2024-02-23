# Random클래스
- 컴퓨터는 난수를 생성할 때 첫번째 수만 무작위로 정한다.
- 나머지 수들은 그 수를 기반으로 여러가지 수학적 방법을 통해 마치 난수처럼 보이지만 실제로는 무작위로 생성된 것이 아닌 수열들을 만들어내게 된다.
- 무작위로 정해진 첫번째 수를 시드(seed)라고 부르는데, C의 경우 srand를 통히 seed를 설정할 수 있다.
- 난수를 발생시키는 클래스

### 랜덤클래스.cpp
```c
#include <iostream>
#include <random>
using namespace std;

int main(){
	//시드값을 얻기 위한 random_device 객체 생성
	random_device a;

	//random_device를 통해 난수 생성 엔진을 초기화 한다.
	mt19937 gen(a());

	//1부터3까지 균등하게 나타나는 난수열을 생성하기 위해 균등분포 정의
	uniform_int_distribution<int>(1,3);

	for(int i = 0 ; i < 5; i;;){
		cout <<"난수 : " << dis(gen) << endl;
	}


	return 0;
}

```

### 랜덤클래스연습1.cpp
```c
//랜덤클래스를 사용하여
//1 ~ 45 사이의 정수 6개를 랜덤으로 출력

#include <iostream>
#include <random>
using namespace std;

int main(){
	random_device a;
	uniform_int_distribution<int>dis(1,45);
	uniform_int_distribution<double>dis(1.0,2.0);

	for(int i =0; i< 6; i++){
		cout <<dis(a) <<" "<<endl;
	}
}
```

### 랜덤클래스연습2.cpp
```c
//"red", "blue", "green"중 랜덤으로 10회 출력하시오.

#include <iostream>
#include <string>
#include <random>

using namespace std;

random_device a;
uniform_int_distribution<int>dis(0,2);

int main(){

    string color[3] = {"red","blue","green"};

    for(int i = 0 ; i< 10; i++){
        cout << color[dis(a)] <<endl;
    }
    
    return 0;
}

```

### 랜덤클래스연습3.cpp
```c
//최소 10명의 이름을 입력받고
//청소 당번을 랜덤으로 2명 뽑으세요

//이름 >> 홍길동
//이름 >> 박길동
// ....
//홍길동 박길동 ... 십길동

//청소당번1 : 김길동
//청소당번2 : 홍길동

#include <iostream>
#include <string>
#include <random>

using namespace std;

random_device a;
uniform_int_distribution<int>dis(0,9);

int main(){

    string* name = new string[10];
    int count = 0;
    for(int i = 0 ; i <10; i++){
        cout<<"이름 >> ";
        cin >> name[i];
        if(name[i] == "end")break;
        count++;
    }

    random_device rd;
    uniform_int_distribution<int>(0,count-1);

    for(int i = 0; i < count; i++){
        cout <<name[i] <<" ";
    }
    cout <<endl;

	//중복으로 나오지 않게 해보자.
    string a = name[dis(rd)];
    string b = name[dis(rd)];
    while(true){
        
        if(b == a) b = name[dis(rd)];
        else break;
    }

    cout <<"청소당번1 : "<< name[dis(rd)] <<endl;
    cout <<"청소당번2 : "<< name[dis(rd)] <<endl;

    
    return 0;
}
```

### 랜덤객체생성.cpp
```c
//Dog클래스
//필드 : 품종(string), 나이(int)
//품종 { 포메, 말티즈, 치와와,퍼그, 푸들}
//나이(개월) 3~9개월 중 랜덤
//품종과 개월 출력

//Dog클래스의 객체를 5개 생성하시오
//(실행예)
//시츄 5개월
//시츄 3개월
//치와와 7개월
//퍼그 9개월
//포메 4개월

#include <iostream>
#include <string>
#include <random>

using namespace std;

class Dog{
    public:
        string arr[5] = {"푸들","포메","퍼그","치와와","말티즈"};
        string name;
        int age;

        Dog(){
            random_device a;
            uniform_int_distribution<int>dis1(0,4);
            uniform_int_distribution<int>dis2(3,9);
            name = arr[dis1(a)];
            age = dis2(a);
            cout << name <<" " <<age<<"개월"<<endl;
        }
};

int main(){

    Dog arr[5];

    return 0;
}
```

# 탬플릿(template)
- 템플릿이란 어떠한 물건을 찍어내는 틀이라고 생각하면 된다.
- 함수나 클래스를 개별적으로 다시 작성하지 않아도, 여러 자료 형으로 사용할 수 있도록 하게 만들어 놓은 틀.
- 템플릿은 보관할 자료형을 인수로 하여 컨테이너 클래스나 함수를 생성할 수 있게 함으로써 개별적 자료형 각각에 대해 프로그램을 중복적으로 작성하는 노력을 줄일 수 있게 한다. 

## 템플릿 정의
```c
template <typename T>
```
- 템플릿은 template라는 예약어로 정의한다.
- typename을 T라는 이름으로 아래에 위치한 함수를 템플릿으로 정의하겠다는 의미
- T는 일반적인 이름이고 다른 이름을 사용해도 된다.

## 템플릿함수
- c++에서는 같은 이름의 함수를 반환값이나 매개변수의 타입만 바꿔 하나의 함수로 다양한 타입의 처리를 할 수 있는 오버로딩이라는 개념이 있다.
- 하지만 오버로딩은 필요한 자료형이 하나씩 늘어날때마다 일일히 새로 만들어야 하는 비효율적인 부분이 있다.
- 하지만 템플릿을 사용한다면 모든 자료형에 유연하게 적용할 수 있다.

```c
template <typename T>
T add(T x, T y){
  return x + y;
}
```

### 템플릿함수1.cpp
```c
#include <iostream>
#include <string>

using namespace std;

template< typename T>

T get_max(T a, T b){
    if(a > b) return a;
    else return b;
}

// int get_max(int a, int b){
//     if(a > b) return a;
//     else return b;
// }

// float get_max(float a, float b){
//     if(a > b) return a;
//     else return b;
// }

// double get_max(double a, double b){
//     if(a > b) return a;
//     else return b;
// }

// char get_max(char a, char b){
//     if(a > b) return a;
//     else return b;
// }

//각각의 함수의 다른 부분은 반환형과 매개변수뿐이다.

int main(){
    cout << get_max(1,10) <<endl;
    cout << get_max(1.0f,10.0f) <<endl;
    cout << get_max(1.0,10.0) <<endl;
    cout << get_max('a','b') <<endl;

    return 0;
}
```

## 템플릿클래스
- 타입매개변수로 멤버의 타입을 정하는 클래스
- 함수 템플릿과 같은 원리로 클래스에도 템플릿을 적용할 수 있다.
- 다만 클래스는 생성시 타입을 명시해줘야 한다.

### 템플릿클래스1.cpp
```c
#include <iostream>
#include <string>

using namespace std;

//template< typename T>
template

class Ta{
    public:
        T a;
        int b;
};

int main(){
   
   //필드에 들어갈 타입을 객체를 생성하며 정해준다.
    Ta<int> a;
    a.a = 10;
    Ta<double> b;
    b.a = 10.5;
    Ta<string> c;
    c.a = "apple";

    return 0;
}
```