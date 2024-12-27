# Quick Sort

Quick Sort, as the name suggests, is supposed to be quicker than the O(N<sup>2</sup>) algorithms. Just like Merge Sort, it achieves this through recursion and by dividing the array into smaller arrays each time. It differs from merge sort in that we don’t actually create new arrays, instead, we update the original array in memory. The way quick sort works is:

1. Marking the last element as the pivot.
1. Putting the pivot in its correct spot (all smaller elements to the left and larger ones to the right of the pivot).
1. Repeating the process for subarrays to the left and right of the pivot.

The second step is actually pretty interesting when you think of how it’s achieved. We start one variable from `start - 1` (`i`) and the other from `start` (`j`). Anytime we find an element smaller than the pivot (last element of the array), we increment `i` and move this found element behind in the array to the position of `i` by swapping the two. This way, we skip the elements larger than the pivot, and they end up getting swapped out towards the right side while the lesser elements come to the left side of the array. Let’s visualize this to understand better:

Initial array: [2, 4, 1, 3]

-   i = -1, j = 0: `nums[j] < pivot` so `i` becomes 0 and gets swapped with `j` (in this case nothing happens since both are 2).
    
    [2, 4, 1, 3]

-   i = 0, j = 1: `nums[j] > pivot` so nothing happens (notice how now `i` will reach this position in the next turn and then swap out this larger element (4) with a smaller one (1)).
    
    [2, 4, 1, 3]

-   i = 0, j = 2: `nums[j] < pivot` so it's time to send it back. `i` gets incremented and becomes equal to 1, and then elements at `i` and `j` swap places.
    
    [2, 1, 4, 3]

-   i = 1, j = 3: Loop ends.

Finally, the last smaller element than the pivot is at `i`, so we replace `i+1` and the pivot:

[2, 1, 3, 4]

And we return `i+1` (2) because that is the final location of the pivot.

And that's how the partitioning happens. Then we simply call the quick sort function on the two halves to the left and right of the pivot and repeat the process. Here's what the code for it looks like:

```go
package main

import "fmt"

func main() {
	nums := []int{3, -1, 0, 2, -5, 3, 7, -2}
	fmt.Println("Before sorting: ", nums)
	quickSort(nums, 0, len(nums)-1)
	fmt.Println("After sorting: ", nums)
}

func quickSort(nums []int, start, end int) {
	if start >= end {
		return
	}

	pivot := partition(nums, start, end)
	quickSort(nums, start, pivot-1)
	quickSort(nums, pivot+1, end)
}

func partition(nums []int, start, end int) int {
	i := start - 1
	for j := start; j < end; j++ {
		if nums[j] < nums[end] {
			i++
			nums[i], nums[j] = nums[j], nums[i]
		}
	}
	nums[i+1], nums[end] = nums[end], nums[i+1]
	return i + 1
}
```

Since, just like merge sort, we’re dividing the array in half each time, the average time complexity for quick sort is O(N * log N). The log⁡ N part comes because we split the array in half each time and operate on the halves together, which saves time. However, in the case where the array is already sorted with the largest element on the rightmost side, there would be no second half, and the whole array would be operated on each time, just shifting the pivot from the end each time. In this worst case, the time complexity would be O(N<sup>2</sup>)

To understand the space complexity we first need to understand what a function call stack is. Each recursive call gets added to the function call stack. Think of it as basically a stack that keeps track of function calls. For example, for this code:

```go
func main() {
	fmt.Printf("Factorial of %d is %d", 3, fac(3))
}

func fac(n int) int {
	if n == 1 {
		return 1
	}
	return n * fac(n-1)
}
```
The call stack looks like this:

| Function Call Stack |
|---------------------|
| fac(1)              |
| fac(2)              |
| fac(3)              |

Each function in the call stack takes memory space. So for this factorial code, the space complexity is O(N).

From that understanding, we can see that for average and best cases in Quick Sort, the array would keep getting divided into halves, and the space complexity would be O(log N). But in the worst case, like we discussed for the time complexity, there would be N recursive calls, and the space complexity would be O(N).
