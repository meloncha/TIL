## [소형기관차]

- N 최대 5만, 3개 기관차 사용가능 => DP
- 현재 인덱스에서의 최댓값은
- 1. 객차 크기 뺀 곳에서의 최댓값 + sum
- 2. 하나 전 값값
- https://www.acmicpc.net/problem/2616

---

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        int[] guests = Arrays.stream(br.readLine().split(" ")).mapToInt(Integer::parseInt).toArray();
        int M = Integer.parseInt(br.readLine());

        int[][] dp = new int[4][N]; // 기관차 1,2,3대일 때의 최댓값을 DP로 표현하기
        dp[1][M - 1] = sum(guests, M - 1, M);

        for (int i = 1; i <= 3; i++) {
            for (int j = M; j < N; j++) {
                int sum = sum(guests, j, M);
                dp[i][j] = Math.max(dp[i - 1][j - M] + sum, dp[i][j - 1]);

            }
        }

        System.out.println(dp[3][N - 1]);
    }

    private static int sum(int[] guests, int end, int size) {
        int result = 0;
        while (size > 0) {
            result += guests[end];
            end--;
            size--;
        }
        return result;
    }
}

```