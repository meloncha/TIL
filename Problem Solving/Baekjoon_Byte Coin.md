## [Byte Coin]

- n이 최대 15이고 w가 최대 10만
- 1원일때 사서 50원일때 파는 것을 최대 15일동안 할 수 있으므로 최대 50^7배의 수익을 낼 수 있다 
- 그러므로 현금의 최댓값은 int형을 벗어나므로 *long*형이 필수
- 쌀 때 사서 비쌀 때 파는 것이 좋다(greedy) => 떨어지다가 올라갈 때의 변곡점에서 사고, 올라가다가 떨어질 때의 변곡점에서 팔아야 한다
- 마지막 날까지 코인이 남아있다면 다 팔아서 현금화 해야한다
- https://www.acmicpc.net/problem/17521

---

- 꼼꼼하게 푸는 것이 중요한 문제
- 난이도는 낮은데도 정답률이 낮은 이유는 정답의 범위를 신경쓰지 않았기 때문이다

```java
package baekjoon;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Baekjoon17521_ByteCoin {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int N = Integer.parseInt(st.nextToken());
        int W = Integer.parseInt(st.nextToken());

        int[] chart = new int[N];
        for (int i = 0; i < N; i++) {
            chart[i] = Integer.parseInt(br.readLine());
        }

        long money = W;
        long coin = 0;

        for (int i = 0; i < N - 1; i++) {
            int now = chart[i];
            int next = chart[i + 1];

            // 현재보다 내일이 더 비싼 경우 모든 돈으로 코인을 구입한다
            if (now < next) {
                coin += money / now;
                money %= now;
            } else if (now > next) { // 내일보다 오늘이 비싼 경우에는 모든 코인을 판다
                money += coin * now;
                coin = 0;
            }
        }

        // 마지막날까지 왔는데 코인이 남은 경우 마지막 날 가격 기준으로 팔기
        // 코인을 산 이후로 계속 가격이 같은 경우에는 매도가 불가능하기 때문에 처리
        if (coin > 0) {
            money += coin * chart[N - 1];
        }

        System.out.println(money);
    }
}
```