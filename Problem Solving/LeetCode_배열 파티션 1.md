## [배열 파티션 1]

- 2개씩 묶어서 그 중 작은 수끼리 더 해서 만들 수 있는 가장 큰 수를 출력하라
- N = 10000 => 최대 O(N^2)
- 값이 최대로 되려면 작은 애들끼리 묶어야 한다 => 배열 정렬
- 오름차순으로 정렬되어 있다면 둘 사이의 값을 비교할 필요없이 둘중에 앞의 것을 더하면 된다  
- 배열 정렬 NlogN, 배열 순회 N => 시간 복잡도 O(NlogN) 
- https://leetcode.com/problems/array-partition-i/

---

- 문제를 이해하면 쉽게 풀리는 문제

```java
class Solution {

    public int arrayPairSum(int[] nums) {
        Arrays.sort(nums);
        
        int sum = 0;
        for (int i = 0; i < nums.length - 1; i += 2) {
            sum += nums[i];
        }
        
        return sum;
    }
}
```