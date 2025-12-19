# 🐍 Advanced Python Data Hacks

Supercharge your data workflows with lesser-known Python techniques that help you write more concise, efficient, and powerful data code.

## ⚡ Lazy Data Loading with Generators
When working with large files (like logs or event dumps), you can save memory using generators instead of loading everything at once.

```python
def lazy_load(filepath):
    with open(filepath) as f:
        for line in f:
            yield line.strip()

for row in lazy_load("events.log"):
    process(row)
```

🧠 Why it matters: Avoids memory bloat for big data files.

---

## 🧵 Named Tuples for Lightweight Structs

```python
from collections import namedtuple

Record = namedtuple('Record', ['id', 'value'])
r = Record(id=1, value=99)
print(r.id)  # 1
```

Useful for: Replacing lightweight dicts in data transformation steps.

---

## 🔁 Generator Pipelines

```python
def read_lines(path):
    with open(path) as f:
        for line in f:
            yield line.strip()

def filter_lines(lines):
    for line in lines:
        if "ERROR" in line:
            yield line

pipeline = filter_lines(read_lines("logs.txt"))
for err in pipeline:
    print(err)
```

Efficient for handling large files in constant memory.

---

## 🔎 Introspection for Debugging Pipelines

```python
def log_caller():
    import inspect
    frame = inspect.currentframe().f_back
    print(f"Called from: {frame.f_code.co_name}")

def transform():
    log_caller()

transform()  # Output: Called from: transform
```

Helps in tracing where your functions are triggered in DAGs or services.

---


## 🚀 High-Performance Parsing with `pandas.read_fwf()`

```python
import pandas as pd

df = pd.read_fwf("data.txt", colspecs=[(0, 10), (10, 20)])
print(df.head())
```

Useful when ingesting legacy column-based flat files.

---

## 🧹 Clean One-Liners with List Comprehensions & Conditionals

Replace clunky for-loops with elegant list comprehensions.

```python
# Filter and lowercase only valid strings
cleaned = [s.lower() for s in raw_strings if isinstance(s, str)]
```

🔥 *Tip:* Combine `.strip()` or `.replace()` inline for power combos.

---

## 🪄 Use `functools.lru_cache` for Fast Lookups

Avoid redundant expensive computations with automatic memoization.

```python
from functools import lru_cache

@lru_cache(maxsize=256)
def get_user_features(user_id):
    return fetch_from_slow_api(user_id)
```

💡 *Great for:* Feature engineering, lookup-heavy transformations.

---

## 🧊 Dataclasses for Lightweight Data Models

Need structured data classes without the boilerplate?

```python
from dataclasses import dataclass

@dataclass
class Record:
    id: int
    status: str
    created_at: str
```

🧩 *Alternative:* Use `pydantic` if validation is also needed.

---

## 🧼 Safe Parsing with `try/except` One-Liners

Handle dirty data without interrupting flows.

```python
def to_int(val):
    try:
        return int(val)
    except (ValueError, TypeError):
        return None
```

🛡️ *Pattern:* Wrap noisy transforms to preserve pipeline stability.

---

## 🧪 Quick Profiling with `cProfile`

Find bottlenecks without external tools.

```bash
python -m cProfile your_script.py
```

🕵️ *Use when:* Your data pipeline feels slow but you're unsure where.

---

## 💫 Bonus Tips

- Use `itertools.groupby` for efficient streaming aggregation.
- Use `defaultdict(list)` to simplify nested groupings.
- Use `.join()` + f-strings to build cleaner logs.

---

## 🔗 References

- [Python Data Model Reference](https://docs.python.org/3/reference/datamodel.html)
- [functools — Higher-order functions](https://docs.python.org/3/library/functools.html)
- [dataclasses in Python 3.7+](https://docs.python.org/3/library/dataclasses.html)
- [Real Python — namedtuple](https://realpython.com/python-namedtuple/)
- [Functools — lru_cache](https://docs.python.org/3/library/functools.html#functools.lru_cache)
- [Inspect module](https://docs.python.org/3/library/inspect.html)
- [Python `itertools` Recipes](https://docs.python.org/3/library/itertools.html#itertools-recipes)
- [Awesome Python](https://github.com/vinta/awesome-python)
