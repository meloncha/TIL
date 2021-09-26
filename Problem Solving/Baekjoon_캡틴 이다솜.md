## [캡틴 이다솜]

- 단순한 그리디 문제로 풀 수 없다.
- 동전 최소화 문제와 비슷하게 큰 단위부터 계산하는 것보다 작은 단위가 더 작은 갯수가 나올 수 있다
- DP로 풀어야 한다
- N : 30만, 사면체 배열 120 => 3600만 이므로 시간복잡도 통과
- https://www.acmicpc.net/problem/1660

---

```java
package baekjoon;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

public class Baekjoon1660_캡틴이다솜 {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());

        // 1. 사면체당 대포알 갯수를 나타내는 배열 만들기
        // counts 의 크기는 최대 120 (계산결과)
        List<Integer> counts = new ArrayList<>();
        counts.add(1);

        int size = 2;
        while (true) {
            int sum = (size + 1) * size / 2;
            int prev = counts.get(counts.size() - 1);

            if (sum + prev > N) {
                break;
            }
            counts.add(sum + prev);
            size++;
        }

        // 2. 그리디하게 풀면 틀린다. DP로 앞에서부터 최소 개수 갱신
        int[] dp = new int[N + 1];
        Arrays.fill(dp, Integer.MAX_VALUE / 2);
        dp[0] = 0;

        for (int i = 0; i <= N; i++) {
            for (int count : counts) {
                if (i + count <= N) {
                    dp[i + count] = Math.min(dp[i] + 1, dp[i + count]);
                }
            }
        }

        System.out.println(dp[N]);
    }
}
```