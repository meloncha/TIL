## [베스트셀러]

- 단순한 해시 맵 문제
- https://www.acmicpc.net/problem/1302

---

```java
package baekjoon;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayList;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

public class Baekjoon1302_베스트셀러 {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        Map<String, Integer> map = new HashMap<>();
        for (int i = 0; i < N; i++) {
            String title = br.readLine();
            int count = map.getOrDefault(title, 0);
            map.put(title, count + 1);
        }

        int maxCount = 0;
        for (int count : map.values()) {
            maxCount = Math.max(count, maxCount);
        }

        List<String> result = new ArrayList<>();
        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            if (entry.getValue() == maxCount) {
                result.add(entry.getKey());
            }
        }

        result.sort(String::compareTo);
        System.out.println(result.get(0));
    }
}

```