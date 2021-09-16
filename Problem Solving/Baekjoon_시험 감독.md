## [시험 감독]

- 백준 13458번
- 시험장의 개수 N : 100만
- 각 응시자의 수 A : 100만
- 시간 복잡도 최대 O(N)
- B, C가 1명 감독 가능하다면 최악의 경우 100만 * 100만 => 1조명 (*long 타입*) 

---
- 문제는 쉬웠으나 정답의 범위가 int형을 벗어나는 것을 주의해야 하는 문제
- 항상 정답의 범위가 얼마나 되는지도 확인해야한다는 것을 깨닫게 해주는 문제


```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.StringTokenizer;

public class BOJ13458_시험감독 {

    /*
    1. 총감독관은 B명 감시, 부감독관은 C명 감시
    2. 시험장 개수 N (100만), 응시자수 A(100만)
    B=1, C=1 인 경우 감독관의 MAX = 100만*100만 => 1조 (Long 타입이 필수)
     */
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        int N = Integer.parseInt(br.readLine());
        int[] tests= new int[N];
        StringTokenizer st = new StringTokenizer(br.readLine());
        for (int i = 0; i < N; i++) {
            tests[i] = Integer.parseInt(st.nextToken());
        }
        st = new StringTokenizer(br.readLine());
        int B = Integer.parseInt(st.nextToken());
        int C = Integer.parseInt(st.nextToken());

        long answer = 0;
        for (int A : tests) {
            // A가 더 큰 경우 계산 결과를 더한다
            if (A > B) {
                answer += (long) Math.ceil((double) (A - B) / C);
            }
            // 총감독관 추가
            answer++;
        }

        System.out.println(answer);
    }
}
```




