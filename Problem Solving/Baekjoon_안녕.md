## [안녕]

- N이 최대 20이므로 2^20 = 100만 으로 DFS 가능
- 0/1 배낭 채우기 문제로도 풀 수 있다 DP 가능
- https://www.acmicpc.net/problem/1535

```java
package baekjoon;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;

public class Baekjoon1535_안녕 {

    static int answer, N;
    static int[] loses, joys;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        N = Integer.parseInt(br.readLine());
        loses = Arrays.stream(br.readLine().split(" ")).mapToInt(Integer::parseInt).toArray();
        joys = Arrays.stream(br.readLine().split(" ")).mapToInt(Integer::parseInt).toArray();

//        dfs(0, 0, 100);

        dynamicProgramming();

        System.out.println(answer);
    }

    private static void dynamicProgramming() {

        int[][] dp = new int[N + 1][100];
        for (int i = 1; i <= N; i++) {
            for (int j = 0; j < 100; j++) {

                // 현재 무게보다 아이템 무게가 작으면 진행
                if (j >= loses[i - 1]) {
                    dp[i][j] = Math.max(dp[i - 1][j], dp[i - 1][j - loses[i - 1]] + joys[i - 1]);
                } else {
                    dp[i][j] = dp[i - 1][j];
                }
            }
        }

        answer = dp[N][99];
    }


    private static void dfs(int idx, int joy, int health) {
        if (health <= 0) {
            return;
        }

        if (idx == N) {
            answer = Math.max(joy, answer);
            return;
        }

        // 인사하는 경우
        dfs(idx + 1, joy + joys[idx], health - loses[idx]);

        // 인사 안하는 경우
        dfs(idx + 1, joy, health);
    }
}

```