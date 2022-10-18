# Unit 22: Sorting

## Learning Objectives

After this unit, students should:

- be familiar with four sorting algorithms: counting sort, selection sort, bubble sort, and insertion sort
- be able to implement the above algorithms, argue that they are correct, and analyze their running time
- be aware of the difference between comparison-based sorting algorithms and counting sort
- be aware of the situations where insertion sort performs better than selection sort and bubble sort.
- be aware of the situations where counting sort performs better than other sorting algorithms.

## Sorting

Sorting is one of the most fundamental computational problems.  Given a list of items, we want to rearrange the items in some order.  We have seen in the previous unit that searching in a sorted list gives us tremendous improvement in efficiency.

In this unit, we assume that the items we wish to rearrange are integers, and we wish to rearrange them in increasing order.  We could, however, in practice, sort any type of item.

## Counting Sort

The first sorting algorithm we explore today is counting sort.  Suppose we are given a list of numbers between a given range, say 0 to $MAX$.   Counting sort works as follows

- scan through the input list once and count how many times each number appears in the list, using an array of size $MAX+1$ (called _the frequency array_) to keep track of the frequency of each number.  
- scan through the frequency array and put the elements back into the output.

For example, supposed we are asked to sort a list containing only numbers 0-4: $\langle 0, 4, 3, 2, 3, 1, 0, 4, 4, 1 \rangle$.  Scanning through this list, we count how many times each number appears in the list, and we get the following frequency array: 0 appears twice, 1 appears twice, 2 appears once, 3 appears twice, and 4 appears thrice.  So we produce a list of two 0s, two 1s, one 2, two 3s, and three 4s, $\langle 0, 0, 1, 1, 2, 3, 3, 4, 4, 4\rangle$.

Here is the code that implements the counting sort algorithm.
```C
/**
 * Perform counting sort on the input in[] and store the sorted
 * numbers in out[].
 *
 * @param[in] in The array containing numbers to be sorted.
 * @param[out] out The array containing the sorted numbers.
 * @param[in] len The size of the input and output array.
 *
 * @pre in[i] is between 0 and MAX for all i.
 * @post out[] is sorted
 */
void counting_sort(size_t len, const long in[], long out[])
{
  size_t freq[MAX + 1] = { 0 };

  for (size_t i = 0; i < len; i += 1) {
    freq[in[i]] += 1;
  }

  size_t outpos = 0;
  for (long i = 0; i <= MAX; i += 1) {
    for (size_t j = outpos; j < outpos + freq[i]; j += 1) {
  	  out[j] = i;
    }
    outpos += freq[i];
  }
}
```

Let's consider the running time of the counting sort.  We will break it down into three steps of counting sort.

First, initializing `freq` array to 0 takes $O(MAX)$ time.

Second, looping through `in` and counting takes $O(n)$ time (where $n$ is `len`).

What about the third step: populating the output array with sorted numbers?  This step involves a double for loop.  But, you shouldn't jump to the conclusion that it takes $O(MAX^2)$ or $O(n^2)$.  Let's analyze this more carefully.

We will go with a more intuitive/informal approach first.  What we do here is to store the sorted numbers into the output, there are $n$ numbers, so it seems reasonable to assume that this step takes $O(n)$ time.

Let's verify more formally and systematically.  The inner loop:
```C
    for (size_t j = outpos; j < outpos + freq[i]; j += 1) {
  	  out[j] = i;
    }
```

takes $O(f_i)$ time, where $f_i$ is the `freq[i]` the number of times $i$ appears.  The outer loop loops through this for different $i$, from $i = 0, .. MAX$.  So the total number of times is:

\[
\sum_{i=0}^{MAX} f_i
\]

This corresponds to the sum of the number of times each number appears in the input, which is simply $n$.  So the third step takes $O(n)$ time.

The runnning time for counting sort is thus $O(n + n + MAX)$, which is just $O(n + MAX)$.

Note that since we do not know the relationship between $n$ and $MAX$, we cannot simplify this term to either $O(n)$ or $O(MAX)$.

## Selection Sort

Another simple sorting algorithm is _selection sort_.  Assuming we are sorting the numbers in increasing order.  Given a list of numbers, selection sort partitions the list into two parts, an unsorted part followed by a sorted part.  It then repeatedly selects the maximum element from the unsorted part and move it to the front of the sorted part.   Here is an example.  

Suppose the input is `8 4 23 42 16 15`.  At the beginning, the whole list is considered unsorted.   We use the `|` to indicate the boundary between the sorted and unsorted parts below.

