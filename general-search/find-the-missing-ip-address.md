#### Find the Missing IP Address

> The storage capacity of hard drives dwarfs that of RAM. This can lead to interesting space-time trade-offs.
>
> Suppose you were given a file containing roughly one billion IP addresses, each of which is a 32-bit quanity. How would you programmatically find an IP address that is not in the file? Assume you have unlimited drive space but only a few megabytes of RAM at your disposal.

The key limitation here is the small amount of RAM we have relative to the data that needs to be processed. While sorting takes $$\small \mathcal O(n \log(n))$$ time, we would need to perform the sorting using the disk as storage, which in practice is very slow. If we try to use a hash table, we will need at least 4GB of ram, since each IP address is 8 bytes, and one GB is $$\small 10^9$$ bytes. 

We could allocate an array of $$\small 2^{32}$$ bits, initialized to 0, and write a 1 at each index that corresponds to an IP address in the file. Then we iterate the bit array, looking for an entry set to 0. There are $$\small 2^{32}  \approx 4 * 10^{9}$$ possible IP addresses, so not all IP addresses appear in the file. The storage is $$\small 2^{32}/8$$ bytes, roughly half a gigabyte, still well in excess of the storage limit. 

The key trick here is to use "bucketing". Suppose instead we initialize an array of size 2, and in one index we count all IP addresses with a leading bit of a 1, and the other all IP addresses with a leading bit of 0. At least one IP address must exist which is not present in the file, so at least one of these two counts is below $$\small 2^{31}$$. Suppose that we have determined using counting that there must be an IP address which begins with 0 and is absent from the file. We can focus our attention on IP addresses in the file that begin with 0, and continue the process of elimination based on the second bit. This entails 32 passes, and uses only two integer-valued count variables as storage. 

Since we have we have more storage, we can count on groups of bits. Specifically, we can count the number of IP addresses in the file that begin with $$\small 0, 1, 2,...,2^{16} - 1$$ using an array of $$\small 2^{16}$$ integers that can be represented with 32 bits. For every IP address in the file, we take its 16 MSBs to index into this array and increment the count of that number. Since the file contains fewer than $$\small 2^{32}$$ numbers, there must be one entry in the array that is less than $$\small 2^{16}$$. This tells us that there is at least one IP address which has those upper bits and is not in the file. In the second pass we can focus only on the addresses whose leading 16 bits match the one we have found, and use a bit array of size $$\small 2^{16}$$ to identify a missing address.

##### Code:

```py
def find_missing_element(stream):
    NUM_BUCKET = 1 << 16
    coutner = [0] * NUM_BUCKET
    stream, stream_copy = itertools.tee(stream)
    for x in stream:
        upper_part_x = x >> 16
        counter[upper_part_x] += 1

    # Look for a bucket that contains less than (1 << 16) elements
    BUCKET_CAPACITY = 1 <, 16
    candidate_bucket = next(i, for i, c in enumerate(counter) if c < BUCKET_CAPACITY)

    # Finds all IP address in the stream whose first 16 bits are equal to candidate bucket
    candidates = [0] * BUCKET_CAPACITY
    stream = stream_copy
    for x in stream_copy:
        upper_part_x = x >> 16
        if candidate_bucket -- upper_part_x:
            # Records the presence of 16 LSb of x
            lower_part_x = ((1 << 16) - 1) & x
            candidates[lower_part_x] = 1

    # At least of the LSB combinations is absent, find it
    for i, v in enumerate(candidates):
        if v == 0:
            return (candidate_bucket << 16) | i
```



