# 백준 17485 진우의 달 여행 (Large) 

[문제 링크](https://www.acmicpc.net/problem/17485)

## 1. 설계 로직

1. N, M, map 입력 받습니다.
2. dp용으로 사용할 3차원 배열을 만듭니다.
   - 방향 정보가 들어가 있으므로 3차원으로 만듭니다.
   - 초기 값은 최대 값으로 채워줍니다.
     - `dp[y][x][0]`: `dp[y-1][x+1]`의 최솟 값 + `map[y][x]`
     - `dp[y][x][1]`: `dp[y-1][x]`의 최솟 값 + `map[y][x]`
     - `dp[y][x][2]`: `dp[y-1][x-1]`의 최솟 값 + `map[y][x]`
3. dp에서 y == 0인 경우, map 값을 초기 값으로 채워줍니다.
4. 각각의 경우에 따라 dp를 진행합니다. (0 < y < N)
   - x == 0 인 경우: ↘로 오는 방향이 존재하지 않음
   - x == M-1인 경우: ↙로 오는 방향이 존재하지 않음

4. dp를 모두 채우고 나서, N-1번째 행에  존재하는 값 중 최솟값을 구합니다.

![image-20211209163939976](README.assets/image-20211209163939976.png)

- 시간복잡도 : O(3NM) => O(NM)

## 2. 코드

```python
N, M = map(int, input().split())
board = [list(map(int, input().split())) for _ in range(N)]
MAX_VAL = 100 * 1000 + 1
dp = [[[MAX_VAL]*3 for _ in range(M)] for _ in range(N)]

for y in range(N):
    if y == 0:
        for x in range(M):
            for d in range(3):
                dp[y][x][d] = board[y][x]
    else:
        for x in range(M):
            if x == 0:
                dp[y][x][0] = min(dp[y-1][x+1][1], dp[y-1][x+1][2]) + board[y][x]
                dp[y][x][1] = dp[y-1][x][0] + board[y][x]
            elif x == M-1:
                dp[y][x][1] = dp[y-1][x][2] + board[y][x]
                dp[y][x][2] = min(dp[y-1][x-1][0], dp[y-1][x-1][1]) + board[y][x]
            else:
                dp[y][x][0] = min(dp[y-1][x+1][1], dp[y-1][x+1][2]) + board[y][x]
                dp[y][x][1] = min(dp[y-1][x][0], dp[y-1][x][2]) + board[y][x]
                dp[y][x][2] = min(dp[y-1][x-1][0], dp[y-1][x-1][1]) + board[y][x]

answer = 1e9
for x in range(M):
    answer = min(min(dp[N-1][x]), answer)

print(answer)
```

## 3. 후기

dp문제지만 처음에 완탐+가지치기로 풀었다가 시간초과 났습니다.

N, M이 1000까지 가능이라서 가지치기 하더라도 완탐으로 풀기에는 컸던 걸로 ..