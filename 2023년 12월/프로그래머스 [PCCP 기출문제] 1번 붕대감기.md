## 문제 설명
어떤 게임에는 붕대 감기라는 기술이 있습니다.

붕대 감기는 t초 동안 붕대를 감으면서 1초마다 x만큼의 체력을 회복합니다. t초 연속으로 붕대를 감는 데 성공한다면 y만큼의 체력을 추가로 회복합니다. 게임 캐릭터에는 최대 체력이 존재해 현재 체력이 최대 체력보다 커지는 것은 불가능합니다.

기술을 쓰는 도중 몬스터에게 공격을 당하면 기술이 취소되고, 공격을 당하는 순간에는 체력을 회복할 수 없습니다. 몬스터에게 공격당해 기술이 취소당하거나 기술이 끝나면 그 즉시 붕대 감기를 다시 사용하며, 연속 성공 시간이 0으로 초기화됩니다.

몬스터의 공격을 받으면 정해진 피해량만큼 현재 체력이 줄어듭니다. 이때, 현재 체력이 0 이하가 되면 캐릭터가 죽으며 더 이상 체력을 회복할 수 없습니다.

당신은 붕대감기 기술의 정보, 캐릭터가 가진 최대 체력과 몬스터의 공격 패턴이 주어질 때 캐릭터가 끝까지 생존할 수 있는지 궁금합니다.

붕대 감기 기술의 시전 시간, 1초당 회복량, 추가 회복량을 담은 1차원 정수 배열 bandage와 최대 체력을 의미하는 정수 health, 몬스터의 공격 시간과 피해량을 담은 2차원 정수 배열 attacks가 매개변수로 주어집니다. 모든 공격이 끝난 직후 남은 체력을 return 하도록 solution 함수를 완성해 주세요. 만약 몬스터의 공격을 받고 캐릭터의 체력이 0 이하가 되어 죽는다면 -1을 return 해주세요.


## 제한사항
bandage는 [시전 시간, 초당 회복량, 추가 회복량] 형태의 길이가 3인 정수 배열입니다.<br>
1 ≤ 시전 시간 = t ≤ 50<br>
1 ≤ 초당 회복량 = x ≤ 100<br>
1 ≤ 추가 회복량 = y ≤ 100<br>
1 ≤ health ≤ 1,000<br>
1 ≤ attacks의 길이 ≤ 100<br>
attacks[i]는 [공격 시간, 피해량] 형태의 길이가 2인 정수 배열입니다.<br>
attacks는 공격 시간을 기준으로 오름차순 정렬된 상태입니다.<br>
attacks의 공격 시간은 모두 다릅니다.<br>
1 ≤ 공격 시간 ≤ 1,000<br>
1 ≤ 피해량 ≤ 100<br>

## 입출력 예
| bandage	| health | attacks | result|
|-----|-----|-----|-----|
| [5, 1, 5] |	30	| [[2, 10], [9, 15], [10, 5], [11, 5]]	| 5 |

## 입출력 예 설명
입출력 예 #1

몬스터의 마지막 공격은 11초에 이루어집니다. 0초부터 11초까지 캐릭터의 상태는 아래 표와 같습니다.

| 시간 | 현재 체력(변화량) | 연속 성공 | 공격 | 설명 |
|------|-------------------|-----------|------|------|
| 0    | 30                | 0         | X    | 초기 상태 |
| 1    | 30(+0)            | 1         | X    | 최대 체력 이상의 체력을 가질 수 없습니다. |
| 2    | 20(-10)           | 0         | O    | 몬스터의 공격으로 연속 성공이 초기화됩니다. |
| 3    | 21(+1)            | 1         | X    |  |
| 4    | 22(+1)            | 2         | X    |  |
| 5    | 23(+1)            | 3         | X    |  |
| 6    | 24(+1)            | 4         | X    |  |
| 7    | 30(+6)            | 5 → 0     | X    | 5초 연속 성공해 체력을 5만큼 추가 회복하고 연속 성공이 초기화됩니다. |
| 8    | 30(+0)            | 1         | X    | 최대 체력 이상의 체력을 가질 수 없습니다. |
| 9    | 15(-15)           | 0         | O    | 몬스터의 공격으로 연속 성공이 초기화됩니다. |
| 10   | 10(-5)            | 0         | O    | 몬스터의 공격으로 연속 성공이 초기화됩니다. |
| 11   | 5(-5)             | 0         | O    | 몬스터의 마지막 공격입니다. |

## 과정
일단 조건을 다음과 같이 정리해보았다.<br>

(1) 시전 시간동안 1초 마다 초당 회복량(x)만큼 체력 회복 <br>
(2) 시전 시간을 다 채우는 것에 성공하면 추가 회복량(y) 얻음 <br>
(3) 공격 당하면 피해량 만큼 체력 차감<br>
(4) 기술 취소 or 기술이 성공해 끝나면 다시 기술 사용 가능, 이 때 연속 성공 시간 0으로 초기화 <br>
(5) 체력이 0이하가 되면 죽음 -> 이때 -1을 리턴 <br>
(6) 최대 체력이 정해져 있음<br>

위 조건들은 몬스터가 마지막으로 공격하는 시간까지 체크되어야 한다.<br>
다행히 attacks가 공격 시간을 기준으로 오름차순 정렬되어 있기 때문에 벡터의 마지막 원소의 공격 시간을 갖고오면 된다.<br>

조건을 체크하기 위해 필요한 변수를 다음과 같이 생성했다.<br>
- continuity_cnt : 연속 성공 카운트 변수
- max_time : 반복문 수행 시간
- max_health : 최대 체력
- index : 몬스터 공격 시간 지정 변수

```
#include <string>
#include <vector>

using namespace std;

int solution(vector<int> bandage, int health, vector<vector<int>> attacks) {
    int answer = 0;
    int continuity_cnt = 0; // 연속 성공
    int max_time = attacks[attacks.size() - 1][0]; // 수행 시간
    int max_health = health; // 최대 체력
    int index = 0; // 몬스터 공격 정보 지정

    for (int i = 0; i <= max_time; i++) {
        if (attacks[index][0] == i) {
            health -= attacks[index][1];
            if (health <= 0) {
                health = -1;
                break;
            }
            continuity_cnt = 0;
            index++;
        } else {
            continuity_cnt++;
            health += bandage[1];
            if (continuity_cnt == bandage[0]) {
                health += bandage[2];
                continuity_cnt = 0;
            }
            if (health >= max_health) health = max_health;
        }
    }
    answer = health;
    return answer;
}
```

