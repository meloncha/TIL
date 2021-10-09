# [숫자카드]

- N 최대 50만, M 최대 50만
- M 번 돌면서 N 배열에 있는지 여부를 판단해야 한다
    1. 해시
    2. 이분탐색

---

```java
package baekjoon;

import java.io.*;
import java.util.*;

public class Baekjoon10815_숫자카드 {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        int[] nums = Arrays.stream(br.readLine().split(" ")).mapToInt(Integer::parseInt).sorted().toArray();
        int M = Integer.parseInt(br.readLine());
        int[] inputs = Arrays.stream(br.readLine().split(" ")).mapToInt(Integer::parseInt).toArray();

        String answer = "";

        // 1. 해시
        answer = hash(nums,inputs);

        // 2. 이분탐색
//        answer = binarySearch(nums, inputs);



        System.out.println(answer);
    }

    private static String hash(int[] nums, int[] inputs) {
        Set<Integer> set = new HashSet<>();
        for (int n : nums) {
            set.add(n);
        }

        StringBuilder sb = new StringBuilder();
        for (int input : inputs) {
             if (set.contains(input)) {
                 sb.append(1);
             } else {
                 sb.append(0);
             }
             sb.append(" ");
        }

        return sb.toString();
    }

    private static String binarySearch(int[] nums, int[] inputs) {
        StringBuilder sb = new StringBuilder();

        for (int input : inputs) {
            int left = 0;
            int right = nums.length - 1;
            boolean isMatch = false;

            while (left <= right) {
                int mid = left + (right - left) / 2;

                if (input > nums[mid]) {
                    left = mid + 1;
                } else if (input < nums[mid]){
                    right = mid - 1;
                } else {
                    isMatch = true;
                    break;
                }
            }

            sb.append(isMatch ? 1 : 0).append(" ");
        }
        return sb.toString();
    }
}

```