## [종이의 개수]

- 단순한 DFS 문제
- https://www.acmicpc.net/problem/1780

---

```java
package baekjoon;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.StringTokenizer;

public class Baekjoon1780_종이의개수 {

    static int[] answer = new int[3];
    static int[][] map;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        map = new int[N][N];
        for (int i = 0; i < N; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            for (int j = 0; j < N; j++) {
                map[i][j] = Integer.parseInt(st.nextToken());
            }
        }

        dfs(0, 0, N);

        Arrays.stream(answer).forEach(System.out::println);
    }

    private static void dfs(int r, int c, int n) {
        // 종료조건
        if (check(r, c, n)) {
            answer[map[r][c] + 1]++;
            return;
        }

        // 9등분 하기
        for (int i = r; i < r + n; i += n/3) {
            for (int j = c; j < c + n; j += n/3) {
                dfs(i, j, n/3);
            }
        }
    }

    private static boolean check(int r, int c, int n) {
        for (int i = r; i < r + n; i++) {
            for (int j = c; j < c + n; j++) {
                if (map[i][j] != map[r][c]) {
                    return false;
                }
            }
        }
        return true;
    }
}

```