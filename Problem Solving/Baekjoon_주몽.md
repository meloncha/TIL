# [주몽]

- 재료의 개수 N : 15000, 갑옷을 만드는데 필요한 수 M : 1000만
- 정렬 시킨 후 가장 왼쪽, 가장 오른쪽 원소 둘의 합이 M이 되는 값을 찾아보기
- 둘의 합이 M 보다 크면 오른쪽 인덱스를 줄여서 다시 비교
- 둘의 합이 M 보다 작으면 왼쪽 인덱스를 늘려서 다시 비교
- 합이 M 이면 left, right 둘다 변화시킨다
- 고유 번호가 존재하기 때문에 중복되는 번호는 없다고 보면 된다

---

```java
import java.io.*;
import java.util.*;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        int M = Integer.parseInt(br.readLine());

        int[] nums = Arrays.stream(br.readLine().split(" ")).mapToInt(Integer::parseInt).sorted().toArray();

        int count = 0;

        int left = 0;
        int right = N - 1;
        while (left < right) {
            int sum = nums[left] + nums[right];

            if (sum > M) {
                right--;
            } else if (sum < M) {
                left++;
            } else {
                count++;
                left++;
                right--;
            }
        }

        System.out.println(count);
    }
}

```