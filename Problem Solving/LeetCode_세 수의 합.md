## [세 수의 합]

- 배열을 입력받아 합으로 0을 만들 수 있는 3개의 value를 모두 출력하라. 
- 정답은 중복이 되어선 안된다
- https://leetcode.com/problems/3sum/

---

- 단순히 생각하면 3중 for문을 통한 O(N^3)의 시간복잡도로 풀 수 있을 것 같다
- 하지만 N의 max가 3000이기 때문에 N^3은 90억으로 불가능하다
- 두 수의 합에서 O(N^2)을 투포인터를 이용하여 O(N)으로 줄인 기술을 활용해보자
- 인덱스 하나를 정해놓고 나머지 2개를 투포인터로 접근하면 인덱스 순회 N * 투포인터 N => O(N^2)으로 해결할 수 있다

---

1. 투포인터를 활용하기 위해 주어진 배열을 정렬한다
2. 인덱스 하나는 고정한 상태에서 다른 두 인덱스의 값과 계산한다
3. 중복을 제거해야 하므로 이전에 계산한 값과 현재의 값이 같다면 다음으로 넘긴다
4. 정답이 나오는 경우 왼쪽, 오른쪽 인덱스를 둘 다 변경시켜야 한다. 하나만 변경시키는 경우에는 절대로 0이 다시 나올 수 없다. 

---

```java
class Solution {
    
    public List<List<Integer>> threeSum(int[] nums) {
        List<List<Integer>> answer = new ArrayList<>();
        
        // 1. 배열을 정렬한다
        Arrays.sort(nums);
        
        // 2. 숫자를 하나 정해서 그 숫자를 제외한 나머지로 투포인터 연산을 해보자
        // i의 범위는 i, lIdx, rIdx을 표현할 수 있어야 하므로 전체 길이 - 2를 제한으로
        for (int i = 0; i < nums.length - 2; i++) {
            // 중복을 제거하려면 i가 중복이 안되어야 한다
            // 이전 값과 현재 값이 같으면 중복될 가능성이 생긴다 => 넘기기
            if (i > 0 && nums[i] == nums[i - 1]) {
                continue;
            }
            
            // lIdx는 i + 1, rIdx는 배열의 맨 마지막 인덱스로 하자
            int lIdx = i + 1;
            int rIdx = nums.length - 1;
            
            while (lIdx < rIdx) {
                int temp = nums[i] + nums[lIdx] + nums[rIdx];
                
                if (temp < 0) { 
                    // 합이 0보다 작으면 lIdx를 증가시켜서 합을 증가시킨다
                    lIdx++;
                } else if (temp > 0) {
                    // 합이 0보다 크면 rIdx를 감소시켜서 합을 감소시킨다
                    rIdx--;
                } else {
                    // 정답배열에 넣기
                    answer.add(Arrays.asList(nums[i], nums[lIdx], nums[rIdx]));
                    
                    // 중복되는 값의 경우 넘어가기
                    while (lIdx < rIdx && nums[lIdx] == nums[lIdx + 1]) {
                        lIdx++;
                    }
                    while (lIdx < rIdx && nums[rIdx] == nums[rIdx - 1]) {
                        rIdx--;
                    }
                    
                    // left, right 둘 다 변경시키기
                    lIdx++;
                    rIdx--;
                }
            }  
        }
        return answer;
    }
}

```