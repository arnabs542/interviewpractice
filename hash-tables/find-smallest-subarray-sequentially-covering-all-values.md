#### Find Smallest Subarray Sequentially Covering all Values

> In the previous problem, we did not differentiate between the order in which keywords appeared. If the digest has to include the keywords in the order in which they appear in the search textbox, we may get a different digest. Write a program that takes two arrays of strings, and return the indices of the starting and ending index of a shortest subarray of the first array \(the "paragraph" array\) that "sequentially covers", i.e., contains all the strings in the second array \(the "keywords" array\), in the order in which they appear in the keywords array. Assume keywords are distinct. 
>
> Example:
>
> ```
> paragraph = <"apple", "banana", "cat", "apple">
> keywords = <"banana", "apple">
>
> answer: [1,3]
> ```
>
> Explanation:
>
> The paragraph subarray starting at index 0 and ending at index 1 does not fulfill the specification, even though it conatins all the keywords, since they do not appear in the specified order. On the other hand, the subarray starting at index 1 and ending at index 3 does fulfill the specification.



