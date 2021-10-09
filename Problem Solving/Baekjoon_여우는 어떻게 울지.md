# [여우는 어떻게 울지?]

- 울음소리 배열에서 각 동물마다의 울음소리를 제외하면 울음소리N 동물울음소리M => O(NM)이 된다
- 동물울음소리를 먼저 Set에 넣고 울음소리 배열에서 Set에 들어있는지 체크하면 => O(N + M)이 된다

```java
import java.io.*;
import java.util.*;

public class Main {

    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int T = Integer.parseInt(br.readLine());
        StringBuilder sb = new StringBuilder();
        for (int t = 0; t < T; t++) {
            String[] split = br.readLine().split(" ");

            Set<String> set = new HashSet<>();
            String[] strings = br.readLine().split(" ");
            while (strings[1].equals("goes")) {
                set.add(strings[2]);
                strings = br.readLine().split(" ");
            }

            for (int i = 0; i < split.length; i++) {
                String str = split[i];
                if (!set.contains(str)) {
                    sb.append(str).append(" ");
                }
            }
            sb.append("\n");
        }

        System.out.println(sb);
    }
}

```