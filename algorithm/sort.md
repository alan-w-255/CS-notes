# 排序算法

## 冒泡排序

> 时间复杂度 O(n^2), 空间复杂度 O(1)

<details> <summary> c 语言实现 </summary>
<p>

```c
void bubbleSort(int a_count, int* a) {
  for (int i = 0; i < a_count; i++) {
    for (int j = 0; j < a_count -1; j++) {
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
