## 자신을 제외한 배열의 곱
- 배열을 입력받아 answer[i] 가 자신을 제외한 나머지 모든 요소의 곱셈 결과가 되도록 출력하라.
- 시간 복잡도는 O(N)이어야 한다, 나눗셈을 하면 안된다.
- 나눗셈을 안하려면 현재 인덱스의 왼쪽의 전체 곱과 오른쪽의 전체 곱을 곱해야 한다.

---

1. 왼쪽 전체 곱을 정답 배열에 넣기
2. 오른쪽 전체 곱을 정답 배열에 넣기

---

- 발상의 전환으로 나눗셈 없이 구현하는 것이 포인트

```java
class Solution {
    
    public int[] productExceptSelf(int[] nums) {
        int N = nums.length;
        int[] answer = new int[N];
        Arrays.fill(answer, 1);
        
        // 1. 왼쪽 전체 곱을 정답 배열에 넣기
        int mul = 1;
        for (int i = 0; i < N; i++) {
            answer[i] *= mul;
            mul *= nums[i];
        }
        
        // 2. 오른쪽 전체 곱을 정답 배열에 넣기
        mul = 1;
        for (int i = N - 1; i >= 0; i--) {
            answer[i] *= mul;
            mul *= nums[i];
        }
        
        return answer;
    }
}
```