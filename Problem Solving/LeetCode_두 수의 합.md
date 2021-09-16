## [두 수의 합]

- 배열이 주어지고 배열의 숫자 2개의 합이 target과 같은 경우 index를 리턴하는 단순한 문제
- 세 가지의 방법으로 문제를 해결할 수 있다.
---
1. 브루트포스
- 단순한 2중 for문으로 전부 더해봐서 비교 
- 시간복잡도 O(N^2)

2. 투포인터
- 원래 배열의 인덱스를 저장하는 map을 만든다
- 배열을 정렬한다
- 맨 앞, 맨 뒤 인덱스를 포인터로 삼아 합이 target보다 작으면 왼쪽을 증가시키고 크면 오른쪽을 감소시켜 최종적으로 같아질 때까지 반복한다
- 배열 정렬에 O(NlogN) + 투포인터 연산에 O(N) => O(NlogN)

3. 한 수를 뺀 결과를 map에서 조회
- 배열의 값과 인덱스를 map에 저장한다
- 값이 같은 경우 인덱스는 덮어씌운다. 왜냐하면 문제에서 같은 수는 최대 2개임을 조건을 통해 보장하였고, 앞에서부터 탐색하면 문제가 없다
- 배열을 순회하면서 map에서 키를 조회하는 방식이므로 O(N)으로 해결
---

- O(N^2) => O(NlogN) => O(N) 으로 고민을 하며 효율적으로 풀 수 있는 좋은 문제
- https://leetcode.com/problems/two-sum

```java
class Solution {
    
    // 덧셈하여 target을 만들 수 있는 배열의 두 숫자 인덱스를 리턴하기
    public int[] twoSum(int[] nums, int target) {
        int[] answer = new int[2];
        
        // 1. brute-force
        // 시간 복잡도 O(N^2) => N이 10000까지 가능
        // answer = solve1(nums, target);

        // 2. 정렬 후 앞에서부터 탐색하면 되지 않을까?
        // 시간 복잡도 O(NlogN) 정렬 NlogN, 탐색 N 
        // => 인덱스가 같이 정렬되므로 처음의 상태를 저장해야 한다
        // answer = solve2(nums, target);
        
        // 3. map이용하기 
        answer = solve3(nums, target);
        
        return answer;
    }
    
    private int[] solve1(int[] nums, int target) {
        int[] result = new int[2];
        
        for (int i = 0; i < nums.length - 1; i++) {
            for (int j = i + 1; j < nums.length; j++) {
                if (nums[i] + nums[j] == target) {
                    result[0] = i;
                    result[1] = j;
                    return result;
                }        
            }
        }
        return result;
    }
    
    private int[] solve2(int[] nums, int target) {
        int[] result = new int[2];
        // 값에 해당하는 인덱스 저장
        // 값이 같은 경우 문제가 생길 수 있다. => 리스트
        Map<Integer, List<Integer>> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            List<Integer> value = map.getOrDefault(nums[i], new ArrayList<>());
            value.add(i);
            map.put(nums[i], value);
        }
        
        
        Arrays.sort(nums); // 오름차순 정렬
        
        int lIdx = 0;
        int rIdx = nums.length - 1;
        
        // lIdx값 + rIdx값이 target보다 작으면 rIdx를 증가, target보다 크면 lIdx를 증가시켜서 확인
        while (lIdx < rIdx) {            
            int temp = nums[lIdx] + nums[rIdx];
            if (temp < target) {
                lIdx++;
            } else if (temp > target) {
                rIdx--;
            } else {
                // 같을 때에는 리스트 0, 1번
                if (nums[lIdx] == nums[rIdx]) {
                    result[0] = map.get(nums[lIdx]).get(0);
                    result[1] = map.get(nums[lIdx]).get(1);
                } else {
                    result[0] = map.get(nums[lIdx]).get(0);
                    result[1] = map.get(nums[rIdx]).get(0);    
                }                
                break;
            }
        }
        return result;    
    }
    
    private int[] solve3(int[] nums, int target) {        
        int[] result = new int[2];
        
        // 똑같은 수는 최대 2개라는 것이 문제에 증명되어 있다.
        // 그러므로 map의 value를 뒤에 것으로 갱신하면
        // 앞에서부터 탐색할 때에 문제가 없다!
        Map<Integer, Integer> map = new HashMap<>();
        for (int i = 0; i < nums.length; i++) {
            // 같은 값은 뒤의 인덱스로 갱신
            map.put(nums[i], i);
        }
        
        for (int i = 0; i < nums.length; i++) {
            int tempIdx = target - nums[i];
            if (map.containsKey(tempIdx) && map.get(tempIdx) != i) {
                result[0] = i;
                result[1] = map.get(tempIdx);
                break;
            }
        }
        return result;        
    }
}
```