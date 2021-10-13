# [영재의 시험]

- DFS 로 로직에 따라 번호를 선택한다
- 정답과 비교 후 점수가 5 이상인 경우를 count 한다
- DFS 횟수 : 5^10 = 1000만

---

```java
package baekjoon;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;

public class Baekjoon19949_영재의시험 {

    static int[] nums;
    static int count;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        nums = Arrays.stream(br.readLine().split(" ")).mapToInt(Integer::parseInt).toArray();

        dfs(0, new int[nums.length]);

        System.out.println(count);
    }

    private static void dfs(int idx, int[] select) {
        if (idx == select.length) {
            int score = 0;
            for (int i = 0; i < select.length; i++) {
                if (select[i] == nums[i]) {
                    score++;
                }
            }

            if (score >= 5) {
                count++;
            }
            return;
        }

        for (int i = 1; i <= 5; i++) {
            if (idx >= 2) {
                int num1 = select[idx - 2];
                int num2 = select[idx - 1];

                if (num1 == i && num2 == i) {
                    continue;
                }
            }
            select[idx] = i;
            dfs(idx + 1, select);
        }
    }
}
```