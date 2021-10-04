## [뒤집기2]

- 끝에서 변경하면 맨 앞에서부터 전부 변경되므로 끝에서부터 하나씩 맞추기
- 맨 끝에서부터 하나씩 1을 0으로 바꿔나가면 된다
- 행이나 열을 정해서 뒤에서부터 체크하면 된다
- https://www.acmicpc.net/problem/1455

---

```java
package baekjoon;

import java.io.*;
import java.util.*;

public class Baekjoon1455_뒤집기2 {

    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());

        boolean[][] map = new boolean[N][M];
        for (int i = 0; i < N; i++) {
            String str = br.readLine();
            for (int j = 0; j < M; j++) {
                map[i][j] = str.charAt(j) == '0'; // true 가 앞면
            }
        }

        int count = 0;
        for (int i = N - 1; i >= 0; i--) {
            for (int j = M - 1; j >= 0; j--) {
                if (!map[i][j]) {
                    count++;
                    changeMap(map, i, j);
                }
            }
        }

        System.out.println(count);
    }

    private static void changeMap(boolean[][] map, int r, int c) {
        for (int i = 0; i <= r; i++) {
            for (int j = 0; j <= c; j++) {
                map[i][j] = !map[i][j];
            }
        }
    }
}

```