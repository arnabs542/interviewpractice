#### Sorting a List

> Implement a routine which sorts lists efficiently. It should be a stable sort, i.e., the relative positions of equal elements must remain unchanged.

##### Code:

```py
def merge_two_sorted_lists(l1, l2):
    dummy = tail = ListNode(0)
    while l1 and l2:
        if l2.data < l1.data:
            tail.next = l2
            l2 = l2.next
        else:
            tail.next = l1
            l1 = l1.next
        tail = tail.next
    tail.next = l1 if l1 else l2
    return dummy.next

def stable_sort_list(L):
    # If no node of 1 node already sorted
    if not L or not L.next:
        return L

    # Split the list into two using slow/fast pointer
    pre_slow, slow, fast = None, L, L
    while fast and fast.next:
        pre_slow = slow
        slow, fast = slow.next, fast.next.next

    # slow is now at the middle of the list. Split list into 2
    pre_slow.next = None
    return merge_two_sorted_lists(stable_sort_list(L), stable_sort_list(slow))
```

##### Explanation:

There wasn't really anything difficult conceptually; the difficulty of this problem is solely based on the implementation. We apply mergesort to the list, using a slow/fast-pointer method to find the middle of the list to split. After sorting the left and right sublists, we then just merge them together. Running time is $$\small O(n \log{n})$$, space is $$\small O(\log{n})$$ from the recursion stack. 

