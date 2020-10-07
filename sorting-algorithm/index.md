# 排序算法


<!--more-->

## 排序分类

| 排序方法 | 时间复杂度（平均）   | 时间复杂度（最坏）   | 时间复杂度（最好）   | 空间复杂度           | 稳定性 |
| -------- | -------------------- | -------------------- | -------------------- | -------------------- | ------ |
| 插入排序 | O(n<sup>2</sup>)     | O(n<sup>2</sup>)     | O(n)                 | O(1)                 | 稳定   |
| 希尔排序 | O(n<sup>1.3</sup>)   | O(n<sup>2</sup>)     | O(n)                 | O(1)                 | 不稳定 |
| 选择排序 | O(n<sup>2</sup>)     | O(n<sup>2</sup>)     | O(n<sup>2</sup>)     | O(1)                 | 不稳定 |
| 堆排序   | O(nlog<sub>2</sub>n) | O(nlog<sub>2</sub>n) | O(nlog<sub>2</sub>n) | O(1)                 | 不稳定 |
| 冒泡排序 | O(n<sup>2</sup>)     | O(n<sup>2</sup>)     | O(n)                 | O(1)                 | 稳定   |
| 快速排序 | O(nlog<sub>2</sub>n) | O(n<sup>2</sup>)     | O(nlog<sub>2</sub>n) | O(nlog<sub>2</sub>n) | 不稳定 |
| 归并排序 | O(nlog<sub>2</sub>n) | O(nlog<sub>2</sub>n) | O(nlog<sub>2</sub>n) | O(n)                 | 稳定   |
| 计数排序 | O(n+k)               | O(n+k)               | O(n+k)               | O(n+k)               | 稳定   |
| 桶排序   | O(n+k)               | O(n<sup>2</sup>)     | O(n)                 | O(n+k)               | 稳定   |
| 基数排序 | O(n*k)               | O(n*k)               | O(n*k)               | O(n+k)               | 稳定   |

{{<admonition type="tip" open=true title="提示">}}

* 时间复杂度：对数据的操作次数。
* 空间复杂度：指算法会在计算机内存中占用的资源。
* 稳定：就是指排完序后，相等的a和b的相对位置不会发生变动。
* 不稳定：和稳定相反，排序后相等的值相对位置可能变动。

{{</admonition >}}

## 算法详解

### 插入排序

1. 按顺序取出一个值a。
2. 将a和他前一个值b比较，如果a小就将b移动到a的位置上，a移动到b的位置上。
3. 重复2，直到走到比a小的值c后就将a放在c之后。
4. 重复1~3，直到数组遍历完。

{{<image src="/images/sorting-algorithm/Insertion_sort.gif" width="300px" caption="插入排序">}}

```go
package sorting

// InsertionSort 插入排序
// @param a是待排序数组
func InsertionSort(a []float32) []float32 {
	for i := 1; i < len(a); i++ {
		key := a[i]
		j := i - 1
		for ; j >= 0 && key < a[j]; j-- {
			a[j+1] = a[j]
		}
		a[j+1] = key
	}
	return a
}
```

### 希尔排序

先将整个待排序的记录序列分割成为若干子序列分别进行直接插入排序，具体算法描述：

- 选择一个增量序列t1，t2，…，tk，其中ti>tj，tk=1。
- 按增量序列个数k，对序列进行k 趟排序。
- 每趟排序，根据对应的增量ti，将待排序列分割成若干长度为m 的子序列，分别对各子表进行直接插入排序。仅增量因子为1 时，整个序列作为一个表来处理，表长度即为整个序列的长度。

{{<image src="/images/sorting-algorithm/shellsort.gif" width="300px" caption="希尔排序">}}

```go
package sorting

// ShellSort 希尔排序
// @param array:待排序数组
// 对应笔记 http://0w0.pub/sorting-algorithm/#希尔排序
// 本例选择的步长为 N/2^k,目前的最佳步长为2^p3^q https://oeis.org/A003586
func ShellSort(array []int) {
	n := len(array)
	if n < 2 {
		return
	}
	key := n / 2
	for key > 0 {
		for i := key; i < n; i++ {
			j := i
			for j >= key && array[j] < array[j-key] {
				array[j], array[j-key] = array[j-key], array[j]
				j = j - key
			}
		}
		key = key / 2
	}
}
```



### 选择排序

选择排序是一种比较直观的排序方法。首先从待排序数组中按顺序拿出一个数，默认它为最大（小），然后和后面比较。如果遇到比这个数更大（小）的就交换位置。重复操作直到遍历完数组。

{{<image src="/images/sorting-algorithm/Selection_sort.gif" width="500px" caption="选择排序">}}

```Go
package sorting

// SelectionSort 选择排序
// @param array:待排序数组
// 对应笔记 https://0w0.pub/sorting-algorithm/#选择排序
func SelectionSort(array []int) []int {
	n := len(array)
	index := 0
	for i := 0; i < n; i++ {
		index = i
		for j := i + 1; j < n; j++ {
			if array[index] > array[j] {
				index = j
			}
		}
		array[i], array[index] = array[index], array[i]
	}
	return array
}
```

