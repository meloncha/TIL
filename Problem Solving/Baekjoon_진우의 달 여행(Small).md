## [진우의 달 여행(Small)]

- 세 방향으로 갈 수 있고, 연속으로 두 방향으로 이동할 수 없는 제약조건이 있다
- N, M이 6이하 이기 때문에 시간 복잡도는 6*3^6으로 신경을 쓰지 않아도 될 정도이다
- BFS, DFS 둘 다 가능한 문제
- 최소값을 구하는 것이기 때문에 DFS의 가지치기가 가능하다
- https://www.acmicpc.net/problem/17484

---

```java
package baekjoon;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Baekjoon17484_진우의달여행 {

    static int answer, N, M;
    static int[][] map;
    static int[] dc = { -1, 0, 1 }; // 좌, 동, 우

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());
        answer = Integer.MAX_VALUE;

        map = new int[N][M];
        for (int i = 0; i < N; i++) {
            st = new StringTokenizer(br.readLine());
            for (int j = 0; j < M; j++) {
                map[i][j] = Integer.parseInt(st.nextToken());
            }
        }

        for (int i = 0; i < M; i++) {
            // 이전 움직인 방향 체크용 배열
            dfs(0, i, map[0][i], new boolean[3]);
        }

        System.out.println(answer);
    }

    private static void dfs(int r, int c, int sum, boolean[] check) {
        // 가지치기
        if (sum >= answer) {
            return;
        }

        // 종료조건
        if (r == N - 1) {
            answer = sum;
            return;
        }

        for (int d = 0; d < 3; d++) {
            int nc = c + dc[d];
            if (check[d] || nc < 0 || nc >= M) {
                continue;
            }
            boolean[] temp = new boolean[3];
            temp[d] = true;
            dfs(r + 1, nc, sum + map[r + 1][nc], temp);
        }

    }
}
```