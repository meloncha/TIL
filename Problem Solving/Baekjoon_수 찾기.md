# [숫자 찾기]

- N 최대 20만, M 최대 20만으로 단순 O(NM) 으로는 풀 수 없는 문제
- A 배열을 정렬한 뒤 이분탐색으로 확인하기
- https://www.acmicpc.net/problem/1920

---

```java
package baekjoon;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;

public class Baekjoon1920_수찾기 {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        int[] A = Arrays.stream(br.readLine().split(" ")).mapToInt(Integer::parseInt).sorted().toArray();

        int M = Integer.parseInt(br.readLine());
        int[] nums = Arrays.stream(br.readLine().split(" ")).mapToInt(Integer::parseInt).toArray();
        StringBuilder sb = new StringBuilder();

        for (int num : nums) {

            int left = 0;
            int right = N - 1;
            while (left <= right) {
                int mid = left + (right - left) / 2;

                if (A[mid] > num) {
                    right = mid - 1;
                } else if (A[mid] < num) {
                    left = mid + 1;
                } else {
                    break;
                }
            }

            // 끝까지 돌았다면 0 아니면 답을 찾았으므로 1
            sb.append(left > right ? 0 : 1).append("\n");
        }

        System.out.println(sb);
    }
}

```
