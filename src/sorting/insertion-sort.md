# Insertion Sort

Insertion sort operates on the idea of going through each element in the array and inserting it at the right position till that point (not the complete array). Because of the way the algorithm works when you’re working on putting element at index `i` in the right spot, the array before `i` is already sorted.

But what is the right position? Right position for an element is when all the elements greater than it are on the right side and all elements lesser than it are on the left side. How does this happen?

1. For an element at index `i` loop `j` from `i-1` till `0` (the array would be sorted before point `i`)
1. As long as elements are greater than the one at `i` keep moving them forward (`nums[j+1] = nums[j]`)
1. Finally insert the element at `i` in the correct spot which would be `j+1` at the end of the loop (since `j` would point to the element which is lesser than `i`)
1. After that increment `i` and repeat the process. The array before the index `i` would be sorted already.

```go
package main

import "fmt"

func insertionSort(nums []int) {
	for i := 1; i < len(nums); i++ {
		key := nums[i]
		j := i - 1
		for ; j >= 0 && nums[j] > key; j-- {
			nums[j+1] = nums[j]
		}
		nums[j+1] = key
	}
}

func main() {
	nums := []int{3, -1, 0, 2, -5, 3, 7, -2}
	fmt.Println("Before sorting: ", nums)
	insertionSort(nums)
	fmt.Println("After sorting: ", nums)
}
```

The time complexity for this is O(N) in the best case that is when the array is sorted already since we never enter the inner `for` loop. For all other cases it is O(N<sup>2</sup>). The space complexity is O(1) since we’re only storing variables of fixed size independent of the array length.