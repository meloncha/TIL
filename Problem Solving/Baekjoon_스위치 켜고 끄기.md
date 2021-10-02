## [스위치 켜고 끄기]

- 구현문제
- https://www.acmicpc.net/problem/1244

---


```java
package baekjoon;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Baekjoon1244_스위치켜고끄기 {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        StringTokenizer st = new StringTokenizer(br.readLine());
        boolean[] switchs = new boolean[N];
        for (int i = 0; i < N; i++) {
            switchs[i] = st.nextToken().charAt(0) == '1';
        }

        int M = Integer.parseInt(br.readLine());
        for (int m = 0; m < M; m++) {
            st = new StringTokenizer(br.readLine());
            char gender = st.nextToken().charAt(0);
            int num = Integer.parseInt(st.nextToken());

            if (gender == '1') { // 남자인 경우
                for (int i = 0; i < N; i++) {
                    if ((i + 1) % num == 0) {
                        switchs[i] = !switchs[i];
                    }
                }
            } else { // 여자인 경우
                switchs[num - 1] = !switchs[num - 1];

                // 좌우로 확장시키면서 체크하기
                int left = num - 2;
                int right = num;
                while (left >= 0 && right < N && switchs[left] == switchs[right]) {
                    switchs[left] = !switchs[left];
                    switchs[right] = !switchs[right];
                    left--;
                    right++;
                }
            }

        }

        StringBuilder sb = new StringBuilder();
        for (int i = 0; i < N; i++) {
            sb.append(switchs[i] ? "1 " : "0 ");
            if (i % 20 == 19) {
                sb.append("\n");
            }
        }
        System.out.println(sb);
    }
}
```