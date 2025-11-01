### 1. What is a dictionary?

* A dictionary is a **mutable**, **ordered** (from Python 3.7 onward) collection of **key:value** pairs, where keys are *unique* and *hashable*.  
    ```python
    # literal syntax:
    D = {"brand": "Ford", "model": "Mustang", "year": 1964}
    ``` 

---

### 2. Creating dictionaries  
```python
empty = {}
a = {"name": "Alice", "age": 30}
b = dict([("x",1), ("y",2)])        # constructor from iterable
c = dict(x=1, y=2)                 # keyword‑args constructor
nested = {"person": {"name":"Bob", "age":25}}
```
Keys must be immutable (e.g., string, number, tuple); values can be any type. 

---

### 3. Accessing & negative indices (not applicable)  
* Dictionaries **do not** support indexing by integer position as lists do — you access values by key.  
* You can check for membership of a key: `key in D` returns `True`/`False`.

---

### 4. Common operations (retrieve, add, update, remove)  
* Access value: `value = D[key]` — raises `KeyError` if key not present.   
* Safe access: `value = D.get(key, default)` — returns `default` if key missing.   
* Add/update: `D[new_key] = new_value`  
* Delete: `del D[key]`, or `value = D.pop(key)`  
* Clear all: `D.clear()`  
* Merge/update with another dictionary: `D.update(other_dict)`

---

### 5. Dictionary methods  
Some key methods you must know:  
- `D.keys()` → returns view of keys  
- `D.values()` → returns view of values  
- `D.items()` → returns view of (key, value) pairs  
- `D.pop(key[, default])` → removes key, returns value or default  
- `D.popitem()` → removes and returns *last inserted* pair (Python 3.7+)  
- `D.setdefault(key, default)` → returns value of key if present, else inserts key with default and returns default  
- `D.fromkeys(seq, value)` → class method to create new dict from sequence of keys mapping to same value

---

### 6. Built­in functions & dictionary comprehensions  
* `len(D)`, `min(D)`, `max(D)` (on keys)  
* `sorted(D)` returns sorted list of keys; `sorted(D.items(), key=…)` for sorted key‑value pairs.  
* Dictionary comprehension:  

    ```python
    squares = {x: x*x for x in range(6)}
    ```
* Conversion between dict and list of tuples: `dict(zip(keys, values))`

---

### 7. Nested dictionaries & mixed types  
Dictionaries can hold other dictionaries, lists, etc., as values:  
```python
users = {
    "user1": {"name":"Alice", "age":30},
    "user2": {"name":"Bob",   "age":25}
}
```
Keys must still be hashable; values can be any object.

---

### 8. Time complexity (big‑O) — important for GATE  
For a dictionary `D` with `n` items (assuming ideal hashing):  
* Access/lookup `D[key]`: **O(1)** average  
* Insert/update `D[key] = value`: **O(1)** average  
* Delete `del D[key]` or `D.pop(key)`: **O(1)** average  
* Membership test `key in D`: **O(1)** average  
* Iteration over keys/items/values: **O(n)**  
Note: Worst‑case hash collisions degrade performance but rare in Python’s implementation.

---

### 9. Memory model & implementation notes  
* Internally, dictionaries use a hash table mapping keys to values; keys are hashed to determine slot.  
* From Python 3.7 onward, dictionaries **preserve insertion order**, but you should not rely on order for algorithmic correctness unless intended.  
* Values stored are references to Python objects; like lists and tuples, dictionaries store references not copies unless you explicitly copy.

---

### 10. Idiomatic patterns & tips  
* Use dictionaries when you need fast lookup by unique key (e.g., mapping user IDs to records).  
* Use `defaultdict`, `Counter`, etc. from `collections` for specialized use‑cases (though outside core ‘dict’).  
* Use `dict.items()` in loops when you need both key + value:  

    ```python
    for key, value in D.items():
        ...
    ```  
* When you need key‑order independent behavior (just membership or mapping), use `dict` vs list for faster lookup.  
* Avoid using mutable objects (like lists) as keys — will raise `TypeError`.  
* If you need to merge dictionaries: use unpacking `{**D1, **D2}` or `D1.update(D2)`.

---

### 11. Common pitfalls (GATE‑trap examples)  
* Using a list as a key: `D[[1,2]] = "value"` → `TypeError`.  
* Mutating values that are mutable inside dict and assuming immutability of dict (e.g., `D["x"] = [1,2]`, then modifying list).  
* Confusing indexing like a list: `D[0]` unless key `0` exists.  
* Assuming dictionaries are unordered (they **are** ordered from 3.7+).  
* Overwriting duplicate keys silently: when you write `{"a":1, "a":2}`, only last value kept.  
* Forgetting to handle missing keys (KeyError) — use `.get()` or `in`.

