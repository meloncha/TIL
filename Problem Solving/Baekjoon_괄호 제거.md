## [괄호 제거]

- 괄호 쌍을 찾아서 괄호를 제거해서 나올 수 있는 서로 다른 식을 출력해야한다
- String이 겹칠 수 있으므로 List대신 Set을 사용하자
- 괄호 쌍을 stack을 사용하여 먼저 찾기
- DFS로 괄호를 제거하든지 말든지에 대해 경우를 나눈다
- https://www.acmicpc.net/problem/2800

---

```java
import java.io.*;
import java.util.*;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        String str = br.readLine();

        // 1. 괄호 쌍 찾아서 list 에 넣기
        List<int[]> pairs = new ArrayList<>();

        Stack<Integer> stack = new Stack<>();
        for (int i = 0; i < str.length(); i++) {
            char ch = str.charAt(i);

            if (ch == '(') { // 여는 괄호면 스택에 인덱스 넣기
                stack.push(i);
            } else if (ch == ')') { // 닫는 괄호면 스택에서 꺼내서 pairs 에 인덱스 넣기
                int[] pair = new int[2];
                pair[0] = stack.pop();
                pair[1] = i;
                pairs.add(pair);
            }
        }

        // 2. DFS 로 괄호 쌍 제거할지 말지에 따라 순회하기
        // String 에서 괄호를 빼면 길이가 바뀌어 인덱스가 바뀌기 때문에 char 배열로 접근한다
        // 괄호가 겹치는 경우 같은 문자열이 나올 수 있으므로 Set 을 활용하기
        Set<String> result = new HashSet<>();
        dfs(str.toCharArray(), pairs, 0, result);

        // 3. 결과 리스트 사전순으로 정렬한 뒤 출력하기, 원래 String 은 제외해줘야 한다
        StringBuilder sb = new StringBuilder();
        result.stream().filter(s -> !str.equals(s)).sorted().forEach(s -> sb.append(s).append("\n"));
        System.out.println(sb);
    }

    private static void dfs(char[] arr, List<int[]> pairs, int idx, Set<String> result) {
        if (idx == pairs.size()) {
            // 'X' 가 아닌 애들만 더해서 String 으로 만들기
            StringBuilder sb = new StringBuilder();
            for (char ch : arr) {
                if (ch != 'X') {
                    sb.append(ch);
                }
            }
            result.add(sb.toString());
            return;
        }

        int[] pair = pairs.get(idx);
        char leftBracket = arr[pair[0]];
        char rightBracket = arr[pair[1]];

        // pairs.get(idx) 에 해당하는 괄호 쌍을 제거하는 경우
        arr[pair[0]] = 'X';
        arr[pair[1]] = 'X';
        dfs(arr, pairs, idx + 1, result);
        arr[pair[0]] = leftBracket;
        arr[pair[1]] = rightBracket;

        // 제거 안하는 경우
        dfs(arr, pairs, idx + 1, result);
    }
}

```