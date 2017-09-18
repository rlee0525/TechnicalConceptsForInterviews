# Sorting Algorithms

### High-level thoughts
- Common sorting methods are upper-bounded by a polynomial - they are all O(n^2).
- Lower bound is O(n) because we must at least have to read each value in the array. ex) Bucket sort and Radix sort on restricted types of inputs).
- In practice, since n can be quite large, O(n log n) methods are valuably quicker than O(n^2) methods.

### Summary
- O(N) (Special cases)
  - Bucket sort
  - Counting sort
  - Radix sort
- O(N log N)
  - Mergesort
  - Heapsort
  - Quicksort
  - Treesort
- O(N^2) (Works well for sufficiently small arrays)
  - Insertion sort
  - Selection sort
  - Bubble sort

### Bubble sort
- Iterates through an array, swaps with the leemtn in front if necessary

### Selection sort vs. Insertion Sort
#### Selection Sort
![Selection Sort Diagram](http://res.cloudinary.com/rlee0525/image/upload/v1505762432/selection_sort_thumb_4_o6gp4v.jpg)
#### Insertion Sort
![Insertion Sort Diagram](http://res.cloudinary.com/rlee0525/image/upload/v1505762432/insertion_sort_thumb_1_xoqy9k.jpg)
#### Comparison
- Selection sort takes the current element and exchange it with the smallest element on the right side
- Insertion sort takes the current element and insert it at the appropriate position of the list, adjusting the list every time you insert.
- Selection sort is always n(n - 1) / 2 ~= n ^ 2
- Insertion sort is worst case n(n - 1) / 2 ~= n ^ 2

### Merge sort
- Breaking input into many different pieces recursively, and merging back together.
- Space complexity of O(N)
- Stable sort (because you always merge the left side first then the right side)

### Heap sort
- Turn input into a heap
- Extract min or max, set it into it's appropriate place.
- Space complexity of O(1)

### Quicksort
#### Basic Idea
- Base case return if length <= 1
- Select pivot
- Partition array around pivot (left and right)
- Recurse on left and right
#### In place
- **Partitioning** is the main difference
  - Select a pointer that increases by one only when you find elements that are less than or equal to current pivot, while iterating the rest of the array
  - Before incrementing, we'll first swap that element with the element on the right side of the pointer
  - After first iteration, swap our pivot with the element left side of the pointer
  - Pivot is now locked in its proper place
- If already sorted,
  - We'll have N depth tree rather than log n tree causing the algorithm to be N ^ 2.
  - Picking middle element as a pivot is good for already sorted but not for certain pathological data set
  - Try to avoid this by randomizing the pivot
  - In-place but since it's recursive, space complexity is still log N due to call stacks

### Why is quicksort preferred over heapsort or mergesort despite its worst case being O(N^2)?
- If your data is already ordered,
  - Quicksort almost never performs unnecessary element swaps, which is time consuming. (Best case is much better!)
  - Heapsort, you are going to swap 100% of elements.
  - Mergesort, you are going to write 100% of elements in another array and write it back in the original one.
- Locality of memory access is also important to execution speed.
  - Quicksort has better locality of reference as the next thing to be accessed is usually close in memory to the thing you just looked at, meaning they will likely be cached together.

### Bucket Sort
- It separates array into smaller buckets / groups and sort them individually using a subsorting algorithm or recursive call to itself and then combines the result.
- O(n + k)
- Good for **Dense** arrays
- Not in-place but stable

### Radix Sort
- Elements are put into buckets in initial pass O(N)
- O(nk) (k being the length of each element)
- Appends the buckets without further sorting and re-buckets it based on the second digit of the numbers.
- Good for **Sparse** arrays
- In-place and stable

### Helpful Links
- https://www.toptal.com/developers/sorting-algorithms/ (Animated Gifs)
