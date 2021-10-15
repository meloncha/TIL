# [과자 나눠주기]

- M, N 이 모두 최대 100만
- 같은 길이의 막대 과자를 나눠 주어야 한다
- 과자를 합칠 수는 없고 나누어 줄 수만 있다
- 이분 탐색으로 해결해보자

---

```java
import java.util.*;
import java.io.*;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int M = Integer.parseInt(st.nextToken());
        int N = Integer.parseInt(st.nextToken());

        int[] lengths = Arrays.stream(br.readLine().split(" ")).mapToInt(Integer::parseInt).sorted().toArray();

        // 못 나누어주면 답은 0
        int answer = 0;

        int left = 1;
        int right = lengths[N - 1];
        while (left <= right) {
            int mid = left + (right - left) / 2;

            int count = 0;
            for (int i = 0; i < N; i++) {
                count += lengths[i] / mid;
            }

            // mid 로 나누어줬을때 수가 조카 수보다 크면 길이를 늘린다
            if (count >= M) {
                answer = mid;
                left = mid + 1;
            } else { // 더 작으면 right 를 줄인다
                right = mid - 1;
            }

        }

        System.out.println(answer);

    }
}
```