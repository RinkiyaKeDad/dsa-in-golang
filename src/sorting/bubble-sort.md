# Bubble Sort 

Bubble sort is all about *bubbling up* the largest element towards the end of the array.

- Starting from index 0 each time till the end, we keep swapping elements if the larger one is on the left.
- This ensures the largest element is at the end of the array after the first pass.
- Then we repeat the process but only till the second last element. This ensures the correct position for the second largest element.
- Simiarly keep repeating the process for the third largest element and so on.

This way, by the end of the sorting algorithm, the entire array gets sorted. Every time we do a pass on the array, we also store whether there were any swaps or not in a [flag](https://en.wikibooks.org/wiki/Programming_Fundamentals/Flag_Concept). If there have been no swaps, that means the array is already sorted. This reduces the time complexity to O(N) in the best case when the array is already sorted. In all other cases, the time complexity is O(N<sup>2</sup>), like for [selection sort](./selection-sort.md). The space complexity is O(1). Since repeated swaps cost more memory, this is slightly more memory-intensive compared to selection sort, which only swaps once per loop.

```go
package main

import "fmt"

func bubbleSort(nums []int) {
	for i := len(nums) - 1; i > 0; i-- {
		swap := false
		for j := 0; j < i; j++ {
			if nums[j] > nums[j+1] {
				nums[j], nums[j+1] = nums[j+1], nums[j]
				swap = true
			}
		}
		if !swap {
			break
		}
	}
}

func main() {
	nums := []int{3, -1, 0, 2, -5, 3, 7, -2}
	fmt.Println("Before sorting: ", nums)
	bubbleSort(nums)
	fmt.Println("After sorting: ", nums)
}
```