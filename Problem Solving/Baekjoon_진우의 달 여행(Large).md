## [진우의 달 여행(Large)]

- 진우의 달 여행(Small)에 비해 N, M의 크기가 늘어났다
- 단순히 DFS로 푼다면 O(N * 3^N)으로 시간초과가 난다
- 가지치기를 하더라도 유의미한 결과를 낼 수 없다
- 이전 값이 이후의 값에 영향을 주므로 *DP*로 풀자
- DP 시간 복잡도는 O(N * M)으로 풀이 가능
- https://www.acmicpc.net/problem/17485

---

```java

package baekjoon;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Baekjoon_진우의달여행2 {

    // dp 로 풀어보자
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());

        int[][] map = new int[N][M];
        for (int i = 0; i < N; i++) {
            st = new StringTokenizer(br.readLine());
            for (int j = 0; j < M; j++) {
                map[i][j] = Integer.parseInt(st.nextToken());
            }
        }

        int[][][] dp = new int[N][M][3]; // 이전에 3 방향에서 올 수 있다 (좌, 동, 우)
        for (int i = 0; i < M; i++) {
            for (int j = 0; j < 3; j++) {
                dp[0][i][j] = map[0][i];
            }
        }
        for (int i = 1; i < N; i++) {
            for (int j = 0; j < M; j++) {
                for (int k = 0; k < 3; k++) {
                    dp[i][j][k] = 100000000;
                }
            }
        }

        int[] dc = { -1, 0, 1 };

        for (int i = 1; i < N; i++) {
            for (int j = 0; j < M; j++) {
                // 왼쪽, 동일, 오른쪽 3방향
                for (int d = 0; d < 3; d++) {
                    // 맨 왼쪽에서 왼쪽에서 온 이전 값을 탐색하는 경우와 맨 오른쪽에서 오른쪽에서 온 이전 값을 탐색하는 경우에는 pass
                    if (d == 0 && j == 0 || d == 2 && j == M - 1) {
                        continue;
                    }

                    // 현재 들어온 방향이 아닌 다른 두 방향의 값에서 최소값과 비교하기
                    for (int k = 0; k < 3; k++) {
                        if (k == d) {
                            continue;
                        }
                        dp[i][j][d] = Math.min(dp[i - 1][j + dc[d]][k] + map[i][j], dp[i][j][d]);
                    }
                }
            }
        }

        int answer = dp[N - 1][0][0];
        for (int i = 0; i < M; i++) {
            for (int j = 0; j < 3; j++) {
                answer = Math.min(dp[N - 1][i][j], answer);
            }
        }

        System.out.println(answer);
    }
}
```