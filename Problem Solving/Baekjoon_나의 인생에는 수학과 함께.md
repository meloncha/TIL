## [나의 인생에는 수학과 함께]

- 지도의 크기 N 이 최대 5이기 때문에 시간복잡도는 크게 신경쓸 필요가 없다
- 오른쪽과 아래쪽으로만 이동하기 때문에 visit 배열은 필요없다
- DFS, BFS, DP 로 풀 수 있다. => DP 로 풀어보자
- https://www.acmicpc.net/problem/17265

---

- 배열의 인덱스를 다룰 때에는 헷갈리지 않도록 주의하자

```java
package baekjoon;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Baekjoon17265_나의인생에는수학과함께 {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        char[][] map = new char[N][N];
        for (int i = 0; i < N; i++) {
            String[] s = br.readLine().split(" ");
            for (int j = 0; j < N;j ++) {
                map[i][j] = s[j].charAt(0);
            }
        }

        // dp 배열 생성 및 초기화
        int[][] maxDP = new int[N][N];
        int[][] minDP = new int[N][N];
        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                maxDP[i][j] = -1000000;
                minDP[i][j] = 1000000;
            }
        }
        maxDP[0][0] = minDP[0][0] = map[0][0] - '0';

        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                // 인덱스 둘을 더한 것이 짝수인 곳에 숫자가 들어있다
                if ((i + j) % 2 != 0) {
                    continue;
                }

                int num = map[i][j] - '0';

                // 1. (i-2, j)과 비교
                if (i >= 2) {
                    maxDP[i][j] = Math.max(maxDP[i][j], calc(maxDP[i - 2][j], map[i - 1][j], num));
                    minDP[i][j] = Math.min(minDP[i][j], calc(minDP[i - 2][j], map[i - 1][j], num));
                }

                // 2. (i-1, j-1)과 비교
                if (i >= 1 && j >= 1) {
                    maxDP[i][j] = Math.max(maxDP[i][j], calc(maxDP[i - 1][j - 1], map[i - 1][j], num));
                    maxDP[i][j] = Math.max(maxDP[i][j], calc(maxDP[i - 1][j - 1], map[i][j - 1], num));
                    minDP[i][j] = Math.min(minDP[i][j], calc(minDP[i - 1][j - 1], map[i - 1][j], num));
                    minDP[i][j] = Math.min(minDP[i][j], calc(minDP[i - 1][j - 1], map[i][j - 1], num));
                }

                // 3. (i, j-2)와 비교
                if (j >= 2) {
                    maxDP[i][j] = Math.max(maxDP[i][j], calc(maxDP[i][j - 2], map[i][j - 1], num));
                    minDP[i][j] = Math.min(minDP[i][j], calc(minDP[i][j - 2], map[i][j - 1], num));
                }

            }
        }



        System.out.println(maxDP[N - 1][N - 1] + " " + minDP[N - 1][N - 1]);
    }

    private static int calc(int A, char op, int B) {
        if (op == '+') {
            return A + B;
        } else if (op == '-') {
            return A - B;
        } else {
            return A * B;
        }
    }
}
```