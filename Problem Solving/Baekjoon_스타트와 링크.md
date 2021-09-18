## [스타트와 링크]
1. N 이 MAX 20 이다. 20C10 = 184756 이므로 완전탐색가능한 숫자    
2. 각 시너지의 최댓값은 100이므로 각 팀의 점수의 합은 최대 10명 * 10명 * 100 = 10000이다.
3. 조합으로 10명을 고른 뒤 계산하여 값을 비교하자

---

- DFS 에서 N - idx < count 인 경우 볼 필요 없이 안된다 => 최적화(가지치기)
- 전체 시간 복잡도 = 재귀(2^N) * 배열순회(N^2) = O(2^N * N^2)
- https://www.acmicpc.net/problem/14889

---

- 조합을 구현할 수 있으면 쉽게 풀 수 있는 문제
- 양 팀의 인원수가 같아야 하므로 한 쪽에서 더 많은 인원이 속하지 않도록 주의해야한다


```java
package baekjoon;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class BOJ14889_스타트와링크 {

    static int N, answer;
    static int[][] map;

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        answer = Integer.MAX_VALUE;
        N = Integer.parseInt(br.readLine());
        map = new int[N][N];
        StringTokenizer st;
        for (int i = 0; i < N; i++) {
            st = new StringTokenizer(br.readLine());
            for (int j = 0; j < N; j++) {
                map[i][j] = Integer.parseInt(st.nextToken());
            }
        }

        // 0 ~ N 중에 N/2 만큼 뽑자 => boolean 배열 사용하기
        dfs(0, N/2, new boolean[N]);

        System.out.println(answer);
    }

    private static void dfs(int idx, int count, boolean[] check) {
        // 가지치기
        if (N - idx < count) {
            return;
        }

        if (idx == N) {
            calc(check);
            return;
        }

        if (count > 0) {
            check[idx] = true;
            dfs(idx + 1, count - 1, check);
            check[idx] = false;
        }
        dfs(idx + 1, count, check);
    }

    private static void calc(boolean[] check) {
        int tempAnswer = 0;

        for (int i = 0; i < N; i++) {
            for (int j = 0; j < N; j++) {
                // 1. check 가 true 인 팀의 능력치를 합하기
                if (check[i] && check[j]) {
                    tempAnswer += map[i][j];
                }

                // 2. check 가 false 인 팀의 능력치를 빼기
                if (!check[i] && !check[j]) {
                    tempAnswer -= map[i][j];
                }
            }
        }

        answer = Math.min(Math.abs(tempAnswer), answer);
    }
}
```