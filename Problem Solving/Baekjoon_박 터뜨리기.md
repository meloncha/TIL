## [박 터뜨리기]

- N 개의 공을 K 개의 바구니에 나누어 담는데 바구니에 담긴 공의 개수가 모두 달라야 한다
- 공의 개수 차이가 최소가 되기 위해서는 N/K 만큼 담고 공의 개수를 조절해서 다르게 담아야 한다
- https://www.acmicpc.net/problem/19939

---

1. A ~ A+K-1 까지의 합은 (2A+K-1)K/2 이다.
2. A를 늘리면서 합이 N보다 같거나 작을 때까지 확인
3. 같으면 답은 K-1, 작으면 답은 K. 처음부터 넘어가면 답은 -1

```java
package baekjoon;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Baekjoon19939_박터뜨리기 {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int N = Integer.parseInt(st.nextToken());
        int K = Integer.parseInt(st.nextToken());

        int answer = -1;
        // 1 ~ K 까지의 합이 N 보다 크다면 답은 -1이므로 아닌 경우에만 계산하기
        if (K * (K + 1) / 2 <= N) {

            int sum = 0;
            int a = 1;
            while (true) {
                int tempSum = (2 * a + K - 1) * K / 2;
                if (tempSum > N) {
                    break;
                }
                sum = tempSum;
                a++;
            }

            answer = sum == N ? K - 1 : K;
        }

        System.out.println(answer);
    }

}
```