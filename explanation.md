The pattern defeating quicksort (PDQsort) is a variation of the quicksort algorithm designed to handle pathological cases more efficiently. Traditional quicksort can degrade to O(n²) time complexity on certain input sequences, such as already sorted or nearly sorted arrays. PDQsort incorporates techniques from both introsort and insertion sort to mitigate these issues.

# --------  Key Features of PDQsort  --------

Introspection:

PDQsort uses introspection to switch from quicksort to heapsort when the recursion depth exceeds a certain limit. This ensures that the worst-case time complexity is kept at O(n log n).

Partitioning:

It uses a median-of-medians technique to choose a pivot, improving the chance of balanced partitions. This reduces the likelihood of encountering worst-case scenarios.

Detecting Patterns:

PDQsort incorporates pattern detection to identify and handle already sorted or nearly sorted sequences. When a sorted pattern is detected, it switches to insertion sort, which is efficient for small or nearly sorted arrays.

Adaptive Sorting:

The algorithm adapts to the degree of disorder in the input. If the input is partially sorted, PDQsort takes advantage of this to sort more efficiently than a generic quicksort.

# --------  How PDQsort Works  --------

Pivot Selection:

Selects pivots using a median-of-medians approach to ensure good partitioning.

Recursion Depth Monitoring:

Monitors the recursion depth and switches to heapsort if it exceeds a certain threshold.

Pattern Detection:

During partitioning, it detects if the array is already sorted or exhibits a pattern. If detected, it may use insertion sort or avoid further recursion.

Insertion Sort for Small Partitions:

For small partitions or nearly sorted arrays, PDQsort switches to insertion sort, which is faster for these cases.

# --------  Advantages of PDQsort  --------

Performance:

PDQsort maintains average-case performance similar to quicksort but avoids the O(n²) worst-case scenario.

Adaptability:

It adapts to the input data's order, optimizing the sorting process for partially sorted arrays.

Stability:

While not inherently stable, PDQsort can be modified to maintain stability in sorting.

# --------  Use Cases  --------

PDQsort is particularly useful in scenarios where input data may exhibit patterns or be nearly sorted, such as:

Databases:
Sorting data retrieved in a nearly sorted manner.

Real-time Systems:
Efficient sorting where worst-case performance must be controlled.

General-purpose Libraries:
Standard sorting routines where input characteristics are unpredictable.

In summary, PDQsort enhances the quicksort algorithm by incorporating pattern detection, adaptive sorting, and introspection, ensuring robust performance across various input scenarios.

# --------  Author  --------

nRanzo

# --------  Special thanks  --------

Orson Peters

# --------  History  --------

PDQsort was developed by Orson Peters, a Dutch programmer. He created PDQsort to address the limitations of traditional quicksort and to improve its performance on specific patterns of input data. PDQsort is widely recognized for its efficiency and adaptability, and it has been incorporated into the C++ Standard Library as the default sorting algorithm since C++17.
