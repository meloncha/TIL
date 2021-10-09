# [에너지 모으기]

- 없애는 순서에도 영향이 있다 => 순열
- N개의 에너지 구슬이 있다면 총 N-2개(맨 앞, 맨 뒤 제외)를 뽑아야 한다
- N:10, W:1000 일때 최대값은 100만 * 10 = 1000만으로 int 형 가능

---

```java
import java.io.*;
import java.util.*;

public class Main {

    static int[] energy;
    static int N, answer;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        answer = Integer.MIN_VALUE;
        N = Integer.parseInt(br.readLine());
        energy = Arrays.stream(br.readLine().split(" ")).mapToInt(Integer::parseInt).toArray();

        // 1. 순열만들기
        dfs(0, new boolean[N], new int[N - 2]);

        System.out.println(answer);
    }

    private static void dfs(int idx, boolean[] visit, int[] select) {
        if (idx == N - 2) {
            // 2. 에너지 모으기
            addEnergy(select);
            return;
        }

        for (int i = 1; i < N - 1; i++) {
            if (!visit[i]) {
                visit[i] = true;
                select[idx] = i;
                dfs(idx + 1, visit, select);
                visit[i] = false;
            }
        }
    }

    private static void addEnergy(int[] select) {
        // 3. select 배열을 순서로 에너지 합 계산
        int[] nums = new int[N];
        for (int i = 0; i < N; i++) {
            nums[i] = energy[i];
        }

        int sum = 0;
        for (int i = 0; i < select.length; i++) {
            int idx = select[i];

            int left = idx - 1;
            while (nums[left] == -1) {
                left--;
            }
            int right = idx + 1;
            while (nums[right] == -1) {
                right++;
            }

            sum += nums[left] * nums[right];
            nums[idx] = -1; // 해당 칸 없애는 효과

        }
        answer = Math.max(sum, answer);
    }
}
```