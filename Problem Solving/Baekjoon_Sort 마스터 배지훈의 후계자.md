# [Sort 마스터 배지훈의 후계자]

- N, M 최대 20만이기 때문에 O(NM) = 400억으로 불가능하다
- 정렬 후 이분탐색을 하면 정렬 NlogN + 탐색 MlogN => O((N+M)logN) 의 시간복잡도로 해결가능하다
- 이분탐색으로 값을 찾았을 때 값이 중복된 경우 더 앞의 인덱스가 답이므로 중복 값 체크해봐야 한다
- https://www.acmicpc.net/problem/20551

---

```java
package baekjoon;

import java.io.*;
import java.util.*;

public class Baekjoon20551_Sort마스터배지훈의후계자 {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        int N = Integer.parseInt(st.nextToken());
        int M = Integer.parseInt(st.nextToken());
        StringBuilder sb = new StringBuilder();

        int[] nums = new int[N];
        for (int i = 0; i < N; i++) {
            nums[i] = Integer.parseInt(br.readLine());
        }
        Arrays.sort(nums);

        for (int m = 0; m < M; m++) {
            int num = Integer.parseInt(br.readLine());
            int left = 0;
            int right = N - 1;
            int idx = -1;
            while (left <= right) {
                int mid = left + (right - left) / 2;

                if (nums[mid] > num) {
                    right = mid - 1;
                } else if (nums[mid] < num) {
                    left = mid + 1;
                } else {
                    // mid 가 정답인 경우 중복 숫자가 있을 수 있다. 앞의 숫자를 확인하면서 중복이면 idx 갱신
                    idx = mid;
                    while (idx >= 1 && nums[idx - 1] == num) {
                        idx--;
                    }
                    break;
                }
            }

            sb.append(idx).append("\n");
        }

        System.out.println(sb);
    }
}

```