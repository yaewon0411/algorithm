## 문제
N개의 정수로 이루어진 수열에 대해 서로 인접해 있는 두 수의 차가 1에서 N-1까지의 값을 모두 가지면 그 수열을 유쾌한 점퍼(jolly jumper)라고 부른다. 예를 들어 다음과 같은 수열에서 1 4 2 3 앞 뒤에 있는 숫자 차의 절대 값이 각각 3 ,2, 1이므로 이 수열은 유쾌한 점퍼가 
된다. 어떤 수열이 유쾌한 점퍼인지 판단할 수 있는 프로그램을 작성하라.
## ▣ 입력설명
첫 번째 줄에 자연수 N(3<=N<=100)이 주어진다.<br>
그 다음 줄에 N개의 정수가 주어진다. 정수의 크기는 int 형 범위안에 있습니다.
## ▣ 출력설명
유쾌한 점퍼이면 “YES"를 출력하고, 그렇지 않으면 ”NO"를 출력한다.
## ▣ 입력예제 1 
5 
1 4 2 3 7
## ▣ 출력예제 1
YES
## 풀이 과정
인접한 두 수의 차의 절대값이 1~N-1까지 모든 값을 정확히 한 번씩 가져야 한다.<br>
unordered_map을 사용해서 차이 값을 저장하고 조건을 확인할까 했는데 이 문제에서는 굳이 사용할 필요 없는 거 같다.<br>
벡터를 이용해서 유일하게 등장하는 지 확인할 수 있기 때문이다.<br>
반복문을 돌면서 '1 < 절대값 차이 < n'에 들어오는 지, 그리고 유일하게 등장했는 지만 확인해주면 된다.
```C++
#include <iostream>
#include <cstdlib>
#include <vector>

using namespace std;


int main()
{
	int n;
	cin >> n;

	vector<int> nums(n);
	vector<bool> diff(n, false);

	for (int i = 0; i < n; i++) {
		cin >> nums[i];
	}
	for (int i = 0; i < n-1; i++) {
		int absDiff = abs(nums[i] - nums[i + 1]);
		if (absDiff<1 || absDiff>n - 1 || diff[absDiff]) {
			cout << "NO";
			return 0;
		}
		diff[absDiff] = true;
	}
	cout << "YES";
	
	return 0;
}
```
