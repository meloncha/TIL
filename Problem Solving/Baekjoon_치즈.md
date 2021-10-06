## [치즈]

- 치즈 외부의 공기가 2면 이상 접촉하면 1시간 후 녹아 없어진다
- 치즈 내부의 공기는 관련이 없다
- 외부에서 BFS 탐색을 통해 치즈와 맞닿은 면들을 체크하자
- https://www.acmicpc.net/problem/2638

---

```java
import java.io.*;
import java.util.*;

public class Main {

    /*
    
     */
    static int N, M, ans;
    static int[][] map;
    static int[] dr = { -1, 1, 0, 0 };
    static int[] dc = { 0, 0, -1, 1 };
    
    static class Point {
        int r, c;

        public Point(int r, int c) {
            this.r = r;
            this.c = c;
        }
    }


    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());
        map = new int[N][M];
        for (int i = 0; i < N; i++) {
            st = new StringTokenizer(br.readLine());
            for (int j = 0; j < M; j++) {
                map[i][j] = Integer.parseInt(st.nextToken());
            }
        }

        solve();

        System.out.println(ans);
    }

    private static void solve() {
        Queue<Point> queue = new LinkedList<>();
        boolean[][] visit = new boolean[N][M];

        // 0. 맵이 전부 0인지 확인
        while (!isCheeseGone()) {
            // 1. ans 증가
            ans++;

            // 2. BFS 로 맵 순회하면서 map 의 치즈에 맞닿은 면 체크해주기
            checkCheese(queue, visit);

            // 3. 맵 순회하면서 맞닿은 면이 2개 이상인 부분 (1 + 2 이므로 3이상) 0으로 날리기
            modifyCheese();
        }
    }

    private static void modifyCheese() {
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {
                if (map[i][j] >= 3) { // 2면 이상 맞닿으면 0으로
                    map[i][j] = 0;
                } else if (map[i][j] >= 1) {
                    map[i][j] = 1; // 0~1면 맞닿으면 다시 1로
                }
            }
        }
    }

    private static void checkCheese(Queue<Point> queue, boolean[][] visit) {
        for (int i = 0; i < N; i++) {
            Arrays.fill(visit[i], false);
        }

        queue.add(new Point(0, 0));
        visit[0][0] = true;

        while (!queue.isEmpty()) {
            Point now = queue.poll();

            for (int d = 0; d < 4; d++) {
                int nr = now.r + dr[d];
                int nc = now.c + dc[d];

                if (nr < 0 || nc < 0 || nr >= N || nc >= M || visit[nr][nc]) {
                    continue;
                }

                // 다음 위치가 치즈라면 공기와 맞닿은 것이다
                if (map[nr][nc] != 0) {
                    map[nr][nc]++;
                } else { // 공기라면 큐에 넣기
                    queue.add(new Point(nr, nc));
                    visit[nr][nc] = true;
                }
            }
        }
    }

    private static boolean isCheeseGone() {
        for (int[] row : map) {
            for (int v : row) {
                if (v != 0) {
                    return false;
                }
            }
        }
        return true;
    }
}

```