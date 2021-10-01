## [탈출]



- 고슴도치가 굴로 이동하기 위한 최소 시간을 구하는 문제
- 고슴도치는 물이 찰 예정인 칸을 이동할 수 없다 => 물이 확산되는 행위가 먼저 일어나야 한다
- 물이 먼저 확산하고, 고슴도치가 나중에 이동하는 순서가 중요하다
- 매 시간마다 순서에 맞춰 이동하고, 답을 구한다
- 물이 확산되는 것과 고슴도치가 이동하는 위치를 **BFS**로 탐색하는 것이 좋아보인다
- https://www.acmicpc.net/problem/3055



---



```java
package baekjoon;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.LinkedList;
import java.util.Queue;
import java.util.StringTokenizer;

public class Baekjoon3055_탈출 {

    static int R, C;
    static String ans;
    static char[][] map;
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
        R = Integer.parseInt(st.nextToken());
        C = Integer.parseInt(st.nextToken());

        map = new char[R][];
        for (int i = 0; i < R; i++) {
            map[i] = br.readLine().toCharArray();
        }

        solve();

        System.out.println(ans);
    }

    private static void solve() {
        int count = 0;
        Queue<Point> queueWater = new LinkedList<>();
        Queue<Point> queueAnimal = new LinkedList<>();
        boolean[][] visited = new boolean[R][C];

        // S, * 위치 찾아서 시작하기
        // queue 에는 물이 먼저 들어가고, 고슴도치의 위치가 나중에 들어간다
        checkWater(queueWater);
        checkAnimal(queueAnimal, visited);

        while (!queueAnimal.isEmpty()) {
            count++;

            // 물 확장
            expandWater(queueWater);

            // 고슴도치 이동
            boolean isEnd = moveAnimal(queueAnimal, visited);

            if (isEnd) {
                ans = count + "";
                return;
            }

        }

        ans = "KAKTUS";
    }

    private static void checkAnimal(Queue<Point> queueAnimal, boolean[][] visited) {
        for (int i = 0; i < R; i++) {
            for (int j = 0; j < C; j++) {
                if (map[i][j] == 'S') {
                    queueAnimal.add(new Point(i, j));
                    visited[i][j] = true;
                    map[i][j] = '.';
                }
            }
        }
    }

    private static void checkWater(Queue<Point> queueWater) {
        for (int i = 0; i < R; i++) {
            for (int j = 0; j < C; j++) {
                if (map[i][j] == '*') {
                    queueWater.add(new Point(i, j));
                }
            }
        }
    }

    private static void expandWater(Queue<Point> queueWater) {
        int size = queueWater.size();
        for (int i = 0; i < size; i++) {
            Point now = queueWater.poll();

            for (int d = 0; d < 4; d++) {
                int nr = now.r + dr[d];
                int nc = now.c + dc[d];

                if (nr < 0 || nr >= R || nc < 0 || nc >= C || map[nr][nc] != '.') {
                    continue;
                }
                queueWater.add(new Point(nr, nc));
                map[nr][nc] = '*';
            }
        }
    }

    private static boolean moveAnimal(Queue<Point> queueAnimal, boolean[][] visited) {
        int size = queueAnimal.size();
        for (int i = 0; i < size; i++) {

            Point now = queueAnimal.poll();

            for (int d = 0; d < 4; d++) {
                int nr = now.r + dr[d];
                int nc = now.c + dc[d];

                if (nr < 0 || nr >= R || nc < 0 || nc >= C || map[nr][nc] == '*' || map[nr][nc] == 'X' || visited[nr][nc]) {
                    continue;
                }

                if (map[nr][nc] == 'D') {
                    return true;
                }

                queueAnimal.add(new Point(nr, nc));
                visited[nr][nc] = true;
            }

        }
        return false;
    }

}

```

