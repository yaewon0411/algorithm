## 문제
N명의 학생의 수학점수가 입력되면 각 학생의 석차를 입력된 순서대로 출력하는 프로그램을 
작성하세요.
## ▣ 입력설명
첫 줄에 N(1<=N<=100)이 입력되고, 두 번째 줄에 수학점수를 의미하는 N개의 정수가 입력된다. 같은 점수가 입력될 경우 높은 석차로 동일 처리한다. 즉 가장 높은 점수가 92점인데 92점이 3명 존재하면 1등이 3명이고 그 다음 학생은 4등이 된다. 점수는 100점 만점이다.
## ▣ 출력설명
첫 줄에 입력된 순서대로 석차를 출력한다.
## ▣ 입력예제 1 
5<br>
90 85 92 95 90
## ▣ 출력예제 1
3 5 2 1 3
## 풀이 과정

학생의 점수랑, 그 학생 인덱스를 저장해서 정렬 후에도 원래 입력 순서대로 출력할 수 있게 한다.<br>
점수를 내림차순으로 정렬했다가, 석차를 구한 후 다시 인덱스 기준으로 오름차순 정렬한다.<br>
인덱스 순서대로 출력하는 건 다시 정렬 안해도 반복문으로 구할 수 있다.<br>

<인덱스, 점수> -> <인덱스, 석차> -> 인덱스 오름차순 정렬 후 출력<br>

이렇게 된다


```C++
#include <iostream>
#include <vector>
#include <algorithm>

using namespace std;

bool desc(pair<int, int>& a, pair<int, int>& b) {
	return a.second > b.second;
}
bool asc(pair<int, int>& a, pair<int, int>& b) {
	return a.first < b.first;
}

int main()
{
	int n;
	cin >> n;
	vector<pair<int, int>> v(n);
	for (int i = 0; i < n; i++) {
		cin >> v[i].second;
		v[i].first = i;
	}
	sort(v.begin(), v.end(), desc);

	int MIN = (*v.begin()).second, grade = 1, innerCnt = 0;
	v[0].second = 1;

	for (int i = 1; i < n; i++) {
		if (MIN == v[i].second) {
			v[i].second = grade;
			innerCnt++;
		}
		else if (MIN > v[i].second) {
			grade++;
			grade += innerCnt;
			MIN = v[i].second;
			v[i].second = grade;
			innerCnt = 0;
		}
	}
	sort(v.begin(), v.end(), asc);
	for (auto i : v) cout << i.second << " ";
	return 0;
}
```
