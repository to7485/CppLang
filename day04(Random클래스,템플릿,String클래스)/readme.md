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

### 템플릿함수2.cpp
```c
#include <iostream>
#include <string>

using namespace std;

//템플릿함수 max1, min1
template<typename T>

T max1(T x, T y, T z){
    int max = x;
    if(max < y) max = b;
    if(max < z) max = z;

    return max;
};

template<typename T>

T min1(T x, T y, T z){
    int min = x;
    if(min > y) max = y;
    if(min > z) max = z;

    return min;
}



int main(){

    cout << max1(1,3,5) <<endl;
    cout << max1(1.1,3.5,5.5) <<endl;
    cout << max1('a','b','c') <<endl;

    cout << min1(1,3,5) <<endl;
    cout << min1(1.1,3.5,5.5) <<endl;
    cout << min1('a','b','c') <<endl;

    return 0;
}
```

### 템플릿클래스2.cpp
```c
#include <iostream>
#include <string>

using namespace std;

template<typename T>

class Box{
    T data;
    public:
        void set(T value);
        T get();
};

//템플릿 클래스의 함수를 클래스 외부에서 정의할 경우
//템플릿 함수임을 명시해야 한다.
template<typename T>

void Box<T>::set(T value){
    data = value;
}

T Box<T>::get(){
    return data;
}

int main(){

    Box<int> a;
    a.set(10);
    cout <<a.get() <<endl;

    Box<double> b;
    b.set(10.5);
    cout <<b.get()<<endl;
    return 0;

    Box<string>c;
    c.set("홍길동");
    cout << c.get() <<endl;
}
```

### 템플릿클래스3.cpp
```c
#include <iostream>
#include <string>

using namespace std;

template<typename T1, typename T2>

class Box2{
    T1 first;
    T2 second;
    public:
        T1 get_first();
        T2 get_second();
        void set_first(T1 value){
            first = value;
        }

        void set_second(T1 value){
            second = value
        }
}
template<typename T1,typename T2>
T1 Box2<T1,T2>::get_first(){
    return first;
};

template<typename T1,typename T2>
T2 Box2<T1,T2>::get_second(){
    return second;
}

int main(){
    Box2<int,double> a;
    a.set_first(10);
    a.set_second(3.14);

    cout <<a.get_first()<<endl;
    cout <<a.get_second()<<endl;
    return 0;
}
```


### stach.h
```c
//스택 템플릿 클래스
//배열, top, 스택크기
//push(), pop(), isempty(), isfull(), print();
//초기화함수느 생성자로 대신

#include <iostream>
#include <string>
using namespace std;

template<typename T>
class Stack{
        //어떤 요소가 들어올지 모르기 때문에 T
    T arr[100];
    int top;
    int size;
public:
    Stack(){
        top = -1;
        size = 100;
    }

    void push(T value){
        if(isFull() == ture) return;
        top++;
        arr[top] = value;
    }

    //요소룰 지우고 return하기 때문에 T
    T pop(){
        if(isEmpty() == ture) return (T)0;
        T removeitem = arr[top];
        top--;
        return removeitem;
    }

    bool isEmpty(){
        if(top == -1) return true;
        else false;
        
    }

    bool isFull(){
        if(top == size-1)return true;
        else return false;
    }

    void print(){
        for(int i = 0; i <= top; i++){
            cout <<arr[i] <<" ";
        }
        cout <<"[top: " << top <<"]"<<endl;
    }


};

```

### stack.cpp
```c
#include "stack.h"

int main(){

    // Stack<int> st1;
    // st1.push(10);st1.push(20);st1.push(30);
    // st1.print();
    // cout <<st1.pop()<<endl;
    // st1.print();

    //역순 문자열 만들기
    string s = "apple";
    //s를 한글자씩 stack에 넣구 push()
    //stack에 pop을 해서 문자열을 생성

    Stack<char> st1;
    string rev="";
    for(int i = 0; i <s.length(); i++){
        st1.push(s[i]);
    }
    bool b = true;
    for(int i = 0; i < s.length(); i++){
        rev = rev + st1.pop();
    }

    cout << s <<endl;
    cout << rev <<endl;

    return 0;
}
```

### 회문검사.cpp
```c
#include "test.h"

int main(){
    Stack<char> st1;
    string ori = "level";

    //스택 객체 생성
    //문자열 ori에서 처믕부터 한글자씩 모두 push()
    //문자열 ori에서 첫글자부터 한글자씩 스택에서 pop()한것과 비교
    //끝까지 다른것이 없으면 "회문입니다."
    //하나라도 다른것이 있다면 "회문이 아닙니다."
    string rev = "";
    for(int i = 0 ; i <ori.length(); i++){
        st1.push(ori[i]);
    }

    bool b = true;
    for(int i = 0 ; i < ori.length(); i++){
        if(ori[i] != st1.pop()){
            b = false;
        }
    }

    if(b == true){
        cout << ori <<"는 회문입니다."<<endl;
    } else{
        cout << ori <<"는 회문입니다."<<endl;

    }
    return 0;
}
```

