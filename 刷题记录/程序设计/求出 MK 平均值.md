#### <a href="https://leetcode.cn/problems/finding-mk-average/">求出 MK 平均值</a>

------------

```java
class MKAverage {

    // left 表示最小的k个数，right表示最大的k个数，mid表示剩余的数
    Range left, mid, right;
    List<Integer> list;
    int m, k;

    public MKAverage(int m, int k) {
        this.m = m; this.k = k;
        left = new Range(); mid = new Range(); right = new Range();
        list = new ArrayList<>();
    }

    public void addElement(int num) {
        list.add(num);
        if (list.size() < m) return;
        else if (list.size() == m) {
            List<Integer> tem = new ArrayList<>(list); // 注意这里不能直接赋
            Collections.sort(tem);
            for (int i = 0; i < k; i ++) left.insert(tem.get(i));
            for (int i = k; i < m - k; i ++) mid.insert(tem.get(i));
            for (int i = m - k; i < m; i ++) right.insert(tem.get(i));
        } else {
            mid.insert(num);
            if (left.treeMap.lastKey() > mid.treeMap.firstKey()) {
                int x = left.treeMap.lastKey(), y = mid.treeMap.firstKey();
                left.remove(x); mid.insert(x);
                mid.remove(y); left.insert(y);
            }
            if (mid.treeMap.lastKey() > right.treeMap.firstKey()) {
                int x = mid.treeMap.lastKey(), y = right.treeMap.firstKey();
                mid.remove(x); right.insert(x);
                right.remove(y); mid.insert(y);
            }

            num = list.get(list.size() - 1 - m);
            if (mid.treeMap.containsKey(num)) mid.remove(num);
            else if (left.treeMap.containsKey(num)) {
                left.remove(num);
                int x = mid.treeMap.firstKey();
                mid.remove(x); left.insert(x);
            } else if (right.treeMap.containsKey(num)) {
                right.remove(num);
                int x = mid.treeMap.lastKey();
                mid.remove(x); right.insert(x);
            }
        }
    }

    public int calculateMKAverage() {
        if (list.size() < m) return -1;
        return (int) (mid.sum / mid.size);
    }
}

class Range {
    TreeMap<Integer, Integer> treeMap = new TreeMap<>();
    long sum = 0;
    long size = 0;

    void insert(int x) {
        treeMap.put(x, treeMap.getOrDefault(x, 0) + 1);
        sum += x;
        size ++;
    }

    void remove(int x) {
        treeMap.put(x, treeMap.get(x) - 1);
        if (treeMap.get(x) == 0) treeMap.remove(x);
        sum -= x;
        size --;
    }
}
```

