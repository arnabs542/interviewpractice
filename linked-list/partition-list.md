#### Partition List

> Given a linked list and a value _x_, partition it such that all nodes less than _x_ come before nodes greater than or equal to _x_.
>
> You should preserve the original relative order of the nodes in each of the two partitions.
>
> **Example:**
>
> ```
> Input: head = 1->4->3->2->5->2, x = 3
> Output: 1->2->2->4->3->5
> ```

##### Dummy Nodes:

```py
def partition(head, x):

    s_head = s_tail = ListNode(0)
    l_head = l_tail = ListNode(0)
    while head:
        if head.val < x:
            s_tail.next = head
            s_tail = s_tail.next
        else:
            l_tail.next = head
            l_tail = l_tail.next
        head = head.next        

    l_tail.next = None
    s_tail.next = l_head.next
    return s_head.next
```

For linked list partitioning/rearranging problems, always consider using some dummy nodes to deal with the movement. The code above has $$\small \mathcal O(n)$$ runtime and $$\small \mathcal O(1)$$ space usage.

