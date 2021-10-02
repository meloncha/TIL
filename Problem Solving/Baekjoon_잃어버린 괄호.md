## [잃어버린 괄호]

- 괄호를 적절히 넣어서 최소로 만드는 문제
- 연산자의 순서를 내 마음대로 할 수 있다
- 길이 최대 50이므로 숫자 최대 25개, 연산자 24개 인경우 완전탐색으로 순서를 정하면 24! => 불가능
- 그리디 : 최솟값이 되기 위해서는 최대한 덧셈을 많이 해서 빼야 한다
- https://www.acmicpc.net/problem/1541

---

- 정규식을 활용한 split 메소드를 잘 사용하면 쉽게 풀 수 있다

```java
package baekjoon;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;

public class Baekjoon1541_잃어버린괄호 {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        // - 를 기준으로 나누었다. 0번 string 뺀 나머지는 -이다
        String[] minusSplit = br.readLine().split("-");

        int answer = 0;
        for (int i = 0; i < minusSplit.length; i++) {
            // + 기준으로 나누어서 숫자들로 변환시킨 뒤 다 더한다
            int sum = Arrays.stream(minusSplit[i].split("\\+"))
                    .mapToInt(Integer::parseInt).sum();

            answer += i == 0 ? sum : sum * -1;
        }

        System.out.println(answer);
    }

}

```