
시간 제한: 1 초 <br>
정점 N개의 트리에서 두 사람이 게임을 진행하려 한다.<br>
각 정점은 1번부터 N번 까지 번호가 매겨져 있고 루트노드는 1번 노드이다.<br>
게임은 서로 턴을 번갈아 가며 진행되고 트리 위에 놓을 수 있는 말과 함께 진행된다.<br>
두 사람의 점수는 모두 0점으로 시작한다.<br>

각 턴마다 두 사람은 다음과 같은 작업을 반복한다.<br>

현재 말이 놓여 있는 정점의 번호만큼 자신의 점수에 더한다.<br>
현재 말이 놓여 있는 정점의 자식 정점이 없다면 그대로 게임을 종료한다.<br>
자식 정점이 존재한다면 자식 정점 중 원하는 자식 정점으로 말을 옮긴다.<br>
게임이 종료되었을 때 선공의 점수가 후공의 점수보다 높거나 같다면 선공이 승리하고 아니라면 후공이 승리한다.<br>
두 사람이 최적으로 플레이할 때, 처음 말이 놓여져 있는 정점의 번호에 따라 선공이 이기는지 후공이 이기는지 구해보자.<br>


## 입력
첫째 줄에 정점의 수 N이 주어진다.<br>
```
1≤N≤100000
```
둘째 줄부터 N−1개의 줄에 간선을 나타내는 정수 u,v가 주어진다.<br>
```
1≤u,v≤N
```
이는 u번 정점과 v번 정점을 잇는 간선이 존재한다는 뜻이다.
## 출력
N개의 줄에 걸쳐 정답을 출력한다.<br>
i번째 줄에는 말의 시작위치가 i번 정점일 때의 결과를 출력한다.<br>
선공이 이긴다면 1을 후공이 이긴다면 0을 출력한다.<br>
## 입력 예시 1
```
5
1 3
2 1
3 4
5 1
```
## 출력 예시 1
```
1
1
0
1
1
```
## 풀이 과정
요약하면
```
1) 현재 말이 놓여 있는 정점의 번호만큼 자신의 점수에 더하기
2-1) 현재 말이 놓여 있는 정점의 자식 정점이 없다면 그대로 게임 종료.
2-2) 현재 말이 놓여 있는 정점의 자식 정점이 있다면 원하는 자식 정점으로 이동

선공 점수 >= 후공 점수 => 선공이 승리
선공 점수 < 후공 점수 => 후공이 승리

최적으로 플레이 할 떄, 처음 말이 놓여져 있는 정점 번호 따라 누가 이기는 지 구함
선공이 이기면 1, 후공이 이기면 0을 출력
```
이렇게 된다<br>
문제 이해가 쉽지 않아서 예제를 사용해 과정을 살펴봤다
```
<정점 4에서 시작하는 경우>

1) 선공 : 0점
- 현재 말이 놓여 있는 정점 4 -> 0+4해서 현재 4점
- 자식 정점 없음. 게임 종료

결과 : 선공(4), 후공(0) -> 선공이 승리


<정점 3에서 시작하는 경우>
1) 선공 : 0점
- 현재 말이 놓여 있는 정점 3 -> 0+3해서 현재 3점
- 자식 정점 4로 이동
2) 후공 : 0점
- 현재 말이 놓여 있는 정점 4 -> 0+4해서 4점
- 자식 정점 없음. 게임 종료

결과 : 선공(3), 후공(4) -> 후공이 승리


<정점 1에서 시작하는 경우>
- 1->2
1) 선공 : 0점
- 현재 말이 놓여 있는 정점 1 -> 0+1해서 현재 1점
- 자식 정점 2로 이동
2) 후공 : 0점
- 현재 말이 놓여 있는 정점 2 -> 0+2 해서 현재 2점
- 자식 정점 없음. 게임 종료

결과 : 선공(1), 후공(2) -> 후공이 승리. 최적의 게임이 아님

- 1->3->4
1) 선공 : 0점
- 현재 말이 놓여 있는 정점 1 -> 0+1해서 현재 1점
- 자식 정점 3으로 이동
2) 후공 : 0점
- 현재 말이 놓여 있는 정점 3 ->0+3해서 현재 3점
- 자식 정점 4로 이동
3) 선공 : 1점
- 현재 말이 놓여 있는 정점 4 -> 1+4해서 현재 5점
- 자식 정점 없음. 게임 종료

결과 : 선공(5), 후공(3) -> 선공이 승리. 최적의 게임
```
리프 노드에서 게임을 시작하면, 선공이 리프 노드의 번호를 점수로 갖게 되며 반드시 승리하게 된다<br>
그리고 '현재노드점수 = 현재노드번호 - 자식점수' 이렇게 계산이 되는데, 이 때 자식 노드로 이동했을 때 얻을 수 있는 점수들 중에 최소값을 골라야 한다. <br>
그래야 최적으로 진행하는 경우가 되며, 선공 입장에서 후공이 선택할 수 있는 노드로 인한 최악의 경우를 고려할 수 있기 때문이다<br>
이렇게 함으로써 선공이 자식 노드의 점수 중 최소값을 현재 노드 점수에서 빼는 것으로, 후공이 최적으로 플레이할 때 선공이 받게 되는 최소 점수를 계산할 수 있게 된다<br>

```C++
#include <iostream>
#include <vector>
#include <climits>

#define fastio ios::sync_with_stdio(0), cin.tie(0), cout.tie(0)

using namespace std;

vector<vector<int>> board;
vector<int> match;
vector<bool> visited;

void dfs(int node) {

    if (visited[node]) return;

    visited[node] = true;
    bool isLeaf = true;
    int toSub = INT_MAX;

    for (int i = 0; i < board[node].size(); i++) {
        int next = board[node][i];
        
        if (!visited[next]) {
            dfs(next);
            isLeaf = false;
            toSub = min(toSub, match[next]);
        }
    }

    if (isLeaf) match[node] = node;
    else match[node] = node - toSub;
}

int main() {


    fastio;
    cin >> n;
    
    board.resize(n+1);
    match.resize(n + 1, 0);
    visited.resize(n + 1, false);

    for (int i = 1; i < n; i++) {
        int a, b;
        cin >> a >> b;
        board[a].push_back(b);
        board[b].push_back(a);
    }

    dfs(1);

    for (int i = 1; i <= n; i++) {
        if (match[i] >= 0) cout << "1";
        else cout << "0";
        cout << "\n";
    }
    return 0;
}
```




