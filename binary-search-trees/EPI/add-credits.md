#### Add Credits

> Consider a server that a large number of clients connect to. Each client is identified by a string. Each client has a "credit", which is a nonnegative integer value. The server needs to maintain a data structure to which clients can be added, removed, queried, or updated. In addition, the server needs to be able to add a specified number of credits to all clients simultaneously.
>
> Design a data structure that impelements the following methods:
>
> * Insert: add a client with specified credit, replacing any existing entry for the client.
> * Remove: delete the specified client.
> * Lookup: return the number of credits associated with the specified client. 
> * Add-to-all: increment the credit count for all current clients by the specified amount. 
> * Max: return a client with the highest number of credits.

##### Code \(Hash table\):

```py
class ClientsCreditsInfo:
    def __init__(self):
        self.offset = 0
        self.clients = {}

    def insert(self, client_id, c):
        self.clients[client_id] = c - self.offset
        return

    def remove(self, client_id):
        if client_id in self.clients:
            del self.clients[client_id]
            return True
        return False

    def lookup(self, client_id):
        if client_id in self.clients:
            return self.clients[client_id] + self.offset
        return -1

    def add_all(self, C):
        self.offset += C
        return

    def max(self):
        res = float('-inf')
        max_client = ''
        for k, v in self.clients.items():
            if v > res:
                max_client = k
        return max_client
```

##### Explanation:

A hash table is an obvious candidate for this problem - it supports fast insertion, removal, and lookup. A quick way to deal with add-to-all is to simply keep a global counter of the current offset. When new elements come in, we adjust their value by subtracting the offset, and when we return lookups, we return the value plus the offset. For example, if we are to add `22` to all current values, values afterwards need to account for the `22`, otherwise we'd end returning the wrong value.

The problem is that it takes $\small \mathcal O(n)$ time to find the maximum value, since hash tables aren't ordered in any way. Therefore, we need to iterate through all elements to find the maximum value. If we expect the max function to be called constantly, this can get expensive.

##### Code \(Hash table + BST\):

```py
class ClientsCreditsInfo:
    def __init__(self):
        self._offset = 0
        self._client_to_credit = {}
        self._credit_to_clients = bintrees.RBTree()

    def insert(self, client_id, c):
        self.remove(client_id)
        self._client_to_credit[client_id] = c - self._offset
        self._credit_to_clients.setdefault(c - self._offset, set()).add(client_id)

    def remove(self, client_id):
        credit = self._client_to_credit.get(client_id)
        if credit is not None:
            self._credit_to_clients[credit].remove(client_id)
            if not self._credit_to_clients[credit]:
                del self._credit_to_clients[credit]
            del self._client_to_credit[client_id]
            return True
        return False

    def lookup(self, client_id):
        credit = self._client_to_credit.get(client_id)
        return credit + self._offset if credit is not None else -1

    def add_all(self, C):
        self._offset += C

    def max(self):
        if not self._credit_to_clients:
            return ''
        clients = self._credit_to_clients.max_item()[1]
        return '' if not clients else next(iter(clients))
```

##### Explanation:

To mitigate the inefficiency of finding the maximum element from a hash table, we use a BST. Insert and removal takes $\small \mathcal O(\log{n})$ time now due to the BST. However, max also only takes $\small \mathcal O(\log {n})$, which is faster than just with a hash table. 

The documentation for setdefault:

> set\_default\(k\[,d\]\) -&gt; value, T.get\(k, d\), also set T\[k\]=d if k not in T, O\(log\(n\)\) \(synonym setdefault\(\) exist\)

Use it for when you're not sure if the key exists. If not, it'll initialize the key-value pair with the value you specify.

