### 1. What is a list?

* A list is an ordered, mutable collection of items (elements) that can hold heterogeneous types.
```python
Literal syntax: [].Example: L = [1, "a", 3.14, [2,3]].
```
---

### 2. Creating lists
```python
empty = []
a = [1,2,3]
b = list([4,5,6])          # constructor from iterable
chars = list("abc")        # ['a','b','c']
rep = [0]*5                # [0,0,0,0,0]
nested = [[1,2],[3,4]]
```
---

### 3. Indexing & negative indices

* Zero-based indexing: L[0] is first element.

* Negative indexing: L[-1] is last, L[-2] second last.

* IndexError if out of range.
---

### 4. Slicing (creates a new list)

* Syntax: L[start:stop:step]

* L[1:4] -> elements at indices 1,2,3.

* Omitted start/stop/step default to beginning/end/1.

* L[:] makes a shallow copy.
#### Examples:

```python
L = [0,1,2,3,4,5]
L[::2]    # [0,2,4]
L[::-1]   # reversed list
```
---
### 5. Mutability & shallow vs deep copy

* Lists are mutable: you can assign ```python L[0] = 99```.

* Shallow copy duplicates top-level list but not nested objects:

* copy1 = L[:] or list(L) or L.copy()

* Deep copy copies nested objects: ```python import copy; deep = copy.deepcopy(L)```
---
### 6. Common list methods (in-place modifies list)

* L.append(x) — add x to end. O(1) amortized.

* L.extend(iterable) — append all. O(k).

* L.insert(i, x) — insert at index i. O(n).

* L.remove(x) — remove first occurrence. O(n).

* L.pop([i]) — pop and return element (default last). O(1) for last, O(n) otherwise.

* L.clear() — remove all elements.

* L.index(x[, start[,end]]) — first index. O(n).

* L.count(x) — number of occurrences. O(n).

* L.sort(key=None, reverse=False) — in-place sort. TimSort: O(n log n) average.

* L.reverse() — reverse in-place. O(n).

* L.copy() — shallow copy.
---
### 7. Built-in functions working with lists

* len(L), min(L), max(L), sum(L) (numeric), sorted(L) (returns new list), reversed(L) returns iterator, any(L), all(L).

* enumerate(L) for index + value.

* zip() to iterate parallel lists.

* map, filter, reduce (from functools).
---
### 8. List comprehensions (pythonic & fast)
```python
squares = [x*x for x in range(10)]
evens = [x for x in range(20) if x%2==0]
pairs = [(i,j) for i in range(3) for j in range(3)]
```
* Equivalent to for-loops but usually faster and concise.

* Avoid overly complex comprehensions for readability.
---
### 9. Generator expressions vs list comprehensions

* (x*x for x in range(10)) creates generator (lazy) — uses less memory.

* Use when you don't need entire list at once.
---
### 10. Nested lists (2D lists)
```python
matrix = [[0]*C for _ in range(R)] — correct way.
```
Pitfall: matrix = [[0]*C]*R shares rows (bad).
Example:
```python
rows = 3; cols = 4
mat = [[0]*cols for _ in range(rows)]
mat[0][0] = 5   # only affects mat[0][0]
```

---
### 11. Time complexity (big-O) — important for GATE

* Assume n = len(L).

* Access by index L[i]: O(1)

* Update L[i] = x: O(1)

* append: O(1) amortized

* pop() last: O(1); pop(i): O(n)

* insert(0, x) or insert at arbitrary index: O(n)

* remove(x), index(x), count(x): O(n)

* in membership test: O(n)

* copy, slice (creates new list): O(n)

* extend(k items): O(k)

* sort: O(n log n) average (TimSort)

* reverse: O(n)

---
### 12. Memory model & implementation notes

* Python lists are dynamic arrays (contiguous pointers to PyObject*).

* Amortized resizing strategy grows capacity to avoid frequent reallocations.

* Lists store references; elements themselves live elsewhere.

---
### 13. Idiomatic patterns & tips

* Use append + pop() for stack.

* Use collections.deque for efficient queue (append/pop left O(1)).

* For membership of many elements, convert list to set for O(1) average in.

* Use enumerate instead of manual index counters.

* Use zip for parallel loops.

* Prefer list comprehensions for clarity and speed over map/lambda in many cases.

---
### 14. Common pitfalls (GATE trap examples)

Shared references in nested list creation:

* Wrong: a = [[0]*3]*3 leads to shared rows.

Mutability when copying:

* b = a does not copy; both refer same list.

Using list + vs append:

* L = L + [x] creates a new list (O(n)); prefer append.

* Sorting list of tuples: sorts by first element then second.

* Using min/max on empty list → ValueError.

---
### 15. Useful short examples

Reverse a list:
```python
L[::-1]     # returns new reversed list
L.reverse() # in-place, returns None
```

Remove duplicates while preserving order:
```python
seen = set()
res = [x for x in L if not (x in seen or seen.add(x))]
```

Flatten one-level nested list:
```python
flat = [y for x in nested for y in x]
```
---
### 16. Advanced: slicing assignment (mutating)
```python
L[1:3] = [9,9]   # replaces slice
L[::2] = [1,2,3] # must match length of slice
```
---
### 17. Sorting with key
```python
names = ["alice","Bob","charlie"]
names.sort(key=str.lower)         # case-insensitive
pairs = [(1,3),(1,2),(0,5)]
pairs.sort(key=lambda x: (x[0], -x[1]))
```
---
### 18. Practical GATE-style practice problems (with short hints/solutions)

Reverse list in-place without builtin:

* Hint: two-pointer swap.

* Solution:
```python
i, j = 0, len(L)-1
while i < j:
    L[i], L[j] = L[j], L[i]
    i += 1; j -= 1
```

Rotate list right by k:

* Use k %= n and L[:] = L[-k:] + L[:-k].

Find second largest element:

* Single pass maintaining max1, max2 (handle duplicates).

Check if list is palindrome:

* L == L[::-1] (O(n) time, O(n) extra). For O(1) extra, two-pointer compare.

Flatten arbitrary nested lists (recursive):

* Use recursion or stack; careful with types.
---
### 19. Quick reference cheat sheet (one-liners)

* Copy shallow: b = a[:] or a.copy()

* Deep copy: import copy; b = copy.deepcopy(a)

* Append: a.append(x)

* Extend: a.extend([x,y])

* Insert: a.insert(i, x)

* Pop: x = a.pop() or x = a.pop(i)

* Remove by value: a.remove(x)

* Sort in place: a.sort()

* Sorted copy: b = sorted(a)

* Reverse in place: a.reverse()

* Slice copy: b = a[:]

* Length: n = len(a)
---
### 20. Suggested study plan for GATE (practical)

* Day 1: basics, create/index/slice, mutability.

* Day 2: methods & built-ins, practice append/extend/insert/pop.

* Day 3: comprehensions + generator expressions.

* Day 4: nested lists, matrix creation pitfalls.

* Day 5: complexity and common pitfalls.

* Day 6–7: solve 20 problems (rotations, searches, merges, flatten, etc.).

Revise cheat sheet daily.

---
### 21. Recommended practice problems (easy→hard)

Reverse, rotate, remove duplicates, merge two sorted lists, sliding window max, two-sum (using dict), longest increasing subsequence (uses lists + DP), reshape matrix, spiral order traversal.

---
### 22. Final quick tips

* Memorize complexities (access O(1), insert/ remove O(n) unless at end).

* Remember shallow vs deep copy.

* Avoid [[0]*C]*R for matrices.

* Use deque for efficient queue operations.

Practice writing clean code — GATE values correctness + clarity.