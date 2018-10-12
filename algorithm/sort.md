# :whale: 排序算法

<!-- TOC -->

- [:whale: 排序算法](#whale-排序算法)
  - [:fish: 冒泡排序](#fish-冒泡排序)
  - [:fish: 堆排序](#fish-堆排序)
    - [:lollipop: 最大堆调整 (Max Heapify)](#lollipop-最大堆调整-max-heapify)
    - [:lollipop: 建堆 (Build Heap)](#lollipop-建堆-build-heap)
    - [:lollipop: 堆排序操作 (heap sort)](#lollipop-堆排序操作-heap-sort)
  - [:fish: 插入排序](#fish-插入排序)
  - [:fish: 快速排序](#fish-快速排序)

<!-- /TOC -->

## :fish: 冒泡排序

> 稳定, 时间复杂度 O(n^2), 空间复杂度 O(1)

<details> <summary> c 语言实现 </summary>
<p>

```c
void bubbleSort(int len, int* a) {
  for (int i = 0; i < len; i++) {
    for (int j = 0; j < len -1; j++) {
      if (a[j] < a[j+1]) {
        int b = 0;
        b = a[j];
        a[j] = a[j+1];
        a[j+1] = b;
      }
    }
  }
}
```

</p>
</details>

## :fish: 堆排序

> 不稳定, 时间复杂度 O(n) ~ O(n*log(n)), 空间复杂度 O(1)

堆通常用一维数组表示. 在数据起始位置为 0 的情形中:

- 父节点 i 的左节点在位置 (2*i + 1)
- 父节点 i 的右节点在位置 (2*i + 2)
- 子节点 i 在父节点在位置 floor((i - 1) / 2)

堆定义以下几种操作:

- 最大堆调整 (Max Heapify): 将堆的末端子节点作调整, 使得子节点永远小于父节点.
- 创建最大堆 (Build Max Heap): 将堆中所有数据重新排序.
- 堆排序 (HeapSort): 移除位在第一个数据的根节点, 并作最大堆调整的递归运算.

### :lollipop: 最大堆调整 (Max Heapify)

:boom: 伪代码实现

    max_heapify(A, i):
      l = left(i) // 左孩子下标
      r = right(i) // 右孩子下标
      if l <= A.heap_size and A[l] > A[i]
        largest = l
      else largest = i
      if r <= A.heap_size and A[r] > A[largest]
        largest = r
      if largest != i
        exchange A[i] with A[largest]
        max_heapify(A, largest)

### :lollipop: 建堆 (Build Heap)

:boom: 伪代码实现

    build_max_heap(A):
      A.heap_size = A.length
      for i = floor(A.length / 2 - 1) downto 0:
        max_heapify(A, i)

:herb: n = A.length, 下标从零开始的话, 子数组 A(floor(n/2) .. n) 都是树的叶节点, 每个叶节点都可以看成只包含一个元素的堆, `build_max_heap` 对其它节点都调用一次 `max_heapify` , 这是自底向上的建堆.

### :lollipop: 堆排序操作 (heap sort)

:boom: 伪代码实现

    heapsort(A):
      for i = A.length downto 1
        exchange A[0] with A[i]
        A.heap_size = A.heap_size - 1
        max_heapify(A, 0)

:herb: 不断地把堆定元素放在数组后面, 把数组后面地替换的元素放在堆顶, 再调整堆. 这样操作直到 heap_size = 0;

## :fish: 插入排序

> 不稳定, 时间复杂度 O(n) ~ O(n<sup>2</sup>), 空间复杂度 O(1)

:boom: 伪代码实现

    insert_sort(A):
      for j = 2 to A.length:
        key = A[j]
        i = j - 1
        while i > 0 and A[i] > key:
          // 将 j 前面比 key 大的向后面移动一位
          A[i+1] = A[i]
          i = i - 1
        A[i+1] = key // 将 key 插入到比 key 小的元素后面

## :fish: 快速排序
