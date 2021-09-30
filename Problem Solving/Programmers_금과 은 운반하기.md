## [금과 은 운반하기]

- 도시의 개수가 10만이므로 최대 O(NlogN)의 시간복잡도로 해결해야 한다
- a, b는 최대 10억이다
- Binary Search로 진행해보자
- 금과 은의 합은 이분탐색으로 해결할 수 있다
- 합의 최소시간을 구했을 때 a, b 중 하나가 더 적으면 시간을 늘려서 탐색하기!
- 처음엔 t시간 그 다음엔 3t 5t 마다 광물 운반이 가능하다
- mid 시간동안 운반 횟수  = (mid - t) / 2t + 1
- right를 Long.MAX_VALUE로 설정하면 오류가 난다 
- https://programmers.co.kr/learn/courses/30/lessons/86053

---

```java
import java.util.*;

/*

*/

class Solution {
    public long solution(int a, int b, int[] g, int[] s, int[] w, int[] t) {
        long answer = Long.MAX_VALUE;

        // 1. 각 시간마다 금과 은의 합을 얼마나 받아올 수 있는지 체크
        int N = g.length;

        long left = 0;
        long right = Long.MAX_VALUE / 1000;
        long sum;
        while (left <= right) {                        
            long mid = left + (right - left) / 2;

            sum = 0;

            // mid 시간동안 은과 금의 합
            for (int i = 0; i < N; i++) {
                long count = (mid - t[i]) / (2 * t[i]) + 1;
                sum += (count * w[i] > g[i] + s[i]) ? g[i] + s[i] : count * w[i];

                if (sum > a + b) {
                    break;
                }
            }

            if (sum < a + b) {
                left = mid + 1;
            } else {
                // 해당 시간에 a + b는 맞는 것을 확인했지만 해당 시간 내에 a나 b를 다 못 가져오는 경우가 생길 수 있다
                // 해당 시간동안 가져올 수 있는 최댓값을 비교하여 a, b보다 작으면 시간을 늘린다
                long goldSum = 0;                
                long silverSum = 0;

                for (int i = 0; i < N; i++) {
                    long count = (mid - t[i]) / (2 * t[i]) + 1;
                    goldSum += (count * w[i] > g[i]) ? g[i] : count * w[i];
                    silverSum += (count * w[i] > s[i]) ? s[i] : count * w[i];
                }                

                if (goldSum < a || silverSum < b) {
                    left = mid + 1;                    
                } else {
                    answer = Math.min(answer, mid);                       
                    right = mid - 1;    
                }              
            }

        }

        return answer;
    }
}
```