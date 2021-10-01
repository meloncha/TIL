## [ACM Craft]



- 특정 건물을 가장 빨리 짓는 시간을 알아내기
- 이전 건물의 걸린 시간에 추가로 현재 건물 시간을 더해야 하기 때문에 ***DP***를 사용하자
- 최소 시간이기 때문에 `Math.min`을 사용해야 한다고 생각할 수 있으나 현재 건물을 짓는 데에 2개 이상의 건물이 필요한 경우 가장 늦게 지어진 건물에 현재 건물을 짓는 시간을 더해야 하므로 `Math.max`로 연산한다
- DP는 순서가 중요한데, 건물을 짓는데 영향을 받지 않는 건물부터 계산해야 하기 때문에 ***위상정렬***을 사용한다 (진입 차수가 0인 건물부터 DP 계산)

- https://www.acmicpc.net/problem/1005



---



```java
package baekjoon;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Baekjoon1005_ACMCraft {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int T = Integer.parseInt(br.readLine());
        StringBuilder sb = new StringBuilder();
        for (int t = 0; t < T; t++) {
            StringTokenizer st = new StringTokenizer(br.readLine());
            int N = Integer.parseInt(st.nextToken());
            int K = Integer.parseInt(st.nextToken());
            int[] D = Arrays.stream(br.readLine().split(" ")).mapToInt(Integer::parseInt).toArray();

            List<List<Integer>> graph = new ArrayList<>();
            for (int i = 0; i < N; i++) {
                graph.add(new ArrayList<>());
            }

            int[] indegree = new int[N];

            for (int k = 0; k < K; k++) {
                st = new StringTokenizer(br.readLine());
                int from = Integer.parseInt(st.nextToken()) - 1;
                int to = Integer.parseInt(st.nextToken()) - 1;

                // 진입 차수 계산하기
                indegree[to]++;

                // 그래프 연결
                graph.get(from).add(to);
            }

            // 위상정렬하기
            int[] order = topologySort(graph, indegree);

            int[] dp = new int[N];
            for (int i = 0; i < N; i++) {
                dp[i] = D[i];
            }

            // order 대로 DP 작업
            for (int from : order) {
                for (int to : graph.get(from)) {
                    dp[to] = Math.max(dp[to], dp[from] + D[to]);
                }
            }

            int W = Integer.parseInt(br.readLine());
            sb.append(dp[W - 1]).append("\n");
        }

        System.out.println(sb);
    }

    private static int[] topologySort(List<List<Integer>> graph, int[] indegree) {
        List<Integer> result = new ArrayList<>();
        Queue<Integer> queue = new LinkedList<>();

        // 1. 진입차수 0인 노드 큐에 삽입하기
        for (int i = 0; i < indegree.length; i++) {
            if (indegree[i] == 0) {
                queue.add(i);
            }
        }

        // 2. 큐에서 값을 꺼내면서 해당 노드가 가리키는 노드의 진입차수를 1 감소
        while (!queue.isEmpty()) {
            int now = queue.poll();
            result.add(now);

            for (int i = 0; i < graph.get(now).size(); i++) {
                int next = graph.get(now).get(i);
                indegree[next]--;

                if (indegree[next] == 0) {
                    queue.add(next);
                }
            }
        }

        return result.stream().mapToInt(v -> v).toArray();
    }
}

```

