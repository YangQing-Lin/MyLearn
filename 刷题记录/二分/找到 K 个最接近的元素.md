#### <a href="https://leetcode.cn/problems/find-k-closest-elements/">找到 K 个最接近的元素</a>

-------

##### 排序

```java
class Solution {
    public List<Integer> findClosestElements(int[] arr, int k, int x) {
        List<Integer> list = new ArrayList<Integer>();
        for (int num : arr) {
            list.add(num);
        }
        Collections.sort(list, (a, b) -> {
            if (Math.abs(a - x) != Math.abs(b - x)) {
                return Math.abs(a - x) - Math.abs(b - x);
            } else {
                return a - b;
            }
        });
        List<Integer> ans = list.subList(0, k);
        Collections.sort(ans);
        return ans;
    }
}
```

##### 二分＋双指针

```java
class Solution {
    public List<Integer> findClosestElements(int[] arr, int k, int x) {
        int l = 0, r = arr.length - 1;
        while (l < r) {
            int mid = l + r >> 1;
            if (arr[mid] >= x) {
                r = mid;
            } else {
                l = mid + 1;
            }
        }
        int right = l;
        int left = l - 1;

        while (k -- > 0) {
            if (left < 0) {
                right ++;
            } else if (right >= arr.length) {
                left --;
            } else if (x - arr[left] <= arr[right] - x) {
                left --;
            } else {
                right ++;
            }
        }
        
        List<Integer> list = new ArrayList<>();
        for (int i = left + 1; i < right; i ++) {
            list.add(arr[i]);
        }
        return list;
    }
}
```

