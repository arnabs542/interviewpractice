#### Read N Characters Given Read4 II - Call multiple times
> The API: `int read4(char *buf)` reads `4` characters at a time from a file.
>
> The return value is the actual number of characters read. For example, it returns `3` if there is only `3` characters left in the file.
>
> By using the `read4` API, implement the function `int read(char *buf, int n)` that reads `n` characters from the file.
>
> The read function may be called multiple times.
>
> Example 1
> ```
> Input:
> "filetestbuffer"
> read(6)
> read(5)
> read(4)
> read(3)
> read(2)
> read(1)
> read(10)
> Output:
> 6, buf = "filete"
> 5, buf = "stbuf"
> 3, buf = "fer"
> 0, buf = ""
> 0, buf = ""
> 0, buf = ""
> 0, buf = ""
> Example 2
> 
> Input:
> "abcdef"
> read(1)
> read(5)
> Output:
> 1, buf = "a"
> 5, buf = "bcdef"
> ```

##### Solution

The entire trick to this problem is to not over think it. Instead, since we know we can only read 4 characters at once from a file, we maintain an internal buffer that only stores 4 characters. Once we exhuast those, we go get more from the file - if there are more to be found. Otherwise, we simply return.

```py
class Solution:

    # @param {char[]} buf destination buffer
    # @param {int} n maximum number of characters to read
    # @return {int} the number of characters read
    def __init__(self):
        self.buf = [None] * 4
        self.start = self.end = 0
        
    def read(self, buf, n):
        # Write your code here
        char_read = 0
        while char_read < n:
            if self.start == self.end:  # Our current buffer is exhuasted
                self.start, self.end = 0, Reader.read4(self.buf)
                if not self.end:    # Out of characters
                    break
            buf[char_read] = self.buf[self.start]
            self.start += 1
            char_read += 1
        
        return char_read
```