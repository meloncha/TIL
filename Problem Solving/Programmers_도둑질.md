## [도둑질]



- 인접한 두 집을 털면 경보가 울린다. 원형으로 이루어져있다.
- 도둑이 훔칠 수 있는 돈의 최댓값을 return 하라
- 3 <= N <= 100만, money 배열의 각 원소는 최대 1000
- N이 100만 이므로 O(N)으로 해결해야한다. 
- 100만 * 1000 = 10억으로 모든 돈의 합이 *int*형을 벗어나지 않는다



---



- DP로 해결해야 하는데, 0번 집을 털고 N-1번 집을 털면 서로 인접하여 경보가 울린다
- 0번 집을 터는 경우와 0번 집을 안 터는 경우를 고려하여 DP를 계산하자
- https://programmers.co.kr/learn/courses/30/lessons/42897



```java
import java.util.*;

class Solution {
    
    public int solution(int[] money) {
        int answer = 0;
        int N = money.length;
          
        // 0: 0번 집 안 터는 경우
        // 1: 0번 집 터는 경우
        int[][] dp = new int[N][2];
        dp[0][1] = money[0];
        dp[1][0] = money[1];
        dp[1][1] = dp[0][1];
        
        for (int i = 2; i < N; i++) {
            for (int j = 0; j < 2; j++) {
                dp[i][j] = Math.max(dp[i - 1][j], dp[i - 2][j] + money[i]);
            }
        }        
        
        return Math.max(dp[N - 1][0], dp[N - 2][1]);
    }
}
```

