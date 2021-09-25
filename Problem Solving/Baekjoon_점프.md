## [점프]

- 전형적인 DP 문제
- `경로의 개수는 2^63-1 보다 작거나 같다` 라는 말이 있으므로 long 형을 사용해야 한다
- https://www.acmicpc.net/problem/1890

---

```java

package baekjoon;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Baekjoon1890_점프 {

    
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        int[][] map = new int[N][N];
        for (int i = 0; i < N; i++) {
            String[] splits = br.readLine().split(" ");
            for (int j = 0; j < N; j++) {
                map[i][j] = Integer.parseInt(splits[j]);
            }
        }

        long[][] dp = new long[N][N];
        dp[0][0] = 1;
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                // 종료 조건
                if (map[i][j] == 0 ) {
                    break;
                }

                // 1. 오른쪽
                if (j + map[i][j] < N) {
                    dp[i][j + map[i][j]] += dp[i][j];
                }

                // 2. 아래쪽
                if (i + map[i][j] < N) {
                    dp[i + map[i][j]][j] += dp[i][j];
                }
            }
        }

        System.out.println(dp[N - 1][N - 1]);
    }
}

```
