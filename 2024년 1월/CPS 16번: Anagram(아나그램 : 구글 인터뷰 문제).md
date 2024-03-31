## 문제

Anagram이란 두 문자열이 알파벳의 나열 순서를 다르지만 그 구성이 일치하면 두 단어는 아나그램이라고 합니다.
예를 들면 AbaAeCe 와 baeeACA 는 알파벳을 나열 순서는 다르지만 그 구성을 살펴보면 A(2), a(1), b(1), C(1), e(2)로 알파벳과 그 개수가 모두 일치합니다. 즉 어느 한 단어를 재배열하면 상대편 단어가 될 수 있는 것을 아나그램이라 합니다.
길이가 같은 두 개의 단어가 주어지면 두 단어가 아나그램인지 판별하는 프로그램을 작성하세요. 아나그램 판별시 대소문자가 구분됩니다.

## ▣ **입력설명**
첫 줄에 첫 번째 단어가 입력되고, 두 번째 줄에 두 번째 단어가 입력됩니다.
단어의 길이는 100을 넘지 않습니다.

## ▣ **출력설명**
두 단어가 아나그램이면 “YES"를 출력하고, 아니면 ”NO"를 출력합니다.

## ▣ **입력예제 1**
AbaAeCe
baeeACA

## ▣ **출력예제 1**
YES

## 풀이 과정

1. <br>
두 문자열에 대해서 배열을 돌며 각 문자에 대한 등장 횟수를 저장하고, 다시 반복문을 돌면서 해당 문자 등장 횟수가 같지 않은 경우가 등장하는 지 지켜봤다.
```C++
#include <iostream>
#include <string>
#include <unordered_map>

using namespace std;

unordered_map<char, int> A, B;

int main() {
	bool isAnagram = true;
	string a, b;
	cin >> a >> b;
	for (int i = 0; i < a.length(); i++) {
		A[a[i]]++;
		B[b[i]]++;
	}
	for (int i = 0; i < a.length(); i ++) {
		if (A[a[i]] != B[a[i]]) {
			isAnagram = false;
			break;
		}
	}
	if (isAnagram) cout << "YES";
	else cout << "NO";

	return 0;
}
```
2. 두 번째 문자열을 순회하며 map B를 채우는 것보다 첫 번째 문자열을 순회하며 A를 채울 때, 바로 두 번째 문자열의 해당 문자를 감소시키는 것이 더 괜찮은 방법인 거 같아서 다르게 풀어봤다.<br>
그리고 알파벳만 등장하니까 고정 크기 배열로 가져가는 게 더 효율적인 것 같다. <br>
cnt라는 카운트 배열을 하나 생성하고, a의 각 문자에 대해서는 증가 시킨다. 반대로 b의 각 문자에 대해서는 감소 시킨다. 이렇게 되면,<br>
두 문자가 아나그램일 시, cnt 배열의 모든 값이 0이 되므로 쉽게 판별 가능하다.
```C++
#include <iostream>
#include <string>

using namespace std;

int main()
{
	string a, b;
	cin >> a >> b;

	int cnt[128] = { 0 }; //아스키 문자만 처리한다면 크기를 128로 잡을 수 있음

	for (int i = 0; i < a.length(); i++) {
		cnt[a[i]]++;
		cnt[b[i]]--;
	}
	for (int i = 0; i < 128; i++) {
		if (cnt[i] != 0) {
			cout << "NO";
			return 0;
		}
	}
	cout << "YES";

	return 0;
}
```

