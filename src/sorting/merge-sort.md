# Merge Sort

Merge sort is where the sorting algorithms start getting fun (and efficient). The basic idea of merge sort is you take an array, divide it into two halves, sort the two halves, and then put them back together. How do you sort the two halves? You divide them into further two halves, sort them, and then put them back together. I think you can now sense the recursion :) Putting the halves back together is where the actual sorting logic comes into place.

So there are two concepts to understand for the merge sort algorithm:

1. Splitting the array and doing merge sort on it again and again till we’re down to single element arrays.
1. Putting the two arrays back together in a way that they are sorted.

The first one is pretty easy to understand. You just keep calling the `mergeSort` function on itself:

```go
func mergeSort(nums []int, start, end int) {
    if (start >= end){
        return
    }
    middle := start + ((end - start)/2)
    mergeSort(nums, start, middle)
    mergeSort(nums, middle+1, end)
    merge(nums,start, middle, end)
}
```

The terminating condition is when the `start` and `end` elements are equal (single element) or when the `start` index is greater than the `end` one (no element).

One thing to note here is how we’re calculating `middle`. We can do `start + end / 2`, but the problem with that is that if `start` and `end` are really large numbers, adding them would give a number that could be out of the range of a 64-bit integer. But then why use `start + ((end - start) / 2)` instead of `end - ((end - start) / 2)` when mathematically they’re both the same? It is because the first one gives us a `middle` which is closer to the `start` whereas the second one gives one closer to the `end`. Let’s understand this with an example:

For the case `start = 0, end = 1`:

```
middle = 1 - ((1 - 0) / 2) = 1
```

This would mean the recursive calls are:

```go
mergeSort(0, 1)
mergeSort(2, 1)
```

So you see how we end up making a recursive call again and again with `start = 0, end = 1`? This would cause a memory overflow. With the first formula, we don’t face this problem:

```
middle = 0 + ((1 - 0) / 2) = 0

mergeSort(0, 0)
mergeSort(1, 1)
```
Both of these recursive calls would terminate after entering the `if` statement in this case.

Now that we have understood the `mergeSort` function, let’s understand the putting back together (or the `merge`) step:

```go
func merge(nums []int, start, middle, end int){
    l1 := middle - start + 1
    l2 := end - middle

    a1 := make([]int, l1)
    a2 := make([]int, l2)


    for i:=0 ; i < l1; i++ {
        a1[i] = nums[start + i]
    }


    for i:=0 ; i < l2; i++ {
        a2[i] = nums[middle + 1 + i]
    }

    i := 0
    j := 0

    for i < l1 && j < l2 {
        if a1[i] < a2[j] {
            nums[start] = a1[i]
            i++
            start++
        } else {
            nums[start] = a2[j]
            j++
            start++
        }
    }

    for i < l1 {
        nums[start] = a1[i]
        i++
        start++
    }

    for j < l2 {
        nums[start] = a2[j]
        j++
        start++
    }
}
```

We basically first create two arrays from our single array. Remember that in the recursive calls no actual splitting of the array is happening. We’re just changing the start and end index of the same array. In the merge step, we create two new arrays which are actually different arrays based on the the current start, middle end points. Then we populate these arrays appropriately.

Because of recursion, when the two single element arrays are combined, they are sorted. This means any further arrays we’re calling this merge function on would also be sorted individually. We can take advantage of this fact by comparing the first elements of each array and incrementing till we reach the end of one of the split arrays. Then we add all the elements of the remaining array. Please understand that it is because of recursion and the first `a1[i] < a2[j]` condition that the smallest split arrays are sorted and so the ones combining them also keep getting sorted.


**Go syntax refresher**: In Go, the size of an array must be a compile-time constant. If you need a dynamically sized array, use a slice instead. A slice is a flexible and resizable abstraction over an array in Go. 

TLDR: you can’t declare arrays like this:
```go
a1: [l1]int
```

Here’s the full code for Merge Sort algorithm:

```go
package main

import "fmt"

func main() {
	nums := []int{3, -1, 0, 2, -5, 3, 7, -2}
	fmt.Println("Before sorting: ", nums)
	mergeSort(nums, 0, len(nums)-1)
	fmt.Println("After sorting: ", nums)
}

func mergeSort(nums []int, start, end int) {
	if start >= end {
		return
	}
	middle := start + ((end - start) / 2)
	mergeSort(nums, start, middle)
	mergeSort(nums, middle+1, end)
	merge(nums, start, middle, end)
}

func merge(nums []int, start, middle, end int) {
	l1 := middle - start + 1
	l2 := end - middle

	a1 := make([]int, l1)
	a2 := make([]int, l2)

	for i := 0; i < l1; i++ {
		a1[i] = nums[start+i]
	}

	for i := 0; i < l2; i++ {
		a2[i] = nums[middle+1+i]
	}

	i := 0
	j := 0

	for i < l1 && j < l2 {
		if a1[i] < a2[j] {
			nums[start] = a1[i]
			i++
			start++
		} else {
			nums[start] = a2[j]
			j++
			start++
		}
	}

	for i < l1 {
		nums[start] = a1[i]
		i++
		start++
	}

	for j < l2 {
		nums[start] = a2[j]
		j++
		start++
	}
}
```

The time complexity of Merge Sort is O(N * log N). This is because each time we split the array into two halves. So for an array of size 4, we would be spitting two times:

1. To create arrays of size 2 
1. These further get split to create arrays of size 1

This is mathematically the same as log<sub>2</sub> 4 = 2.

And to merge the subarrays we need to loop once which takes O(N) time. Both these combined give us a time complexity of O(N * log N) in all cases.

Space complexity of Merge Sort is O(N) since we create additional arrays (see the `merge` function) to store the two halves. The size of these additional arrays would lineraly depend on the size of the original array.