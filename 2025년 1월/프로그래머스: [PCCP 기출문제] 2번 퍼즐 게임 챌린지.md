https://school.programmers.co.kr/learn/courses/30/lessons/340212?language=cpp


## 풀이

level = 1부터 완전 탐색하게 되면 퍼즐 300,000개에 난이도가 최대 100,000인 경우에<br>
완전 탐색 = 300,000 * 100,000 = 30,000,000,000번 동안 찾게 돼서 이분 탐색으로 찾아야 한다



이분 탐색 종료 조건은 다음과 같다<br>
만약 diff = {1, 2, 3, 4, 5}에 대해 <br>
가능 여부 = {X, X, O, O, O} 인 경우라면

1) left = 1, right = 5<br>
이 경우 mid = 3으로 level = 3일 때 충분히 limit 내에 풀 수 있으므로 그 이하 구간 탐색을 위해 right = mid가 된다
2) left = 1, right = 3<br>
이 경우 mid = 2로 level = 2일 때 limit 내에 들어오지 못하므로 그 다음 레벨에서의 구간 탐색을  left = mid+1이 된다
3) left = 3, right = 3<br>
이 경우 mid = 3으로 level = 3일 때 충분히 limit 내에 풀 수 있으므로 그 이하 구간 탐색을 위해 right = mid가 되고, left >= right인 상태로 이미 가능한 구간 내 탐색을 모두 한 상태이므로 최소 레벨인 left이 답이 된다

```C++
bool canSolve(vector<int>& diffs, vector<int>& times, long long limit, int level, long long basicTime) {
    long long totalTime = basicTime;

    for (int i = 1; i < diffs.size(); i++) {
        if (diffs[i] > level) {
            int failure = diffs[i] - level;
            totalTime += failure * (times[i] + times[i - 1]);
        }
        if (totalTime > limit) {
            return false;
        }
    }
    return true;
}

int solution(vector<int> diffs, vector<int> times, long long limit) {
    int left = 1;
    int right = *max_element(diffs.begin(), diffs.end());
    long long basicTime = 0;
    for(const auto &time: times){
        basicTime += time;
    }

    while (left < right) {
        int mid = left + (right - left) / 2;
        if (canSolve(diffs, times, limit, mid, basicTime)) {
            right = mid;
        }
        else {
            left = mid+1;
        }
    }
    return left;
}
```
