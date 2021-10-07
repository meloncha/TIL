## [빙산]

- 두 덩어리 이상으로 분리되는 최초의 시간을 찾아라
- 한 점에서 BFS 탐색을 하고 난 뒤 탐색이 안된 빙산이 있다면 덩어리가 쪼개진 것이다. => 정답

---

```java
import java.util.*;
import java.io.*;

public class Main {

    static int N, M, time;
    static int[][] map;
    static int[] dr = { -1, 1, 0, 0 };
    static int[] dc = { 0, 0, -1, 1 };
    static int[][] waterCount;


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

        System.out.println(time);
    }

    private static void solve() {
        waterCount = new int[N][M];

        while (true) {
            // time 증가
            time++;

            // 빙산 하나하나마다 주위에 바닷물이 얼마나 있는지 체크한다
            checkWaters();

            // count 만큼 각 빙산에서 값을 빼기
            modifyMap();

            // 빙산 하나 찾기
            Point p = findOne();

            // 전부 녹아버린 경우
            if (p == null) {
                time = 0;
                break;
            }

            // BFS 를 통해 덩어리 확인
            if (isFinish(p)) {
                break;
            }
        }


    }

    private static Point findOne() {
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {
                if (map[i][j] != 0) {
                    return new Point(i, j);
                }
            }
        }
        return null;
    }

    private static boolean isFinish(Point p) {
        Queue<Point> queue = new LinkedList<>();
        boolean[][] visit = new boolean[N][M];

        queue.add(p);
        visit[p.r][p.c] = true;

        // BFS 탐색
        while (!queue.isEmpty()) {
            Point now = queue.poll();

            for (int d = 0; d < 4; d++) {
                int nr = now.r + dr[d];
                int nc = now.c + dc[d];

                if (nr < 0 || nc < 0 || nr >= N || nc >= M || map[nr][nc] == 0 || visit[nr][nc]) {
                    continue;
                }

                queue.add(new Point(nr, nc));
                visit[nr][nc] = true;
            }
        }

        // 방문하지 않은 덩어리가 있는 경우 분리된 것
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {
                if (map[i][j] != 0 && !visit[i][j]) {
                    return true;
                }
            }
        }
        return false;
    }

    private static void modifyMap() {
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {
                if (map[i][j] == 0) {
                    continue;
                }

                map[i][j] = Math.max(map[i][j] - waterCount[i][j], 0);
            }
        }
    }

    private static void checkWaters() {
        // 배열 초기화
        for (int i = 0; i < N; i++) {
            Arrays.fill(waterCount[i], 0);
        }

        // 배열에 count 저장
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < M; j++) {
                if (map[i][j] == 0) {
                    continue;
                }

                for (int d = 0; d < 4; d++) {
                    int nr = i + dr[d];
                    int nc = j + dc[d];

                    if (nr >= 0 && nc >= 0 && nr < N && nc < M && map[nr][nc] == 0) {
                        waterCount[i][j]++;
                    }
                }
            }
        }
    }
}


```