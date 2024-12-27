# Binary Search

In the previous section, we learned how we can sort an array by dividing it into two halves each time using [merge sort](../sorting/merge-sort.md). A similar logic can be used to find a particular element in a **sorted array**. Let's see how binary search achieves exactly that:

1. Find the middle index.
1. Compare the element at the middle index with the one we have to find.
1. If found, return the index.
1. If the middle element is greater, then that means the element to be found is in the left half. So repeat the process reducing the search space to `[start, middle-1]`. 
1. If the middle element is smaller, then that means the element to be found is in the right half. Repeat the process reducing the search space to `[middle+1, end]`.
1. If at the end you don't find anything, it means the element never existed in the array.

Let's look at two ways to implement this in Golang now:

## Binary Search Iterative Approach

The code for the iterative approach for binary search looks like this:

```go
package main

import (
	"fmt"
)

func main() {
	nums := []int{2, 4, 5, 6, 8, 9}
	target := 8
	fmt.Printf("%d found at %d index", target, binarySearch(nums, 0, len(nums)-1, target))
}

func binarySearch(nums []int, start, end, target int) int {
	for start <= end {
		mid := (start + end) / 2
		if nums[mid] == target {
			return mid
		} else if nums[mid] > target {
			end = mid - 1
		} else {
			start = mid + 1
		}
	}
	return -1
}
```

Following the same logic as in our [quick sort](../sorting/quick-sort.md) discussion, the time complexity for this is O(log N). The space complexity is O(1) since we're only using constant space.

## Binary Search Recursive Approach

Here's what the code for binary search looks like with the recursive approach:

```go
package main

import (
	"fmt"
)

func main() {
	nums := []int{2, 4, 5, 6, 8, 9}
	target := 8
	fmt.Printf("%d found at %d index", target, binarySearch(nums, 0, len(nums)-1, target))
}

func binarySearch(nums []int, start, end, target int) int {
	if start > end {
		return -1
	}
	mid := (start + end) / 2
	if nums[mid] == target {
		return mid
	} else if nums[mid] > target {
		return binarySearch(nums, start, mid-1, target)
	} else {
		return binarySearch(nums, mid+1, end, target)
	}
}
```
Instead of going in a loop like we did in the previous approach, here we use recursion to call the `binarySearch` function again on one of the two halves depending on where our element might be.

The time complexity in this case is O(log N) as well. The space complexity is O(log N) instead of O(1) because of the recursive call stack which has been explained [here](../sorting/quick-sort.md).