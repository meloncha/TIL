## [세 용액]

- 특성값이 -10억 ~ 10억 이므로 세 특성을 더하면 int 형을 벗어난다 => long 형
- N이 최대 5000이므로 O(N^2)의 시간복잡도로 해결해야 한다. => 정렬 후 투포인터 사용하기
- 시간 복잡도 : 정렬 NlogN + 탐색 N^2 = O(N^2)
- 시간초과가 나서 
    1. String 더하기를 StringBuilder 로 변환
    2. 2중 for문에서 0을 찾은 경우 더 이상 확인하지 않고 escape 하도록 개선
- 틀림
    1. 합은 long 형이지만 각 원소가 int 형으로 더하여 합이 달라지는 문제가 있어서 type casting

---

```java
import java.util.*;
import java.io.*;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        int[] liquids = Arrays.stream(br.readLine().split(" ")).mapToInt(Integer::parseInt).sorted().toArray();

        int[] ans = new int[3];
        
        solve(ans, liquids, N);        

        StringBuilder sb = new StringBuilder();
        for (int v : ans) {
            sb.append(v).append(" ");
        }
        System.out.println(sb);
    }

    private static void solve(int[] ans, int[] liquids, int N) {
        long min = Long.MAX_VALUE;

        for (int i = 0; i < N - 2; i++) {
            int left = i + 1;
            int right = N - 1;

            while (left < right) {
                long sum = (long) liquids[i] + liquids[left] + liquids[right];
                if (Math.abs(sum) < min) {
                    min = Math.abs(sum);
                    ans[0] = liquids[i];
                    ans[1] = liquids[left];
                    ans[2] = liquids[right];
                }

                if (sum > 0) { // 합이 0보다 크면 right 를 줄여서 합을 작게 한다
                    right--;
                } else if (sum < 0) { // 합이 0보다 작으면 left 를 키워서 합을 크게 한다
                    left++;
                } else { // 0 이면 더 이상 볼 필요없다
                    return;
                }
            }
        }
    }
}

```