```
8 4 23 42 16 15 | 
```

Selection sort selects the largest element from the unsorted part, which is 42, and moves it to the beginning of the sorted part.  We get:

```
8 4 23 16 15 | 42
```

Now, the largest element from the unsorted part is 23.  Selection sort moves it to the beginning of the sorted part.

```
8 4 16 15 | 23 42
```

This continues until the unsorted part is empty.  The list is now sorted.

```
8 4 15 | 16 23 42
8 4 | 15 16 23 42
4 | 8 15 16 23 42
| 4 8 15 16 23 42
```

Here is an implementation of selection sort.

```C
/**
 * Find the index of the largest element among list[0..last].
 *
 * @param[in] last The last element to search.
 * @param[in] list Input list
 * @return The index of the max element among list[0..last].
 *         Breaking ties by choosing the smaller index.
 * @pre list is not NULL and list[0] .. list[last] are valid.
 */
size_t max(size_t last, const long list[])
{
  long max_so_far = list[0];
  size_t max_index = 0;
  for (size_t i = 1; i <= last; i += 1) {
    if (list[i] > max_so_far) {
      max_so_far = list[i];
      max_index = i;
    }
  }
  return max_index;
}

/**
 * Sort a list using selection sort.
 *
 * @param[in] n The size of the list to sort.
 * @param[in] list The input list
 * @pre list is not NULL and list[0]..list[n-1] are valid
 * @post The list is sorted.
 */
void selection_sort(size_t n, long list[])
{
  for (size_t j = n - 1; j >= 1; j -= 1) {
    size_t max_pos = max(j, list);
    swap(list, max_pos, j);
  }
}
```

To analyze the running time, notice that in the function `selection_sort` above, Lines 17-18 are repeated $O(n)$ times.  Line 17 calls another function `max`.  Inside `max`, there is another loop that repeats $j - 1$ times.  Each time we call `max`, `j` decreases, so the loop in `max` takes one fewer step each time the function is called.

To calculate the total number of steps, we can compute the following sum
$\sum_{j=1}^{n-1}(j-1)$, which is just $0 + 1 + 2 + .. + (n-3) + (n-2)$
This sum is the sum of an arithmetic series and equals to $(n-2)(n-1)/2$.   Since we use the Big-O notation, we can focus on the term with the highest rate of growth, $n^2$, and ignore everything else.  The running time for `selection_sort` function is therefore $O(n^2)$.

## Selection vs Counting Sort

Comparing the running of counting sort $O(n + MAX)$ vs selection sort $O(n^2)$, it is clear that counting sort is more efficient -- we say that counting sort is a _linear time_ algorithm and selection sort is a _quadratic time_ algorithm.

What is the magic of counting sort?  Why don't we just use counting sort all the time, and why bother learning about other sorting algorithms?

It turns out that counting sort is special because it has an assumption that the input numbers fall into a certain range.  If $MAX$ is small, then counting sort is efficient.  If $MAX$ is say, the maximum long values, $2^{63}-1$, then counting sort is not necessarily more efficient (both in terms of time and space) in practice than selection sort.  

Because of this assumption, counting sort does not need to compare the inputs during sorting, and thus it can achieve a linear time.

Selection sort, on the other hand, does not assume the range of the input numbers.  It is a _comparison sort_ since it compares the input numbers during sorting.  It is therefore more general and has a wider range of applications.

We now look at two more comparison-based sorting algorithms.

## Bubble Sort

Bubble sort is probably the most well-known, under-performed sorting algorithm[^1], but is taught in most CS classes because of its simplicity.  The idea of bubble sort is to make multiple passes through the list.  In each pass, we look for all possible adjacent pairs of items.  Any adjacent pair that is out of order is swapped so that they are in order.  This process repeats until everything is in order.

Let's look at an example.  Suppose we have, as an input, the numbers `8 4 23 42 16 15`.  In the first pass, we start from the first item and check from left to right.  The pair `8 4` is out of order, so we swap them, and we get `4 8 23 42 16 15`.  Pairs `8 23` and `23 42` are in order, so we do not need to swap them.  The pair `42 16` is out of order.  We swap them and get `4 8 23 16 42 15`.  The pair `42 15` is again out of order, so we swap them and get `4 8 23 16 15 42`.

The following sequence shows the first pass through the array:

```
 8  4 23 42 16 15   <- swap
-- --
 4  8 23 42 16 15  
   -- --
 4  8 23 42 16 15  
      -- --
 4  8 23 42 16 15   <- swap
         -- --
 4  8 23 16 42 15   <- swap
            -- --
 4  8 23 16 15 42  
```

