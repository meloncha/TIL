## [팰린드롬 만들기]



- 팰린드롬을 만들기 위해서는 문자열의 맨 앞과 맨 뒤에 같은 문자를 추가해야 한다
- 문자 갯수가 홀수일 때는 짝 안 맞는 문자 하나를 가운데에 넣어야하고
- 짝수일 때는 짝 안 맞는 문자가 생기면 안된다. 짝수이므로 안 맞는 문자는 최소 2개
- 짝이 안 맞는 문자는 하나만 가능하고, 그 문자를 가운데에 넣고 시작한다
- 맨 앞과 맨 뒤에 넣는 자료구조인 `Deque`을 사용하자

- https://www.acmicpc.net/problem/1213



---



```java
package baekjoon;

import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.ArrayDeque;
import java.util.Deque;

public class Baekjoon1213_팰린드롬만들기 {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));

        String str = br.readLine();

        // 1. 문자 갯수 확인하기
        int[] alphabets = new int[26];
        for (int i = 0; i < str.length(); i++) {
            alphabets[str.charAt(i) - 'A']++;
        }

        // 2. 짝 안 맞는 문자 확인하기
        int count = 0;
        int idx = -1;
        for (int i = 0; i < alphabets.length; i++) {
            if (alphabets[i] % 2 == 1) {
                count++;
                idx = i;
            }
        }

        // 3. 짝 안 맞는 문자가 2개 이상이면 불가능
        if (count > 1) {
            System.out.println("I'm Sorry Hansoo");
        } else  {
            Deque<Character> deque = new ArrayDeque<>();

            // 홀수인 경우 홀수를 가운데에 먼저 넣기
            if (count == 1) {
                deque.add((char) ('A' + idx));
            }

            // Z부터 채우면 맨 앞은 A가 되어서 사전순으로 가장 앞선다
            for (int i = alphabets.length - 1; i >= 0; i--) {
                while (alphabets[i] >= 2) {
                    alphabets[i] -= 2;
                    deque.addFirst((char) ('A' + i));
                    deque.addLast((char) ('A' + i));
                }
            }

            StringBuilder sb = new StringBuilder();
            while (!deque.isEmpty()) {
                sb.append(deque.pop());
            }
            System.out.println(sb);
        }

    }
}

```

