## [알바생 강호]

- 팁이 음수인 경우에는 0원을 받기 때문에 팁 내림차순으로 정렬하면 그리디하게 최댓값이 나온다
- N 최대 10만, 팁 최대 10만이므로 10만~1까지 합은 100001*50000 = 50억을 넘어가서 int 형 넘어간다
-https://www.acmicpc.net/problem/1758

---

```java
package baekjoon;

import java.io.*;
import java.util.*;

public class Baekjoon1758_알바생강호 {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        Integer[] tips = new Integer[N];
        for (int i = 0; i < N; i++) {
            tips[i] = Integer.parseInt(br.readLine());
        }

        Arrays.sort(tips, Collections.reverseOrder());
        long sum = 0;
        for (int i = 0; i < N; i++) {
            int tip = Math.max(tips[i] - i, 0);
            sum += tip;
        }
        System.out.println(sum);
    }
}

```