# string클래스
- c++ STL(Standard Template Library)에서 제공하는 클래스로, 말 그대로 string(문자열)을 다루는 클래스이다.
- C에서는 char* 또는 char[]형태로 문자열을 다뤘다면 C++에서는 문자열을 하나의 변수 type처럼 사용하며, 문자열을 훨씬 다양하고 쉽게 다룰 수 있게 해준다.
- char*, char[]과 다르게 문자열의 끝에 '\0'문자가 들어가지 않으며, 문자열의 길이를 동적으로 변경 가능하다.

### string클래스.cpp
```c
#include <iostream>
#include <string>

using namespace std;

//c타입의 문자열은 '\0'(null)문자가 포함된다.
//string타입의 문자열에는 포함되지 않는다.

int main(){
    string str;
    //입력
    //cin >> str;

    //getline(입력스트림, 저장할 문자열)
    //getline(cin, str);

    //출력
    //cout << str <<endl;

    //string 객체 생성방법
    str = "apple"; // = 로 생성
    string str1("apple"); // 생성자로 생성

    char arr[] = {'a','b','c'};

    string str2(arr); //char형 배열로 생성

    string* str3 = new string("apple");//동적할당으로 생성



    return 0;
}

```

### string클래스2.cpp
```c
#include <iostream>
#include <string>

using namespace std;


int main(){
    //string클래스의 연산자
    //< > == 사전적 순서로 비교
    // + 문자열 연결

    // string s1 = "apple";
    // string s2 = "banana";

    // cout <<(s1 < s2) <<endl;
    // cout <<(s1 == "apple") <<endl;
    // cout <<(s1 > s2) <<endl;
    // cout <<(s1 + s2) <<endl;

    //string클래스의 멤버함수
    string s = "apple";

    //유효한 글자가 있는지 체크하지 않음, at보다 접근이 빠름
    cout <<s[0]<<endl;

    //유효한 글자가 있는지 체크함 -> 범위를 벗어나면 예외 발생
    cout <<s.at(0)<<endl;

    //첫글자 리턴
    cout <<s.front() <<endl;

    //마지막 글자 리턴
    cout <<s.back() <<endl;

    //문자열의 길이 반환
    cout <<s.length() <<endl;

    //문자열 길이 반환
    cout <<s.size() <<endl;

    //문자열이 사용중인 메모리 크기 반환
    cout<<s.capacity() <<endl;

    //빈문자열이면 1, 아니면 0을 반환
    cout << s.empty() <<endl;

    s = "banana";
    
    s.erase(2,2);

    //erase(n,m) n번 인덱스로부터 m개의 글자를 삭제
    cout << s << endl;

    //제일 뒤에 ()안의 문자를 추가
    s.push_back('n');

    cout <<s <<endl;

    //제일 뒤의 한글자를 지움
    s.pop_back();
    cout << s <<endl;

    //()안의 문자열의 위치를 리턴
    //찾는 문자열이 없으면 -1를 return
    cout << s.find("an")<<endl;

    //find(찾을 문자열, 시작 index);

    //1번 인덱스부터 끝까지 리턴
    cout <<s.substr(1) << endl;

    //부분 문자열 리턴
    cout <<s.substr(2,2) <<endl;

    return 0;
}
```

### string클래스연습1.cpp
```c
#include <iostream>
#include <string>
using namespace std;

int main() {

	string s = "banana";

	//replace(시작인덱스,글자수,치환할 문자열)
	//s.replace(2, 2, "");
	//cout << s;

	string exp = "123+45";
	cout << "수식>>";
	cin >> exp;
	int result = 0;

	//수식을 입력하면 연산이 되는 코드 작성하기
	//(실행예)
	//수식 >> 111-22(엔터)
	//89

	if (exp.find("+") != -1) {
		int index = exp.find("+");
		int x = stoi(exp.substr(0, index));
		int y = stoi(exp.substr(index+1, exp.length() - 1));

		result = x + y;

	}



	//(실행예)
	//수식>> 11-222(엔터)
	// -211

	if (exp.find("-") != -1) {
		int index = exp.find("-");
		int x = stoi(exp.substr(0, index));
		int y = stoi(exp.substr(index + 1, exp.length() - 1));

		result = x - y;

	}

	cout << "결과 : " << result << endl;
	

	return 0;
}
```

### string클래스연습2.cpp
```c
#include <iostream>
#include <string>
using namespace std;

int main() {

	string address;

	//(실행예)
	//주소 >> 서울시 서대문구 창천동(엔터)
	//시 : 인천시
	//구 : 부평구
	//동 : 작전동

	cout << "주소 >>";
	getline(cin, address);

	int f1 = address.find(" ");
	int f2 = address.find(" ", f1 + 1);

	string s1 = address.substr(0, f1);
	string s2 = address.substr(f1+1, f2 - f1 - 1);
	string s3 = address.substr(f2 + 1, address.length() - f2 - 1);

	cout << "시: " << s1 <<endl;
	cout << "구: " << s2 << endl;
	cout << "동: " << s3 << endl;


	return 0;
}

```