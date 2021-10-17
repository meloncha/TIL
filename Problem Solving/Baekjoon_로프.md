# [로프]

- N (최대 10만) 개의 로프가 있다.
- 로프를 k개 연결하면 중량이 w인 물체를 들어올릴 때 w/k 만큼의 중량이 걸린다
- 물체의 최대 중량을 구해야 한다 => 가장 큰 무게부터 차례대로 탐색해보자

---

```java
package baekjoon;

import java.util.*;
import java.io.*;

public class Baekjoon2217_로프 {

    public static void main(String[] args) throws IOException{
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        Integer[] nums = new Integer[N];
        for (int i = 0; i < N; i++) {
            nums[i] = Integer.parseInt(br.readLine());
        }
        Arrays.sort(nums, Collections.reverseOrder());

        int max = 0;
        for (int i = 0; i < N; i++) {
            // 로프 i + 1 개
            max = Math.max(nums[i] * (i + 1), max);
        }

        System.out.println(max);
    }
}

```