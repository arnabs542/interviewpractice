#### Implement an ISBN Cache \(LRU Cache\)

##### Code:

```py
class LruCache:
    class ListNode:
        def __init__(self, val):
            self.val = val
            self.next = self.front = None

    def __init__(self, capacity):
        self.prices, self.books = {}, {}
        self.capacity = capacity
        self.head, self.tail = self.ListNode(-1), self.ListNode(-1)
        self.head.next = self.tail
        self.tail.front = self.head
        return

    def update_node(self, isbn):
        node = self.books[isbn]
        node.front = self.head
        node.next = self.head.next
        node.next.front = node
        self.head.next = node

    def delete_node(self, isbn):
        node = self.books[isbn]
        node.front.next = node.next
        node.next.front = node.front

    def lookup(self, isbn):
        if isbn in self.prices:
            # Update LRU chain
            self.delete_node(isbn)
            self.update_node(isbn)
            return self.prices[isbn]
        return -1

    def insert(self, isbn, price):
        # Node doesn't exist, create node and update to front of LRU chain
        if isbn not in self.prices:
            self.prices[isbn] = price
            self.books[isbn] = self.ListNode(isbn)
            self.update_node(isbn)
            # If cache is full, delete according to LRU policy
            if len(self.prices) > self.capacity:
                self.erase(self.tail.front.val)
        else:   # Update value and node to be at front of LRU chain
            self.delete_node(isbn)
            self.update_node(isbn)
        return

    def erase(self, isbn):
        if isbn in self.prices:
            self.delete_node(isbn)
            del self.prices[isbn]
            del self.books[isbn]
            return True
        return False
```



