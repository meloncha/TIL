# [소수 찾기]

- 소수 찾는 알고리즘을 이용한 문제

---

```java 
import java.util.*;
import java.io.*;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        br.readLine();

        long count = Arrays.stream(br.readLine().split(" "))
                .mapToInt(Integer::parseInt)
                .filter(Main::isPrime)
                .count();

        System.out.println(count);
    }

    public static boolean isPrime(int num) {
        if (num == 1) {
            return false;
        }
        
        for (int i = 2; i * i <= num; i++) {
            if (num % i == 0) {
                return false;
            }
        }
        return true;
    }
}
```