# 4. Computation

### Computation
- **divide and conquer** (divide는 function을 통해 가능)
- **abstraction** (안전성 높아짐, 실수의 가능성 적음, 유지보수 쉬워짐)
<br> => 전역변수는 모든 곳에서 접근가능하므로 위험. 사용 권장하지 않음.
 <br>
=> member function으로 사용 권장 (main함수는 global function임) <br>
(java의 경우 전역변수 X , main함수도 멤버function으로 들어감)


### Selection
조건문

### Iteration (for loop)
- 변수 scope에 대한 example1

~~~cpp
for(int i=0;i<10;i++){
  int j=0;
  j++;
  cout << i << endl;
}

cout << i << endl;
~~~

~~~
오류: i를 찾을 수 없음
~~~

- 변수 scope에 대한 example2

~~~cpp
int i;
for(i=0;i<10<i++){
  int j=0;
  j++;
//  cout << i << endl;
}

cout << i << endl;
~~~
~~~
 10

~~~

=> local 변수의 범위에 영향

### Functions
call by value : 값만 복사 <br>
call by reference : 공간주소넘겨줌

** reference 변수 는 기존의 메모리 공간을 같이 사용 **

~~~cpp
int max( int &a, int &b){   //const int &a로 쓰면 바꿀 수 없음. 안전하게
  int x;
  x=b+30;

  if(a<b) return x;
  else return x;
}
int main(){

  int a = 10;
		int b = 10;
	//	int c = max(a, 20);// error. 상수의 메모리 공간은 x

		int &d = b; //c++의 경우 reference타입 지원함
		int * e = &b; //포인터도 지원함
		int f = b; //값만 복사한 것

		cout << d << * e  << f << endl;  //10 10 10
		b = 100;
		cout << d << * e << f << endl;  // 100 100 10
		d = 200;
		cout << b << * e << f << endl;  // 200 200 10
}

~~~

** 변수 d, e, f의 차이 이해할 것 **

- d는 할당받은 메모리 공간 없음 (b 것을 같이 씀). 데이터 안정성에 문제 생김

- e는 포인터 변수로 자신의 메모리 공간이 할당되어 있음. 주소값만 복사한 것. 마찬가지로 데이터 안정성에 문제<br>

- f는 값만 복사한 것.

<br>

function call 하면 주소값 복사해야해서 효율성 떨어짐 => reference타입을 파라미터로 사용

* const를 사용하여 안전성 확보할 수 있음

~~~cpp
int max(const int &a, int &b){  
   //함수내에서 a 바꿀 수 없음. 안전하게 
}
~~~

* 만약 리턴타입이 reference타입이라면?

~~~cpp
int& max( int &a, int &b){  
  int x;
  x=b+30;

  if(a<b) return x;
  else return x;
}
~~~
=> x는 함수 종료되면 사라지므로 error
~~~cpp
int& max( int &a, int &b){  
  int x;
  x=b+30;

  if(a<b) return b;
  else return a;
}
~~~
=> 가능
~~~cpp
int& max( int a, int b){  
  int x;
  x=b+30;

  if(a<b) return b;
  else return a;
}
~~~
=> a,b 는 함수의 parameter일뿐.

### Vector

- array: element가 연속적으로 존재. random access 가능함. 메모리 사용이 flexible 하지 않음
- linked list: 아무 공간에 존재. randomly, directly access가 불가. 처음부터 따라가야 함. 메모리 사용이 dynamic

=> Vector라는 자료구조는 이 둘의 장단점을 혼합함 <br>
메모리 공간 연속적으로 directly 접근가능하며, 사용하는 메모리 사이즈가 동적임

내부적인 구조는 array임 <br>
insert시 doubling 2->4->8->16... 으로 더블링 횟수는 log2 N
***
vector을 이용한 중복 단어 제거 예제
~~~cpp
#include <iostream>
#include <string>
#include <vector>
#include <algorithm>

using namespace std;

int main() {
	vector<string> words;
	for (string s; cin >> s && s != "quit";) {
		words.push_back(s);

	}
	sort(words.begin(), words.end());
	vector<string>w2;
	if (0 < words.size()) {

		w2.push_back(words[0]);
		for (int i = 1; i < words.size(); ++i) {
			if (words[i - 1] != words[i])
				w2.push_back(words[i]);
		}
	}

	cout << "found" << words.size() - w2.size() << " duplicates" << endl;
	for (string s : w2)
		cout << s << endl;

	return 0;
}
~~~

***
가위바위보 게임 (랜덤함수를 이용하지 않을 것, 벡터 사용하기)
~~~cpp
#include <iostream>
#include <string>
#include <vector>

using namespace std;

void initialize(vector<string> &mylist) {	//값을 바꿔줘야 하므로 call by reference

	mylist.push_back("rock");
	mylist.push_back("paper");
	mylist.push_back("rock");
	mylist.push_back("scissors");
	mylist.push_back("paper");
	mylist.push_back("scissors");
	mylist.push_back("rock");
	mylist.push_back("paper");
	mylist.push_back("rock");
	mylist.push_back("scissors");


}
int getWin(const string &computer, const string &user) {	//함수내에서 값 바꾸지 않을 것이므로 const
	int win;
	if (user != "rock" && user != "paper" && user != "scissors")
		win = -100;
	else if (user == computer)
		win = 0;
	else if ((computer == "rock" && user == "scissors") || (computer == "scissors" && user == "paper") || (computer == "paper" && user == "rock"))
		win = 1;
	else
		win = -1;

	return win;
}

string getNext(const vector<string> &mylist, const string &user) { //값 안바꿀 것

	static int cur = 0; //로컬변수로 쓸 수 없음(전역변수는 쓰지말것) , static을 쓰면 한번 메모리 잡힌 후 반환하지 않고 계속 사용

	cout << cur << endl;

	if (user == "rock")
		cur += 2;
	else if (user == "scissors")
		cur += 3;
	else if (user == "paper")
		cur += 4;
	else
		cur++;

	return mylist[(cur % mylist.size())];
}

int main() {

	vector<string> mylist;

	initialize(mylist);

	string user;
	string computer;
	int win;


	for (cin >> user; user != "quit"; cin >> user) {
		computer = getNext(mylist, user);

		cout << computer << endl;
		cout << user << endl;
		win = getWin(computer, user);

	;

		switch (win) {
		case -1:
			cout << "You win" << computer << "," << user << endl;
			break;
		case 1:
			cout << "I win" << computer << "," << user << endl;
			break;
		case 0:
			cout << "We win" << computer << "," << user << endl;
			break;
		default:
			cout << "wrong Input" << endl;
		}

	}
}
~~~
