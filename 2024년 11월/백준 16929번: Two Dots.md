## 문제
Two Dots는 Playdots, Inc.에서 만든 게임이다. 게임의 기초 단계는 크기가 N×M인 게임판 위에서 진행된다.

각각의 칸은 색이 칠해진 공이 하나씩 있다. 이 게임의 핵심은 같은 색으로 이루어진 사이클을 찾는 것이다.

다음은 위의 게임판에서 만들 수 있는 사이클의 예시이다.

	
점 k개 d1, d2, ..., dk로 이루어진 사이클의 정의는 아래와 같다.

- 모든 k개의 점은 서로 다르다. 
- k는 4보다 크거나 같다.
- 모든 점의 색은 같다.
- 모든 1 ≤ i ≤ k-1에 대해서, di와 di+1은 인접하다. 또, dk와 d1도 인접해야 한다. 두 점이 인접하다는 것은 각각의 점이 들어있는 칸이 변을 공유한다는 의미이다.
- 게임판의 상태가 주어졌을 때, 사이클이 존재하는지 아닌지 구해보자.

## 입력
첫째 줄에 게임판의 크기 N, M이 주어진다. 둘째 줄부터 N개의 줄에 게임판의 상태가 주어진다. 게임판은 모두 점으로 가득차 있고, 게임판의 상태는 점의 색을 의미한다. 점의 색은 알파벳 대문자 한 글자이다.

## 출력
사이클이 존재하는 경우에는 "Yes", 없는 경우에는 "No"를 출력한다.

## 제한
2 ≤ N, M ≤ 50
## 예제 입력 1 
```
3 4
AAAA
ABCA
AAAA
```
## 예제 출력 1 
```
Yes
```

## 풀이

같은 색으로 이루어진 사이클을 찾아야 한다

사이클을 찾아야 하기 때문에 DFS를 사용해 경로를 따라가며 시작점으로 돌아오는지 확인했다

이 문제에서는 시작점이 어디든 간에 사이클이 만들어질 수 있으므로 모든 칸에서 DFS를 수행해야 한다

DFS를 돌 때
- 현재 위치
- 시작 위치
- 색깔 정보
- 현재까지의 경로 길이

위 정보를 갖고 조건에 따라 true 또는 false를 반환하게 했다

만약 현재 위치가 시작점과 동일하고, 문제 조건에 따라 길이가 4 이상이면 사이클이 형성된 것이므로 true를 반환한다

만약 탐색하려는 위치가 그래프 내 범위를 벗어나거나 다른 색이면 false이다

이 때 방문 배열인 visited를 새로운 시작점마다 초기화해야 이전 탐색에서는 사이클이 형성되지 않았던 점도 현재 시작점에서는 사이클에 포함되는지를 판단할 수 있다

```C++
#include <iostream>
#include <vector>
#include <algorithm>

#define fastio cin.tie(0), cout.tie(0), ios::sync_with_stdio(0)

using namespace std;


vector<vector<bool>> visited;
vector<vector<char>> board;
const int xPos[4] = { 1,0,0,-1 };
const int yPos[4] = { 0,1,-1,0 };
int n, m;

bool isValid(int x, int y) {
	return x >= 0 && x < n && y >= 0 && y < m;
}

bool DFS(int i, int j, char color, int startI, int startJ, int len ) {
	/*
	
	방문했던 점 기록

	원래 위치로 돌아오는 지 체크 -> 돌아온다면 true
	
	방문하지 않았던 점들 중, 이동 가능한 같은 색깔 점이 있다면 dfs 수행
	
	*/

	visited[i][j] = true;

	for (int k = 0; k < 4; k++) {
		int newX = i + yPos[k];
		int newY = j + xPos[k];

		if (!isValid(newX, newY)) continue;
		if (board[newX][newY] != color) continue;

		if (newX == startI and newY == startJ and len >= 4) {
			return true;
		}

		if (!visited[newX][newY]) {
			if (DFS(newX, newY, color, startI, startJ, len + 1)) {
				return true;
			}
		}
	}
	return false;
}


int main() {

	fastio;
	cin >> n >> m;

	board.resize(n, vector<char>(m,0));
	visited.resize(n, vector<bool>(m, 0));

	for (int i = 0; i < n; i++) {
		for (int j = 0; j < m; j++) {
			cin >> board[i][j];
		}
	}

	for (int i = 0; i < n; i++) {
		for (int j = 0; j < m; j++) {
			char color = board[i][j];	
			fill(visited.begin(), visited.end(), vector<bool>(m, false));
			if (DFS(i, j, color, i, j, 1)) {
				cout << "Yes";
				return 0;
			}
		}
	}
	
	cout << "No";

	return 0;
}

```



