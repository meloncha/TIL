## [말이 되고픈 원숭이]

- W, H 가 평소처럼 행->열 이 아니라 열->행 으로 헷갈리기 때문에 주의해야 한다
- 최소한의 동작으로 왼쪽 상단에서 오른쪽 하단으로 가야한다 => DP?
- 장애물이 있으므로 돌아 가야하는 경우가 생긴다
- DP 불가능 => BFS
- https://www.acmicpc.net/problem/1600

---

```java
package baekjoon;

import java.io.*;
import java.util.*;

public class Baekjoon1600_말이되고픈원숭이 {

    static int[] dr = { -1, 1, 0, 0, -1, -2, -2, -1, 1, 2, 2, 1 };
    static int[] dc = { 0, 0, -1, 1, -2, -1, 1, 2, 2, 1, -1, -2 };

    static class Point {
        int r, c, k;

        public Point(int r, int c, int k) {
            this.r = r;
            this.c = c;
            this.k = k;
        }
    }

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int K = Integer.parseInt(br.readLine());
        StringTokenizer st = new StringTokenizer(br.readLine());
        int W = Integer.parseInt(st.nextToken());
        int H = Integer.parseInt(st.nextToken());

        boolean[][] map = new boolean[H][W];
        for (int i = 0; i < H; i++) {
            st = new StringTokenizer(br.readLine());
            for (int j = 0; j < W; j++) {
                map[i][j] = st.nextToken().charAt(0) == '1';
            }
        }

        final int INF = Integer.MAX_VALUE / 2;
        int[][][] memo = new int[H][W][K + 1]; // 말의 움직임이 0일 때~ K일때
        for (int i = 0; i < H; i++) {
            for (int j = 0; j < W; j++) {
                for (int k = 0; k <= K; k++) {
                    memo[i][j][k] = INF;
                }
            }
        }

        Queue<Point> queue = new LinkedList<>();
        queue.add(new Point(0, 0, 0));
        memo[0][0][0] = 0;

        while (!queue.isEmpty()) {
            Point now = queue.poll();
            int count = memo[now.r][now.c][now.k];

            for (int d = 0; d < 12; d++) {
                // k 가 이미 K 번 채우면 더 이상 말의 움직임 불가능
                if (d >= 4 && now.k == K) {
                    break;
                }

                int nr = now.r + dr[d];
                int nc = now.c + dc[d];

                if (nr < 0 || nc < 0 || nr >= H || nc >= W || map[nr][nc]) {
                    continue;
                }

                // 단순 1칸 움직임일 때
                if (d < 4 && memo[nr][nc][now.k] > count + 1) {
                    memo[nr][nc][now.k] = count + 1;
                    queue.add(new Point(nr, nc, now.k));
                } else if (d >= 4 && memo[nr][nc][now.k + 1] > count + 1) { // 말처럼 이동
                    memo[nr][nc][now.k + 1] = count + 1;
                    queue.add(new Point(nr, nc, now.k + 1));
                }
            }
        }

        int ans = INF;
        for (int k = 0; k <= K; k++) {
            ans = Math.min(memo[H - 1][W - 1][k], ans);
        }
        System.out.println(ans == INF ? -1 : ans);
    }
}
```