## [최단경로]

- 다익스트라 알고리즘 문제
- https://www.acmicpc.net/problem/1753

---

```java
package baekjoon;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Baekjoon1753_최단경로 {

    static int V, E;
    static StringBuilder sb = new StringBuilder();
    static List<List<Node>> graph;

    static class Node {
        int to, weight;

        public Node(int to, int weight) {
            this.to = to;
            this.weight = weight;
        }
    }

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st = new StringTokenizer(br.readLine());
        V = Integer.parseInt(st.nextToken());
        E = Integer.parseInt(st.nextToken());
        int start = Integer.parseInt(br.readLine());

        graph = new ArrayList<>();
        for (int i = 0; i <= V; i++) {
            graph.add(new ArrayList<>());
        }

        for (int i = 0; i < E; i++) {
            st = new StringTokenizer(br.readLine());
            int u = Integer.parseInt(st.nextToken());
            int v = Integer.parseInt(st.nextToken());
            int w = Integer.parseInt(st.nextToken());

            graph.get(u).add(new Node(v, w));
        }

        dijkstra(start);

        System.out.println(sb);
    }

    private static void dijkstra(int start) {
        PriorityQueue<Node> pq = new PriorityQueue<>(Comparator.comparingInt(o -> o.weight));
        int INF = Integer.MAX_VALUE / 10;
        int[] dist = new int[V + 1];
        Arrays.fill(dist, INF);
        dist[start] = 0;
        pq.add(new Node(start, 0));

        while (!pq.isEmpty()) {
            Node now = pq.poll();

            if (dist[now.to] < now.weight) {
                continue;
            }

            for (int i = 0; i < graph.get(now.to).size(); i++) {
                Node next = graph.get(now.to).get(i);

                if (dist[next.to] > now.weight + next.weight) {
                    dist[next.to] = now.weight + next.weight;
                    pq.add(new Node(next.to, dist[next.to]));
                }
            }
        }

        for (int i = 1; i <= V; i++) {
            String ans = dist[i] == INF ? "INF" : dist[i] + "";
            sb.append(ans).append("\n");
        }
    }
}

```