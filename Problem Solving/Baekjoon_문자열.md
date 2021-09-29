## [문자열]

- 단순한 이중 for문 문제
- https://www.acmicpc.net/problem/1120

---

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String[] splits = br.readLine().split(" ");
        String A = splits[0];
        String B = splits[1];

        int min = Integer.MAX_VALUE;
        for (int i = 0; i <= B.length() - A.length(); i++) {
            int tempMin = 0;
            for (int j = 0; j < A.length(); j++) {
                if (A.charAt(j) != B.charAt(i + j)) {
                    tempMin++;
                }
            }
            min = Math.min(tempMin, min);
        }

        System.out.println(min);
    }
}
```