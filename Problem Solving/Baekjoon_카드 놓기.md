# [카드 놓기]

1. 제일 위의 카드 1장을 내려놓는다
2. 카드 2장 이상일 때 위에더 두번째 카드를 바닥에 내려놓는다
3. 카드 2장 이상일 때 제일 밑에 있는 카드를 바닥에 내려놓는다
처음의 상태를 출력하기

---

- 처음 숫자 배열을 만들고 left, right 인덱스로 전체 크기를 조절하자 => 실패
- N 최대 100만 
- left, right 인덱스로 탐색시 시간초과
- 덱 자료구조를 사용하자
- https://www.acmicpc.net/problem/18115

---

```java
package baekjoon;

import java.util.*;
import java.io.*;

public class Baekjoon18115_카드놓기 {

    /*
    
     */
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        int[] commands = Arrays.stream(br.readLine().split(" ")).mapToInt(Integer::parseInt).toArray();

        Deque<Integer> deque = new ArrayDeque<>();
        for (int i = N - 1; i >= 0; i--) {
            int num = N - i;
            int command = commands[i];

            if (command == 1) { // 덱의 앞에 삽입
                deque.addFirst(num);
            } else if (command == 3) { // 덱의 뒤에 삽입
                deque.addLast(num);
            } else { // 앞에서 두번째에 삽입해야 하므로 하나 빼고 다시 넣기
                int temp = deque.pollFirst();
                deque.addFirst(num);
                deque.addFirst(temp);
            }
        }

        StringBuilder sb = new StringBuilder();
        for (int num : deque) {
            sb.append(num).append(" ");
        }
        System.out.println(sb);

    }
}

```