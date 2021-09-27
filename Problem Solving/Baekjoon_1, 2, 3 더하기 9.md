## [1, 2, 3 더하기 #9]

1. DP 문제
2. 테스트 케이스마다 계산을 DP 계산을 하니까 시간초과가 난다
    - 1000 * 1000 * T 이므로 T가 100이상이면 시간초과가 날 수 밖에 없다
3. 그렇다면 주어진 숫자 중 최대 N과 최대 M을 찾아서 그것만큼 DP 연산을 한 번만 하자 (N: 1000, M: 1000)
    - 시간 복잡도 O(N * M)

- https://www.acmicpc.net/problem/16195

---

```java
package baekjoon;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Baekjoon16195_123더하기9 {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int MOD = 1000000009;
        StringBuilder sb = new StringBuilder();

        // 최대 크기 dp 배열 설정 및 초기화
        int[][] dp = new int[1001][1001];
        for (int i = 1; i <= 3; i++) {
            dp[i][1] = 1;
        }

        for (int i = 1; i <= 1000; i++) {
            for (int j = 1; j <= 1000; j++) {
                // 1. 1을 더해서 만들기
                dp[i][j] = (dp[i][j] + dp[i - 1][j - 1]) % MOD;

                // 2. 2를 더해서 만들기
                if (i >= 2) {
                    dp[i][j] = (dp[i][j] + dp[i - 2][j - 1]) % MOD;
                }

                // 3. 3을 더해서 만들기
                if (i >= 3) {
                    dp[i][j] = (dp[i][j] + dp[i - 3][j - 1]) % MOD;
                }
            }
        }

        int T = Integer.parseInt(br.readLine());
        for (int t = 0; t < T; t++) {
            String[] splits = br.readLine().split(" ");
            int N = Integer.parseInt(splits[0]);
            int M = Integer.parseInt(splits[1]);

            int sum = 0;
            for (int i = 0; i <= M; i++) {
                sum = (sum + dp[N][i]) % MOD;
            }
            sb.append(sum).append("\n");
        }

        System.out.println(sb);

    }
}
```