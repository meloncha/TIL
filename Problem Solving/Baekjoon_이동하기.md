## [이동하기]

- 사탕을 최대한으로 가져오는 것이기 때문에 DFS는 어울리지 않는다. 
- 갈 수 있는 방향이 아래, 오른쪽, 아래 + 오른쪽 세 가지이다. 
- BFS, DP로 풀 수 있을 것 같다
- BFS로 푸는 경우 메모이제이션을 사용해도 메모리 초과가 난다 
- *DP*로만 풀어야 하는 문제

---

- DP로 풀 수 있는 문제인지 먼저 판단하고 접근하는 것이 좋아보인다.

```java
package baekjoon;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;

public class Baekjoon11048_이동하기 {

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

        int answer = 0;
        // answer = solve1(map, N, M);

        answer = solve2(map, N, M);

        System.out.println(answer);
    }

    static class Point {
        int r, c, candy;

        public Point(int r, int c, int candy) {
            this.r = r;
            this.c = c;
            this.candy = candy;
        }
    }

    // 하, 우, 하 + 우
    static int[] dr = { 1, 0, 1 };
    static int[] dc = { 0, 1, 1 };

    // BFS 풀이 => 아무리 해도 메모리 초과를 피할 수 없다. 
    private static int solve1(int[][] map, int N, int M) {
        // 뒤로 갈 수 없으므로 visit 배열은 필요 없다
        Queue<Point> queue = new LinkedList<>();
        queue.add(new Point(0, 0, map[0][0]));

        // 단순하게 queue 만 사용하면 메모리 초과가 난다 => 메모이제이션 사용하기
        int[][] memo = new int[N][M];

        while (!queue.isEmpty()) {
            Point now = queue.poll();

            for (int d = 0; d < 3; d++) {
                int nr = now.r + dr[d];
                int nc = now.c + dc[d];

                // 메모배열의 원소보다 작으면 최선의 값이 아님 => 배제한다
                if (nr < 0 || nr >= N || nc < 0 || nc >= M || now.candy + map[nr][nc] <= memo[nr][nc]) {
                    continue;
                }

                queue.add(new Point(nr, nc, now.candy + map[nr][nc]));
                memo[nr][nc] = now.candy + map[nr][nc];
            }

        }
        return memo[N - 1][M - 1];
    }

    // DP 풀이
    private static int solve2(int[][] map, int N, int M) {
        int[][] dp = new int[N][M];
        dp[0][0] = map[0][0];
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {
                // 1. 위 -> 아래
                if (i > 0) {
                    dp[i][j] = Math.max(dp[i - 1][j] + map[i][j], dp[i][j]);
                }

                // 2. 왼쪽 -> 오른쪽
                if (j > 0) {
                    dp[i][j] = Math.max(dp[i][j - 1] + map[i][j], dp[i][j]);
                }

                // 3. 대각선
                if (i > 0 && j > 0) {
                    dp[i][j] = Math.max(dp[i - 1][j - 1] + map[i][j], dp[i][j]);
                }

            }
        }
        return dp[N - 1][M - 1];
    }
}
```

