## [퇴사]

- 백준 14501번
- 각각의 상담은 완료하는 기간이 있고 상담을 했을 때 받을 수 있는 금액이 있다. 얻을 수 있는 최대 수익을 구하는 문제
- N의 최대값 : 15 => DFS 가능
- 만약 N이 커지면? => DP로 풀어야 한다

---

- 이 문제는 DFS로 풀 수 있어서 난이도가 낮은 것 같다.
- 만약 N이 커지는 경우에도 해결할 수 있도록 DP에 대한 이해도를 높이는 것이 좋아보인다.
- https://www.acmicpc.net/problem/14501

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {

    /*
    N 은 최대 15 => DFS 로 풀 수 있다.
    하지만 N 이 커지면 DP 로 풀어야 한다!
     */
    static int answer;
    
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        int[] T = new int[N];
        int[] P = new int[N];
        for (int i = 0; i < N; i++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            T[i] = Integer.parseInt(st.nextToken());
            P[i] = Integer.parseInt(st.nextToken());
        }


        // 1. DFS
//        dfs(0, 0, T, P);

        // 2. DP
        // 이전 값이 현재 값에 영향을 주는 것
        int[] dp = new int[N + 1];
        for (int i = 0; i < N; i++) {
            if (i + T[i] <= N) {
                dp[i + T[i]] = Math.max(dp[i] + P[i], dp[i + T[i]]);
            }
            if (i >= 1) {
                dp[i + 1] = Math.max(dp[i], dp[i + 1]);
            }
        }
        answer = dp[N];

        System.out.println(answer);
    }

    private static void dfs(int day, int profit, int[] t, int[] p) {
        if (day > t.length) {
            return;
        } else if (day == t.length) {
            answer = Math.max(profit, answer);
            return;
        }

        // 오늘 상담을 하는 경우
        dfs(day + t[day], profit + p[day], t, p);

        // 오늘 상담을 안하는 경우
        dfs(day + 1, profit, t, p);
    }

}


```