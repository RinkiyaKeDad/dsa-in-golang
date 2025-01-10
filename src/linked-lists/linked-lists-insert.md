# Inserting a Node in Linked Lists

In the [last section](./linked-lists-basics.md), we saw how to represent and initialize a linked list, insert and delete a node, and how to display the entire linked list. Now we'll take a look at two common insert methods: `InsertAfter` and `InsertBefore`.

`InsertAfter`, as the name suggests, inserts a node after the node you specified. It takes two arguments: the node after which the new node is to be inserted and the value of this new node. Here's what the code for this looks like:

```go
func (ll *LinkedList) InsertAfter(target, val int) {
	if ll.Head == nil {
		fmt.Println("Linked list is empty")
		return
	}
	cur := ll.Head

	for cur != nil {
		if cur.Val == target {
			cur.Next = &Node{Val: val, Next: cur.Next}
			return
		}
		cur = cur.Next
	}
	fmt.Printf("%d not found in the linked list\n", target)
}
```

First, we check if the linked list is empty or not, because if it is, then there's nothing to insert after. After that, we iterate till we find the target node. If you're not familiar with the code for iterating linked lists, I recommend checking out the [previous post](./linked-lists-basics.md) on the basics of linked lists.

Once we find the target node, we simply insert a node after it:

- by making the target's next equal to this new node
- setting the new node's next to the target's next (since it's getting inserted between the target and what would've been its next node)

And that's it. In the end, if none of the `return` statements got executed, it means the target node wasn't found in the linked list.

Let's now look at the code for the `InsertBefore` method:

```go
func (ll *LinkedList) InsertBefore(target, val int) {
	if ll.Head == nil {
		fmt.Println("Linked list is empty")
		return
	}
	cur := ll.Head

	// inserting before head
	if cur.Val == target {
		ll.Head = &Node{Val: val, Next: cur}
		return
	}

	for cur.Next != nil {
		if cur.Next.Val == target {
			cur.Next = &Node{Val: val, Next: cur.Next}
			return
		}
		cur = cur.Next
	}
	fmt.Printf("%d not found in the linked list\n", target)
}
```

Just like above, here we first check if the linked list is empty or not.

Then we handle the special case where the new node is to be inserted before the first node, because in that case, we will need to update the head of the linked list.

After that, we iterate through the linked list till we find the node **before which** the new node is to be inserted. Checking for `cur.Next.Val == target` instead of `cur.Val == target` helps us do that. When this condition is met, we just have to insert a node after the `cur` node, so we use similar code as the `InsertAfter` method to achieve that.

Lastly, if no `return` statement gets executed, it means the target wasn't found in the linked list.