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
- O(N^2)
  - Insertion sort
  - Selection sort
  - Bubble sort

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

### Why is quicksort preferred over heapsort or mergesort despite its worst case being O(N^2)?
- If your data is already ordered,
  - Quicksort almost never performs unnecessary element swaps, which is time consuming. (Best case is much better!)
  - Heapsort, you are going to swap 100% of elements.
  - Mergesort, you are going to write 100% of elements in another array and write it back in the original one.
- Locality of memory access is also important to execution speed.
  - Quicksort has better locality of reference as the next thing to be accessed is usually close in memory to the thing you just looked at, meaning they will likely be cached together.