## 문제 설명
AI 엔지니어인 현식이는 데이터를 분석하는 작업을 진행하고 있습니다. 데이터는 ["코드 번호(code)", "제조일(date)", "최대 수량(maximum)", "현재 수량(remain)"]으로 구성되어 있으며 현식이는 이 데이터들 중 조건을 만족하는 데이터만 뽑아서 정렬하려 합니다.
<br>
예를 들어 다음과 같이 데이터가 주어진다면
```
data = [[1, 20300104, 100, 80], [2, 20300804, 847, 37], [3, 20300401, 10, 8]]
```
이 데이터는 다음 표처럼 나타낼 수 있습니다.

| code |	date |	maximum |	remain |
|:-----:|:-----:|:-----:|:------:|
|1	| 20300104	| 100	| 80 |
|2	| 20300804	| 847	| 37 |
|3	| 20300401	| 10	| 8 |

주어진 데이터 중 "제조일이 20300501 이전인 물건들을 현재 수량이 적은 순서"로 정렬해야 한다면 조건에 맞게 가공된 데이터는 다음과 같습니다.<br>
```
data = [[3,20300401,10,8],[1,20300104,100,80]]
```
정렬한 데이터들이 담긴 이차원 정수 리스트 data와 어떤 정보를 기준으로 데이터를 뽑아낼지를 의미하는 문자열 ext, 뽑아낼 정보의 기준값을 나타내는 정수 val_ext, 정보를 정렬할 기준이 되는 문자열 sort_by가 주어집니다.

data에서 ext 값이 val_ext보다 작은 데이터만 뽑은 후, sort_by에 해당하는 값을 기준으로 오름차순으로 정렬하여 return 하도록 solution 함수를 완성해 주세요. 단, 조건을 만족하는 데이터는 항상 한 개 이상 존재합니다.

## 풀이 과정

조건에 맞는 경우들을 필터링하면 된다<br>
제일 마지막 조건인 오름차순 정렬을 할 때 문제가 있었다<br>
처음 코드는 이렇다
```
  //문자열 기준으로 오름차순 정렬
  sort(answer.begin(), answer.end(), [sortIndex](vector<int>& a, vector<int>& b) {
      if(a[sortIndex] < b[sortIndex]) return true;
  });
```
기본 테스트 케이스는 통과하는데 전체 테스트에는 자꾸 시간 초과가 걸려서 다시 코드를 보니까 반환하는 부분에서 문제가 있었다.<br>
if문 조건이 거짓인 경우 아무런 값도 반환하지 않으니까 undefined로 빠지는 것이었다<br>
C++에서는 모든 실행 경로에서 반환 값이 명시되어야 한다

그래서 
```
sort(answer.begin(), answer.end(), [sortIndex](vector<int>& a, vector<int>& b) {
    return a[sortIndex] < b[sortIndex];
});
```
이렇게 a[sortIndex] < b[sortIndex]의 결과를 직접 반환하도록 수정했다

```C++
vector<vector<int>> solution(vector<vector<int>> data, string ext, int val_ext, string sort_by) {
    vector<vector<int>> answer;
    int index = 0, sortIndex = 0;

    if (ext == "date") index = 1;
    else if (ext == "maximum") index = 2;
    else if (ext == "remain") index = 3;

    //기준값보다 작은 데이터 뽑기
    for (int i = 0; i < data.size(); i++) {
        if (data[i][index] < val_ext) answer.push_back(data[i]);
    }

    if (sort_by == "date") sortIndex = 1;
    else if (sort_by == "maximum") sortIndex = 2;
    else if (sort_by == "remain") sortIndex = 3;

    //문자열 기준으로 오름차순 정렬
    sort(answer.begin(), answer.end(), [sortIndex](vector<int>& a, vector<int>& b) {
        return a[sortIndex] < b[sortIndex];
    });


    return answer;
}
```