After the first pass, notice that the largest element, 42, "bubbles" up through the list until it reaches the maximum position.  We can now make the second pass, but we can exclude the last item since it is already in place.

```
 4  8 23 16 15 42  
-- --
 4  8 23 16 15 42  
   -- --
 4  8 23 16 15 42   <- swap
      -- --
 4  8 16 23 15 42   <- swap
         -- --
 4  8 16 15 23 42   
         -- --
```

After the second pass, the second-largest element, 23, is in its position.  So we can exclude this item in the subsequent pass.

The rest of the passes operate similarly.  In the $i$-th pass, we scan through array item 0 to $n-i$, swapping any adjacent element that is out of order, until $i = n - 1$, in which case we only have two elements, we swap them if they are out of order, and we are done!

The code for bubble sort can be written as follows:

```C
void bubble_pass(size_t last, long a[]) {
  for (size_t i = 0; i < last; i += 1) {
    if (a[i] > a[i+1]) {
      swap(a, i, i+1);
    }
  }
}

void bubble_sort(size_t n, long a[n]) {
  for (size_t last = n - 1; last > 0; last -= 1) {
    bubble_pass(last, a);
  }
}
```

How many steps does it take to bubble-sort an array of $n$ elements?   Since the $i$-th pass scans through $n-i$ elements, and there are a $n$ passes in total, the analysis is similar to the one we did for the algorithm to compute the selection sort -- bubble sort takes $O(n^2)$ steps.

## Insertion Sort

The next sorting algorithm we are going to discuss is the insertion sort.  This is another classic algorithm, that could perform better than bubble sort in some scenarios.  Similar to selection sort, we partition the input list into two, a sorted partition, and an unsorted partition.  Then we repeatedly take the first element from the unsorted partition, find its rightful place in the sorted partition, and _insert_ it into place.  We start with a sorted partition of one element, and we end if the sorted partition contains all the elements.

Take `8 4 23 42 16 15` as an example.  We use `|` to partition the array into a sorted partition, and an unsorted partition.

```
8 | 4 23 42 16 15
```

We pick the first element on the unsorted partition, 4, and insert it into the sorted partition.  This involves shifting the elements in the sorted partition to the right until we find the rightful place for `4`.   After this step, the sorted partition grows by 1 and the unsorted partition shrinks by 1.

```
4 8 | 23 42 16 15
```

In the next round, we take `23`, and find its rightful place.  It turns out `23` is already in its correct place.
```
4 8 23 | 42 16 15
```

In the next step, `42` is also in its correct place.
```
4 8 23 42 | 16 15
```

`16` is the next element, and we insert it between 8 and 23.
```
4 8 16 23 42 | 15
```

Finally, we insert `15`, and we are done, as there is no more element in the unsorted partition.
```
4 8 15 16 23 42
```

The code for insertion sort can be written as follows:

```C
void insert(long a[], size_t curr)
{
  size_t i = curr;
  long temp = a[curr];
  while (i >= 1 && temp < a[i - 1]) {
    a[i] = a[i - 1];
    i -= 1;
  }
  a[i] = temp;
}

void insertion_sort(size_t n, long a[]) {
  for (size_t curr = 1; curr < n; curr += 1) {
    insert(a, curr);
  }
}
```

## Animation

Animations for various sorting algorithms, including some of which you will learn in CS2040C, are available online on [VisuAlgo](https://visualgo.net/bn/sorting).

## Problem Set 22

### Problem 22.1

In the implementation of bubble sort above, we always make $n-1$ passes through the array.  It is, however, possible to stop the whole sorting procedure, when a pass through the array does not lead to any swapping.  Modify the code above to achieve this optimization.

### Problem 22.2

(a) Suppose the input list to insertion sort is already sorted.  What is the running time of insertion sort?

(b) Suppose the input list to insertion sort is inversely sorted.  What is the running time of insertion sort?

### Problem 22.3

In certain scenarios, a comparison is more expensive than an assignment.  For instance, comparing two strings is more expensive than assigning a string to a variable.  In this case, we can reduce the number of comparisons during insertion sort by doing the following:

repeat

- take the first element X from the unsorted partition
- use binary search to find the correct position to insert X
- insert X into the right place

until the unsorted partition is empty.

Implement the variation to the insertion sort algorithm above.  You may use your solution from Problem 21.1.

[^1]: https://www.youtube.com/watch?v=k4RRi_ntQc8
