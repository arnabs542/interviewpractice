#### Filling Bookcase Shelves

> We have a sequence of books: the i-th book has thickness `books[i][0]` and height `books[i][1]`.
>
> We want to place these books in order onto bookcase shelves that have total width `shelf_width`.
>
> We choose some of the books to place on this shelf (such that the sum of their thickness is `<= shelf_width`), then build another level of shelf of the bookcase so that the total height of the bookcase has increased by the maximum height of the books we just put down.  We repeat this process until there are no more books to place.
>
> Note again that at each step of the above process, the order of the books we place is the same order as the given sequence of books.  For example, if we have an ordered list of 5 books, we might place the first and second book onto the first shelf, the third book on the second shelf, and the fourth and fifth book on the last shelf.
>
> Return the minimum possible height that the total bookshelf can be after placing shelves in this manner.
>
> Example 1:
> 
> ![](../assets/filling-bookshelves.png)
> ```
> Input: books = [[1,1],[2,3],[2,3],[1,1],[1,1],[1,1],[1,2]], shelf_width = 4
> Output: 6
> Explanation:
> The sum of the heights of the 3 shelves are 1 + 3 + 2 = 6.
> Notice that book number 2 does not have to be on the first shelf.
> ```

##### Solution

Very similar to the pretty printing problem. Instead of messiness, we want to minimize by height. 

Again, the ideal is the same. If the last shelf hold `books[i:j]`, then `books[:i]` must've been already optimally placed. 

For each new book, work backwards and test the minimimum configuration for all the previous backs that can be placed on the same shelf as the current book.

```py
def minHeightShelves(books: "List[List[int]]", shelf_width: int) -> int:
    n = len(books)
    # min_height[i] is the optimal height if books[i] is the last book
    min_height = [float('inf')] * n  
    
    for i, (width, height) in enumerate(books):
        cur_width, cur_height = width, height
        min_height[i] = cur_height + (min_height[i-1] if i > 0 else 0) # Start by placing book on shelf by self
        # Try to stuff earlier books with current book
        for j in range(i-1, -1, -1):
            cur_width += books[j][0]
            if cur_width <= shelf_width:
                cur_height = max(cur_height, books[j][1])
                min_height[i] = min(min_height[i], cur_height + (min_height[j-1] if j > 0 else 0))
            else:
                break
    
    return min_height[-1]
```