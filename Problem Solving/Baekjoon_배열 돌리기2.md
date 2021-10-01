## [배열 돌리기2]



- 배열을 반 시계 방향으로 돌리는 단순한 구현 문제
- 회전 수 R이 최대 10억이므로 R만큼 회전시킬 수는 없다
- 회전 반경이 같은 애들끼리는 갯수만큼 돌리면 자기 자신으로 돌아와 안 돌린 것과 같다
- 결국 `R % (회전 반경 갯수)` 만큼만 돌리면 된다

- https://www.acmicpc.net/problem/16927



---



```java
package baekjoon;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Baekjoon16927_배열돌리기2 {

    static int N, M, R;
    static int[][] map;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        N = Integer.parseInt(st.nextToken());
        M = Integer.parseInt(st.nextToken());
        R = Integer.parseInt(st.nextToken());

        map = new int[N][M];
        for (int i = 0; i < N; i++) {
            st = new StringTokenizer(br.readLine());
            for (int j = 0; j < M; j++) {
                map[i][j] = Integer.parseInt(st.nextToken());
            }
        }

        dfs(0, 0);

        StringBuilder sb = new StringBuilder();
        for (int[] row : map) {
            for (int v : row) {
                sb.append(v).append(" ");
            }
            sb.append("\n");
        }
        System.out.println(sb);

    }

    private static void dfs(int r, int c) {
        // 1. 같은 회전 반경의 갯수 구하기
        int size = calcSize(r, c);
        if (size == 0) {
            return;
        }

        // 2. R % size 만큼 배열 돌리기
        moveArray(R % size, r, c);

        // 3. 다음 반경으로 넘어가기
        dfs(r + 1, c + 1);

    }

    private static int calcSize(int r, int c) {
        int size = 0;
        for (int i = r; i < N - r; i++) {
            for (int j = c; j < M - c; j++) {
                if (i == r || i == N - 1 - r || j == c || j == M - 1 - c) {
                    size++;
                }
            }
        }
        return size;
    }

    private static void moveArray(int count, int r, int c) {
        // R % size 만큼 배열을 돌리기, 시계방향으로 돌면서 값 갱신해주기
        for (int i = 0; i < count; i++) {
            int nr = r;
            int nc = c;
            int temp = map[nr][nc];

            // 오른쪽으로
            while (nc + 1 < M - c) {
                map[nr][nc] = map[nr][nc + 1];
                nc++;
            }

            // 아래쪽으로
            while (nr + 1 < N - r) {
                map[nr][nc] = map[nr + 1][nc];
                nr++;
            }

            // 왼쪽으로
            while (nc - 1 >=  c) {
                map[nr][nc] = map[nr][nc - 1];
                nc--;
            }

            // 위쪽으로
            // map[r][c] 에는 갱신된 값이 들어있어서 map[r + 1][c]에는 temp 를 넣어줘야 한다
            while (nr - 1 >=  r) {
                map[nr][nc] = map[nr - 1][nc];
                nr--;
            }
            map[nr + 1][nc] = temp;
        }
    }
}

```

