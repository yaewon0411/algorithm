## 문제 설명
탄소(C)와 수소(H)로만 이루어진 화합물을 탄화수소라고 합니다.<br>
탄소(C) 한 개의 질량은 12g, 수소(H) 한 개의 질량은 1g입니다.<br>
에틸렌(C2H4)의 질량은 12*2+1*4=28g입니다.<br>
메탄(CH4)의 질량은 12*1+1*4=16g입니다.<br>
탄화수소식이 주어지면 해당 화합물의 질량을 구하는 프로그램을 작성하세요.

## ▣ 입력설명
첫 줄에 탄화수소식이 주어집니다. 식의 형태는 CaHb 형태이며 (1<=a, b<=100)이다.
단 a 나 b 가 1이면 숫자가 식에 입력되지 않는다. 예) CH4

## ▣ 출력설명
첫 줄에 탄화수소의 질량을 출력합니다.

## ▣ 입력예제 1
C2H4

## ▣ 출력예제 1
28
## 풀이 과정
처음에 문제를 제대로 안읽어서 a와 b가 두 자리 수 이상일 때를 간과하도록 짰다..ㅠ

일단 입력 받은 문자열에서 특정 문자(C, H)를 만나면, <br>
그 이후의 문자가 숫자이고 문자열의 끝이 아닌지를 계속 체크하면서 함량 개수를 구해준다
isdigit()는 문자가 아스키 숫자이면 참을 반환하고 아니면 거짓을 반환한다. 이를 사용해서 조건을 검사했다
```C++
#include <iostream>
#include <string>
#include <cctype>

using namespace std;

int extractNumber(string compound, int idx) {
	if (idx >= compound.length() || !isdigit(compound[idx])) return 1;

	int number = 0;
	while (idx < compound.length() && isdigit(compound[idx])) {
		number = number * 10 + (compound[idx] - '0');
		idx++;
	}
	return number;
}


int main() {

	string compound;
	cin >> compound;

	int cCnt = 0, hCnt = 0;
	int i = 0;

	while(i < compound.length()) {
		if (compound[i] == 'C') {
			cCnt = extractNumber(compound, ++i);
		}
		else if (compound[i] == 'H') {
			hCnt = extractNumber(compound, ++i);
		}
	}

	int mass = 12 * cCnt + hCnt;
	cout << mass;

	return 0;
}
```





