Pattern Defeating Quicksort
-------

Pattern-defeating quicksort (pdqsort) is a novel sorting algorithm that combines the fast average case of randomized quicksort with the fast worst case of heapsort, while achieving linear time on inputs with certain patterns. It's an extension and improvement of David Mussers introsort. All code is available for free under the Creative Common license.

    Best        Average     Worst       Memory      Stable      Deterministic
    n           n log n     n log n     log n       No          Yes

### Usage

`pdqsort` is a drop-in replacement for [`std::sort`](http://en.cppreference.com/w/cpp/algorithm/sort). Just replace a call to `std::sort` with `pdqsort` to start using pattern-defeating quicksort. If your comparison function is branchless, you can call `pdqsort_branchless` for a potential big speedup. If you are using C++11, the type you're sorting is arithmetic and your comparison function is not given or is `std::less`/`std::greater`, `pdqsort` automatically delegates to `pdqsort_branchless`.

### Benchmark

A comparison of pdqsort and GCC's `std::sort` and `std::stable_sort` with various input distributions:

![Performance graph](http://i.imgur.com/1RnIGBO.png)

Compiled with `-std=c++11 -O2 -m64 -march=native`.

### Visualization

A visualization of pattern-defeating quicksort sorting a ~200 element array with some duplicates. Generated using Timo Bingmann's [The Sound of Sorting](http://panthema.net/2013/sound-of-sorting/) program, a tool that has been invaluable during the development of pdqsort. For this visualization the cutoff point for insertion sort was lowered to 8 elements.

![Visualization](http://i.imgur.com/QzFG09F.gif)

### The best case

pdqsort is designed to run in linear time for certain best-case patterns: inputs in strictly ascending or descending order, equal elements, or ascending order followed by one out-of-place element.

For equal elements, a smart partitioning scheme always puts equal elements in the partition containing elements greater than the pivot. When a new pivot is chosen, it's compared to the greatest element in the partition before it. If they are equal, we can derive there are no smaller elements. When this happens, we filter out all elements equal to the pivot.

For other patterns, we check after every partition if any swaps were made. If no swaps were made and the partition was balanced, we use insertion sort, which aborts if too many moves are required.

### The average case

On average case data where no patterns are detected, pdqsort is effectively a quicksort using median-of-3 pivot selection, switching to insertion sort if the number of elements is small. The overhead for detecting best-case patterns is minimal.

pdqsort speeds up sorting large arrays (1000+ elements) using a technique from "BlockQuicksort: How Branch Mispredictions don't affect Quicksort" by Stefan Edelkamp and Armin Weiss. This technique bypasses the branch predictor by using small buffers in L1 cache for indices of elements to be swapped, filled in a branch-free way:

buffer_num = 0; buffer_max_size = 64;
for (int i = 0; i < buffer_max_size; ++i) {
    buffer[buffer_num] = i; buffer_num += (elements[i] < pivot);
}

This speedup occurs only if the comparison function is branchless. By default, pdqsort detects this if using C++11 or higher, the type is arithmetic, and the comparison is std::less or std::greater. Use pdqsort_branchless explicitly for branchless partitioning.

The worst case
Quicksort performs poorly on patterned inputs due to partition-based sorting. Choosing a bad pivot results in many comparisons with little progress. To avoid this, traditionally, pivot selection is randomized. Introsort switches to heapsort if recursion depth is too big. pdqsort shuffles elements deterministically when encountering a "bad" partition, switching to heapsort if too many bad partitions are found.

Bad partitions
A bad partition occurs when the pivot's position is under 12.5% or over 87.5% percentile, making the partition highly unbalanced. In these cases, pdqsort shuffles four elements at fixed locations for both partitions. If more than log(n) bad partitions occur, pdqsort switches to heapsort.

An upper bound of quicksort's worst-case runtime is approximated by:

T(n, p) = n + T(p(n-1), p) + T((1-p)(n-1), p)

Choosing p = 1/8 ensures that heapsort is used only if it speeds up the sorting, balancing quicksort's best and worst cases.
