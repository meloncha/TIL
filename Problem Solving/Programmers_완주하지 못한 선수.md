## [완주하지 못한 선수]

- 참가한 선수는 최대 10만명이므로 시간복잡도 O(N)으로 해결해야 한다
- 단순히 String 배열로 확인하려면 participant길이 N * completion 길이 N-1 으로 O(N^2)의 시간복잡도가 나온다
- 그래서 participant 배열을 map으로 바꾸어 O(1) * O(N) = O(N)으로 만들자
- 동명이인이 있을 수 있으므로 해당 이름을 가진 사람을 count하자
- https://programmers.co.kr/learn/courses/30/lessons/42576

---

```java
import java.util.*;

class Solution {
    public String solution(String[] participant, String[] completion) {
        String answer = "";
        
        Map<String, Integer> map = new HashMap<>();
        for (String name : participant) {
            int count = map.getOrDefault(name, 0);
            map.put(name, count + 1);
        }
        
        for (String name : completion) {
            // 참가자 명단에 있으면 count 하나 줄이기
            if (map.containsKey(name)) {
                map.put(name, map.get(name) - 1);
            }
        }
        
        // map의 count가 1인 name이 완주 못 한 사람
        for (Map.Entry<String, Integer> entry : map.entrySet()) {
            if (entry.getValue() == 1) {
                answer = entry.getKey();
                break;
            }
        }
        
        return answer;
    }
}
```