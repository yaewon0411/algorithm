### 문제 설명
각 칸마다 색이 칠해진 2차원 격자 보드판이 있습니다.<br>그중 한 칸을 골랐을 때, 위, 아래, 왼쪽, 오른쪽 칸 중 같은 색깔로 칠해진 칸의 개수를 구하려고 합니다.

보드의 각 칸에 칠해진 색깔 이름이 담긴 이차원 문자열 리스트 board와 고른 칸의 위치를 나타내는 두 정수 h, w가 주어질 때 board[h][w]와 이웃한 칸들 중 같은 색으로 칠해져 있는 칸의 개수를 return 하도록 solution 함수를 완성해 주세요.

### 제한사항
1 ≤ board의 길이 ≤ 7
board의 길이와 board[n]의 길이는 동일합니다.<br>
0 ≤ h, w < board의 길이
1 ≤ board[h][w]의 길이 ≤ 10
board[h][w]는 영어 소문자로만 이루어져 있습니다.

### 입출력 예
board	h	w	result<br>
[["blue", "red", "orange", "red"], ["red", "red", "blue", "orange"], ["blue", "orange", "red", "red"], ["orange", "orange", "red", "blue"]]	1	1	2


### 과정
배열 범위 내에서 조건을 확인하기만 하면 된다.<br>
(1) 2차원 배열 내에 인덱스가 들어오는지<br>
(2) 같은 색깔로 칠해졌는지<br><br>
위 두 가지 조건을 확인해 개수를 카운트한다.



```
#include <string>
#include <vector>

using namespace std;

int h_d[4] = {0,1,-1,0};
int w_d[4] = {1,0,0,-1};
int solution(vector<vector<string>> board, int h, int w) {
	int len = board.size();
	int answer = 0;
	int count = 0;
	for (int i = 0; i < 4; i++) {
		int h_check = h + h_d[i];
		int w_check = w + w_d[i];
		if ((h_check < len && h_check >= 0) && (w_check<len && w_check>=0)) {
			if (board[h_check][w_check] == board[h][w]) {
				count++;
			}
		}
	}
	answer = count;
	return answer;
}
```