---

### 12. Useful short examples  
```python
# Counting occurrences:
word_count = {}
for w in words:
    word_count[w] = word_count.get(w, 0) + 1

# Swap keys and values (assuming values are unique and hashable):
inv = {v: k for k, v in D.items()}

# Merge two dicts (new dict):
D3 = {**D1, **D2}

# Filter dict by value:
filtered = {k: v for k, v in D.items() if v > threshold}
```

---

### 13. Advanced: Dictionary comprehensions & default values  
```python
squares = {x: x*x for x in range(10)}

keys = ["name", "age", "location"]
defaults = dict.fromkeys(keys, None)   # { "name": None, "age": None, ... }

# setdefault example:
profile = {"name": "Alice"}
profile.setdefault("age", 25)  # if "age" not exists, sets to 25
```
`fromkeys`, `setdefault`, dictionary comprehension are powerful extras.

---

### 14. Comparison with list & tuple  
| Feature       | List                         | Tuple                         | Dictionary                         |
|--------------|------------------------------|-------------------------------|------------------------------------|
| Syntax       | `[1,2,3]`                    | `(1,2,3)`                     | `{"a":1, "b":2}`                    |
| Access       | By index `L[i]`              | By index `T[i]`               | By key `D[key]`                     |
| Mutability   | Mutable                      | Immutable                     | Mutable                            |
| Order        | Ordered                      | Ordered                       | Ordered (insertion) from 3.7+      |
| Key requirement | –                         | –                             | Keys must be hashable & unique     |
| Typical use  | Sequence of items            | Fixed collection               | Mapping from keys to values        |
| Time complexity lookup | O(n)            | O(n)                         | O(1) average (hash lookup)         |

---

### 15. Practical GATE‑style practice problems (with short hints/solutions)

**Problem 1**: Given a list of numbers, return a dictionary mapping each number to its frequency.  
* Hint: iterate list and use `.get()` or `dict.setdefault()`.

**Problem 2**: Given two lists `keys` and `values` of the same length, create a dictionary mapping keys[i] → values[i].  
* Hint: Use `zip(keys, values)` or comprehension `{k:v for k, v in zip(keys, values)}`.

**Problem 3**: Given a dictionary `D`, remove all key‑value pairs where value < threshold and return a new dictionary.  
* Hint: use dict comprehension.

**Problem 4**: Given dictionary `D`, invert it (swap keys & values) assuming values are unique and hashable.  
* Hint: `{v:k for k, v in D.items()}`.

---

### 16. Quick reference cheat sheet (one‑liners)  
* Create: `D = {"a":1, "b":2}` or `D = dict(a=1, b=2)`  
* Empty: `D = {}`  
* Access: `x = D["key"]` or `x = D.get("key", default)`  
* Add/Update: `D["new_key"] = new_value`  
* Delete: `del D["key"]` or `val = D.pop("key", default)`  
* Keys / Values / Items: `D.keys()`, `D.values()`, `D.items()`  
* Comprehension: `{k:v for k, v in iterable}`  
* Merge: `D3 = {**D1, **D2}` or `D1.update(D2)`  
* Length: `n = len(D)`  
* Membership: `"key" in D` → `True`/`False`

---

### 17. Suggested study plan for GATE (practical)  
* Day 1: basics of dictionary creation, syntax, key:value pairs, access.  
* Day 2: adding/updating/removing, built‑in methods, safe access.  
* Day 3: dictionary comprehensions, nested dicts, conversion between list and dict.  
* Day 4: complexity behaviours, mapping use‑cases, memory/implementation notes.  
* Day 5: solve ~15 problems – frequency count, invert dict, filter dict, merge dicts.  
* Days 6–7: revisit pitfalls, compare list/tuple/dict, timed mock problems.

---

### 18. Recommended practice problems (easy→hard)  
* Count word frequencies in a string using a dict.  
* Convert list of tuples into dict.  
* Filter dict values below a threshold.  
* Merge multiple dicts and handle key collisions.  
* Given dict of dicts, flatten into single dict with composite keys.  
* Given huge dataset, use dict to index records by ID and support fast lookup.

---

### 19. Final quick tips  
* Use dictionaries when fast key-based lookup is required.  
* Remember: keys must be hashable + unique.  
* `.get()` is safer than `D[key]` when key may not exist.  
* Python dicts from 3.7 maintain insertion order—but don’t rely on ordering for algorithmic logic unless specified.  
* Use comprehensions and built‑in methods to write clean, efficient code.

---