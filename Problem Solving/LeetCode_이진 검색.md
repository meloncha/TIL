## [이진 검색]

- 정렬된 nums를 입력받아 이진 검색으로 target에 해당하는 인덱스를 찾아라
- 기본적인 binary search 알고리즘
- left + right / 2 는 위험성이 있는 코드이다. 둘이 더했을 때 int형 범위를 넘어갈 수 있다.
- left + (right - left) / 2 로 변경한다면 문제가 생길 여지가 없다!
- https://leetcode.com/problems/binary-search/

---

```java
class Solution {
    
    public int search(int[] nums, int target) {
        int ans = 0;
        // 1. 재귀 풀이
        ans = dfs(nums, target, 0, nums.length - 1);
        
        // 2. 반복문 풀이
        // ans = solve(nums, target);
        
        return ans;
    }
    
    private int dfs(int[] nums, int target, int left, int right) {        
        if (left <= right) {
            int mid = left + (right - left) / 2;
            
            if (nums[mid] < target) {
                return dfs(nums, target, mid + 1, right);
            } else if (nums[mid] > target) {
                return dfs(nums, target, left, mid - 1);
            } else {
                return mid;
            }
        } else {
            return -1;
        }        
    }
    
    private int solve(int[] nums, int target) {
        int left = 0;
        int right = nums.length - 1;
        
        while (left <= right) {
            int mid = left + (right - left) / 2;
            
            if (nums[mid] < target) {
                left = mid + 1;
            } else if (nums[mid] > target) {
                right = mid - 1;
            } else {
                return mid;
            }
        }
        return -1;        
    }
}
```