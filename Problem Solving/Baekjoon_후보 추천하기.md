## [후보 추천하기]

- 주어진 조건에 맞추어 구현하는 문제
- https://www.acmicpc.net/problem/1713

---

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.*;

public class Main {

    static class Candidate {
        int num, time, count; // time 은 사진틀에 게시된 시각,  count 는 추천 수

        public Candidate(int num, int time, int count) {
            this.num = num;
            this.time = time;
            this.count = count;
        }
    }

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        int M = Integer.parseInt(br.readLine()); // 사용 안함
        int[] votes = Arrays.stream(br.readLine().split(" ")).mapToInt(Integer::parseInt).toArray();

        List<Candidate> candidates = new ArrayList<>();
        for (int i = 0; i < M; i++) {

            // 사진틀에 없는 경우에 진행
            if (!findCandidate(candidates, votes[i])) {

                // 사이즈가 같을 때에는 삭제하기
                if (candidates.size() == N) {
                    removeCandidate(candidates);
                }

                // 후보 추가하기
                candidates.add(new Candidate(votes[i], i, 1));
            }
        }

        StringBuilder sb = new StringBuilder();
        candidates.stream()
            .map(candidate -> candidate.num)
            .sorted()
            .forEach(n -> sb.append(n).append(" "));
        System.out.println(sb);
    }

    private static void removeCandidate(List<Candidate> candidates) {
        int minCount = candidates.get(0).count;
        int minTime = candidates.get(0).time;
        int idx = 0;
        for (int i = 0; i < candidates.size(); i++) {
            Candidate candidate = candidates.get(i);
            if (candidate.count < minCount) {
                minCount = candidate.count;
                minTime = candidate.time;
                idx = i;
            } else if (candidate.count == minCount && minTime > candidate.time) {
                minTime = candidate.time;
                idx = i;
            }
        }
        candidates.remove(idx);
    }

    private static boolean findCandidate(List<Candidate> candidates, int vote) {
        for (Candidate candidate : candidates) {
            if (candidate.num == vote) {
                candidate.count++;
                return true;
            }
        }
        return false;
    }
}

```