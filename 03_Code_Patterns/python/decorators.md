# 🐍 Python Decorators

Reusable Python decorators for common data engineering tasks such as logging, timing, retries, and error handling.

## 📜 What Are Decorators?
Decorators are a way to modify or enhance functions without changing their code. They wrap a function to inject pre/post behavior.

```python
def my_decorator(func):
    def wrapper(*args, **kwargs):
        print("Before function")
        result = func(*args, **kwargs)
        print("After function")
        return result
    return wrapper
```

## 🕒 Timing Decorator
Useful for profiling or tracking performance.
```python
import time
import functools

def timer(func):
    @functools.wraps(func)
    def wrapper_timer(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        duration = time.time() - start
        print(f"{func.__name__} ran in {duration:.4f}s")
        return result
    return wrapper_timer
```

## 📋 Logging Decorator
Standard logging of input/output.
```python
import functools
import logging

logging.basicConfig(level=logging.INFO)

def log_io(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        logging.info(f"Calling {func.__name__} with args={args} kwargs={kwargs}")
        result = func(*args, **kwargs)
        logging.info(f"{func.__name__} returned {result}")
        return result
    return wrapper
```

## ♻️ Retry Decorator
Automatic retries on failure.
```python
import time
import functools

def retry(retries=3, delay=2):
    def decorator(func):
        @functools.wraps(func)
        def wrapper(*args, **kwargs):
            for i in range(retries):
                try:
                    return func(*args, **kwargs)
                except Exception as e:
                    print(f"Attempt {i+1} failed: {e}")
                    time.sleep(delay)
            raise Exception(f"Failed after {retries} retries.")
        return wrapper
    return decorator
```

## 🚨 Error Handling Decorator
Gracefully handle and log exceptions.
```python
def catch_errors(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        try:
            return func(*args, **kwargs)
        except Exception as e:
            print(f"Error in {func.__name__}: {e}")
            return None
    return wrapper
```

## 🧪 Combine Decorators
Decorators can be stacked for reusable pipelines.
```python
@retry(retries=2)
@log_io
@timer
def fetch_data():
    # Your data fetching logic here
    ...
```

## ✅ Best Practices
- Use functools.wraps to preserve metadata.
- Isolate decorators into a utils/decorators.py file.
- Keep them composable and reusable.
- Log appropriately for prod/debug environments.

> 💡 Decorators abstract operational concerns like retries, timing, and logging — essential for reliable, observable code.
