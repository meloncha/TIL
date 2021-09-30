## [할아버지는 유명해!]

- 문제의 조건에 따라 구현하는 문제
- 1등이 아닌 2등을 찾아야 하기 때문에 그에 맞는 로직을 설계해야 한다
- https://www.acmicpc.net/problem/5766

---

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringBuilder sb = new StringBuilder();

        while (true) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            int N = Integer.parseInt(st.nextToken());
            int M = Integer.parseInt(st.nextToken());

            if (N == 0 && M == 0) {
                break;
            }

            int[] ranks = new int[10001];
            for (int i = 0; i < N; i++) {
                st = new StringTokenizer(br.readLine());
                for (int j = 0; j < M; j++) {
                    ranks[Integer.parseInt(st.nextToken())]++;
                }
            }

            int firstRank = 0;
            int secondRank = 0;

            // 2등 점수 계산
            for (int rank : ranks) {
                if (rank >= firstRank) {
                    secondRank = firstRank;
                    firstRank = rank;
                } else if (rank > secondRank) {
                    secondRank = rank;
                }
            }

            // 2등 출력
            for (int i = 0; i < ranks.length; i++) {
                if (ranks[i] == secondRank) {
                    sb.append(i).append(" ");
                }
            }
            sb.append("\n");
        }

        System.out.println(sb);

    }
}

```