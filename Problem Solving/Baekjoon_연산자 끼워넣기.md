## [연산자 끼워넣기]

- 숫자가 N개면 그 사이의 공간은 N-1개이다
- 문제에서 모든 연산 결과가 -10억과 10억 사이로 주어진다고 나와서 int 형 가능
- 만약 이런 조건이 없었다면 *long형*이 필수인 문제
- 재귀를 통해 연산자를 배치하고, 계산하기

---

- 기본적인 DFS를 활용한 문제
- https://www.acmicpc.net/problem/14888

```java
package baekjoon;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class BOJ14888_연산자끼워넣기 {

    static int N, max, min;
    static int[] operators, nums;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        max = Integer.MIN_VALUE;
        min = Integer.MAX_VALUE;
        N = Integer.parseInt(br.readLine());
        nums = new int[N];
        StringTokenizer st = new StringTokenizer(br.readLine());
        for (int i = 0; i < N; i++) {
            nums[i] = Integer.parseInt(st.nextToken());
        }
        operators = new int[4];
        st = new StringTokenizer(br.readLine());
        for (int i = 0; i < 4; i++) {
            operators[i] = Integer.parseInt(st.nextToken());
        }

        dfs(0, new int[N - 1]);

        System.out.println(max + "\n" + min);
    }

    private static void dfs(int idx, int[] select) {
        if (idx == N - 1) {
            calc(select);
            return;
        }

        for (int i = 0; i < 4; i++) {
            if (operators[i] > 0) {
                select[idx] = i;
                operators[i]--;
                dfs(idx + 1, select);
                operators[i]++;
            }
        }
    }

    private static void calc(int[] select) {
        int result = nums[0];
        for (int i = 0; i < N - 1; i++) {
            switch (select[i]) {
                case 0:
                    result += nums[i + 1];
                    break;
                case 1:
                    result -= nums[i + 1];
                    break;
                case 2:
                    result *= nums[i + 1];
                    break;
                case 3:
                    // result 를 양수로 바꾼 뒤 몫을 취하고, 그 몫을 음수로 바꾼다
                    if (result < 0 && nums[i + 1] > 0) {
                        result = ((result * -1) / nums[i + 1]) * -1;
                    } else {
                        result /= nums[i + 1];
                    }
                    break;
                default:
                    break;
            }
        }

        max = Math.max(result, max);
        min = Math.min(result, min);
    }
}

```