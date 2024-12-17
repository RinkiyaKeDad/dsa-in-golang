# Selection Sort

Selection sort is probably one of the easiest sorting algorithms to begin with. Like the name suggests, selection sort is about:

1.  "Selecting" the element with the lowest value
2.  Swapping it with the element at the start position
3.  Incrementing the start position
4.  Repeating the process

This is what the code for selection sort looks like:

```go
package main

import "fmt"

func sortArray(nums []int) {
	for i := 0; i < len(nums)-1; i++ {
		min_index := i
		for j := i + 1; j < len(nums); j++ {
			if nums[j] < nums[min_index] {
				min_index = j
			}
		}
		nums[i], nums[min_index] = nums[min_index], nums[i]
	}
}

func main() {
	nums := []int{3, -1, 0, 2, -5, 3, 7, -2}
	fmt.Println("Before Sorting: ", nums)
	sortArray(nums)
	fmt.Println("After Sorting: ", nums)
}
```

So for a given array: 

- You begin from index 0 as the start
- Loop from start + 1 till the end of the array
- See if any element is lesser than the element at the start
- If it is store the index of that element 
- Swap the element at the stored index with that at the start
- Update the start to start + 1 and repeat

Selection sort always takes a time complexity of O(N<sup>2</sup>) to sort the array since for each `start`, we go through the complete array (two nested `for` loops). The space complexity is O(1) since the extra memory used is only for the `min_index` variable, which doesn't grow depending on the size of the array.