## [태상이의 훈련소 생활]

- N, M이 최대 10만이므로 O(N)의 시간복잡도로 해결해야 한다
- 누적합 기술을 사용하여 구할 수 있다
- https://www.acmicpc.net/problem/19951

---

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.StringTokenizer;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());

        int[] heights = Arrays.stream(br.readLine().split(" ")).mapToInt(Integer::parseInt).toArray();
        int[] sums = new int[N + 1];
        for (int i = 0; i < M; i++) {
            st = new StringTokenizer(br.readLine());
            int start = Integer.parseInt(st.nextToken()) - 1;
            int end = Integer.parseInt(st.nextToken()) - 1;
            int height = Integer.parseInt(st.nextToken());

            sums[start] += height;
            sums[end + 1] -= height;
        }

        for (int i = 1; i < N; i++) {
            sums[i] += sums[i - 1];
        }

        for (int i = 0; i < N; i++) {
            heights[i] += sums[i];
        }

        StringBuilder sb = new StringBuilder();
        for (int height : heights) {
            sb.append(height).append(" ");
        }
        System.out.println(sb);
    }
}
```