### 堆排序

堆排序是利用`堆`这种数据结构进行排序的算法。

通常堆是通过一维数组来实现的。在数组起始位置为0的情形中：

- 父节点 i 的左子节点在位置 2i+1。
- 父节点 i 的右子节点在位置 2i+2。
- 子节点 i 的父节点在位置 floor((i-1)/2)。floor表示向下取整。

{{<admonition type="quote" open="true">}}
**堆**（英语：Heap）是[计算机科学](https://zh.wikipedia.org/wiki/计算机科学)中的一种特别的[完全二叉树](https://zh.wikipedia.org/wiki/完全二叉树)。若是满足以下特性，即可称为堆：“给定堆中任意[节点](https://zh.wikipedia.org/wiki/節點)P和C，若P是C的母节点，那么P的值会小于等于（或大于等于）C的值”。若母节点的值恒**小于等于**子节点的值，此堆称为**最小堆**（min heap）；反之，若母节点的值恒**大于等于**子节点的值，此堆称为**最大堆**（max heap）。在堆中最顶端的那一个节点，称作**根节点**（root node），根节点本身没有**母节点**（parent node）。<sup>4</sup>
{{</admonition >}}

堆排序的具体用法：

1. 构建最大（小）堆。
2. 将root节点和最后一个节点交换。
3. 树的大小减一后重新构建最大（小）堆。
4. 重复2~3直到整个树操作完成

{{<image src="/images/sorting-algorithm/Heapsort.gif" width="500px" caption="堆排序">}}

```Go
package sorting

// Heapsort 堆排序
// @param array:待排序数组
// 对应笔记 http://0w0.pub/sorting-algorithm/#堆排序
func Heapsort(array []int) []int {
	n := len(array)
	for i := n / 2; i >= 0; i-- {
		MaxHeap(array, i, n)
	}
	for i := n - 1; i >= 0; i-- {
		array[0], array[i] = array[i], array[0]
		n--
		MaxHeap(array, 0, n)
	}
	return array
}

// MaxHeap 构建最大堆
// @param array:待构成的数组
// @param i:第i个子树
// @param n:数组长度
func MaxHeap(array []int, i, n int) {
	left := 2*i + 1
	right := 2*i + 2
	max := i // 最大值的下标

	if left < n && array[left] > array[max] {
		max = left
	}

	if right < n && array[right] > array[max] {
		max = right
	}

	if max != i {
		array[i], array[max] = array[max], array[i]
		MaxHeap(array, max, n)
	}
}
```

### 冒泡排序

1. 比较相邻两个元素，如果前一个比后一个大（小）就交换。
2. 对每个相邻元素执行一遍，最后一个就会是最大（小）的。
3. 重复执行1~2，但是每轮缩小一个范围。

{{<image src="/images/sorting-algorithm/BubbleSort.gif" width="500px" caption="冒泡排序">}}

```Go
package sorting

// BubbleSort 插入排序
// @param array是待排序数组
// 对应笔记 http://0w0.pub/sorting-algorithm/#冒泡排序
func BubbleSort(array []int) []int {
	n := len(array)
	for i := 0; i < n; i++ {
		for j := 0; j < n-1-i; j++ {
			if array[j] > array[j+1] {
				array[j], array[j+1] = array[j+1], array[j]
			}
		}
	}
	return array
}
```

### 快速排序

1. 选出一个区间。
2. 选出一个基准。
3. 将大于基准的放在右边，小于基准的放在左边。
4. 缩小区间。
5. 重复1~4，直到区间为1。

{{<image src="/images/sorting-algorithm/QuickSort.gif" width="500px" caption="快速排序">}}

```Go
package sorting

// QuickSort 快速排序
// @param array是待排序数组
// 对应笔记 http://0w0.pub/sorting-algorithm/#快速排序
func QuickSort(array []int, left, right int) []int {
	if left >= right {
		return array
	}
	l, r := left, right
	p := array[l]
	for l < r {
		for l < r && array[r] >= p {
			r--
		}
		if l < r {
			array[l] = array[r]
			l++
		}
		for l < r && array[l] < p {
			l++
		}
		if l < r {
			array[r] = array[l]
			r--
		}
	}
	array[l] = p
	QuickSort(array, left+1, right)
	QuickSort(array, left, right-1)
	return array
}
```

### 归并排序

1. 将待排序数组划分成小份数组。
2. 排序小数组。
3. 合并小数组。
4. 重复2~3直到全部数组都被排序。

{{<image src="/images/sorting-algorithm/merge_sort.gif" width="500px" caption="归并排序">}}

```Go
package sorting

// MergeSort 归并排序
// @param array是待排序数组
// 对应笔记 http://0w0.pub/sorting-algorithm/#归并排序
func MergeSort(array []int) []int {
	n := len(array)
	if n < 2 {
		return array
	}
	mid := n / 2
	left := MergeSort(array[:mid])
	right := MergeSort(array[mid:])
	return merge(left, right)
}

func merge(left, right []int) []int {
	array := make([]int, len(left)+len(right))
	i, j, index := 0, 0, 0
	for {
		if left[i] > right[j] {
			array[index] = right[j]
			j++
			index++
			if j == len(right) {
				copy(array[index:], left[i:])
				break
			}
		} else {
			array[index] = left[i]
			i++
			index++
			if i == len(left) {
				copy(array[index:], right[j:])
				break
			}
		}
	}
	return array
}
```

### 计数排序

1. 找出排序范围的最大值和最小值。
2. 统计每个值出现的个数。
3. 从小到大填充数组。

{{<image src="/images/sorting-algorithm/counting_sort.gif" width="500px" caption="计数排序">}}

```Go
package sorting

// CountingSort 计数排序
// @param array:待排序数组
// 对应笔记 https://0w0.pub/sorting-algorithm/#计数排序
func CountingSort(array []int) []int {
	min := array[0]
	max := array[0]
	m := make(map[int]int)
	for _, v := range array {
		if min > v {
			min = v
		}
		if max < v {
			max = v
		}
		if _, ok := m[v]; !ok {
			m[v] = 1
			continue
		}
		m[v]++
	}
	array = make([]int, 0)
	for ; min <= max; min++ {
		if num, ok := m[min]; ok {
			for ; num > 0; num-- {
				array = append(array, min)
			}
		}
	}
	return array
}
```

### 桶排序

1. 将数据分到预备的“桶”中。
2. 对桶中的数据进行排序（可以使用桶排序递归）。
3. 将数据放回原序列。

{{<image src="/images/sorting-algorithm/Bucket_sort_1.svg" width="400px" caption="元素分配到桶">}}

{{<image src="/images/sorting-algorithm/Bucket_sort_2.svg" width="400px" caption="排序桶中元素">}}

```Go
package sorting

// BucketSort 桶排序
// @param array:待排序数组
// 对应笔记 https://0w0.pub/sorting-algorithm/#桶排序
func BucketSort(array []int) []int {
	buckets := make(map[int][]int)
	for _, v := range array {
		if bucket, ok := buckets[v/10]; ok {
			bucket = append(bucket, v)
			continue
		}
		buckets[v/10] = []int{v}
	}
	record := make([]int, 0) // 记录桶详情
	for key, bucket := range buckets {
		record = append(record, key)
		bucket = Heapsort(bucket) // 使用堆排序对桶内元素排序
	}
	record = Heapsort(record) // map是不保证遍历顺序的
	array = make([]int, 0)
	for _, v := range record {
		array = append(array, buckets[v]...)
	}
	return array
}
```

### 基数排序

1. 找到序列的最大值。
2. 按照当前最低位（第一遍是个位、第二遍是十位）进行放置。
3. 按照顺序放入原序列。
4. 重复2~3，直到所有位都遍历完毕。

{{<image src="/images/sorting-algorithm/radix_sort.gif" width="500px" caption="基数排序">}}

```Go
package sorting

// RadixSort 基数排序
// @param array:待排序数组
// 对应笔记 https://0w0.pub/sorting-algorithm/#基数排序
func RadixSort(array []int) []int {
	max := array[0]
	for _, v := range array {
		if max < v {
			max = v
		}
	}
	radix := make([][]int, 10)
	i := 1
	for max/i != 0 {
		for _, num := range array {
			radix[num/i%10] = append(radix[num/i%10], num)
		}
		array = make([]int, 0)
		for i := 0; i < 10; i++ {
			array = append(array, radix[i]...)
			radix[i] = make([]int, 0)
		}
		i *= 10
	}
	return array
}
```

## 源代码

此节源代码我都放在了本人的GitHub上，可[点此](https://github.com/ByLCY/algorithm/tree/master/sorting)直接访问。

## 参考文献

[1] 维基百科.Sorting algorithm[DB/OL]. https://en.wikipedia.org/wiki/Insertion_sort#/media/File:Insertion_sort.gif ,2011-11--28/2020-10-04

[2] 维基百科.Shellsort[DB/OL]. https://en.wikipedia.org/wiki/Shellsort#/media/File:Sorting_shellsort_anim.gif ,2014-09--12/2020-10-04

[3] 一像素.十大经典排序算法（动图演示）[DB/OL]. https://www.cnblogs.com/onepixel/articles/7674659.html , 2017-10-15/2020-10-04

[4] 维基百科.堆[DB/OL]. https://zh.wikipedia.org/wiki/堆積 ,2020-08--15/2020-10-04

[5] 维基百科.快速排序. https://zh.wikipedia.org/wiki/快速排序#/media/File:Sorting_quicksort_anim.gif , 2006-06-19/2020-10-04

[6] 维基百科.桶排序. https://zh.wikipedia.org/wiki/%E6%A1%B6%E6%8E%92%E5%BA%8F , 2006-09-11/2020-10-04
