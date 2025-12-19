# ⏳ Async Python Patterns for High-Performance Data Processing

## Why Async?

Traditional synchronous code waits for each IO operation to finish before moving on. This causes delays and underutilization of resources when working with data fetching, API calls, or database operations. Async programming enables concurrency without threading, making your data pipelines faster and more efficient.

## 🧠 Core Async Concepts

- **Coroutines:** Special functions defined with `async def` that can pause and resume.
- **Awaitables:** Objects you can `await` to yield control until the operation completes.
- **Event Loop:** Core of async runtime managing execution and switching between coroutines.
- **Tasks:** Wrappers for coroutines scheduled concurrently on the event loop.

## 🛠️ Example 1: Concurrent HTTP Requests with `aiohttp` 🌐

Fetch multiple URLs simultaneously to speed up data ingestion.

```python
import asyncio
import aiohttp

async def fetch(session, url):
    async with session.get(url) as response:
        return await response.text()

async def fetch_all(urls):
    async with aiohttp.ClientSession() as session:
        tasks = [asyncio.create_task(fetch(session, url)) for url in urls]
        results = await asyncio.gather(*tasks)
        return results

urls = [
    'https://jsonplaceholder.typicode.com/posts/1',
    'https://jsonplaceholder.typicode.com/posts/2',
    'https://jsonplaceholder.typicode.com/posts/3',
]

results = asyncio.run(fetch_all(urls))

for i, content in enumerate(results):
    print(f"Response {i+1} snippet: {content[:100]}...\n")
```

**What’s happening?**

- `aiohttp.ClientSession` manages connection pooling.
- `asyncio.create_task` schedules all fetches concurrently.
- `asyncio.gather` waits for all tasks to complete and collects results.

## 🛠️ Example 2: Async Data Processing Pipeline ⛓️

Simulate a multi-stage pipeline with async functions to improve throughput.

```python
import asyncio

async def stage_1(data):
    print(f"Stage 1 processing {data}")
    await asyncio.sleep(1)  # simulate async IO-bound work
    return data * 2

async def stage_2(data):
    print(f"Stage 2 processing {data}")
    await asyncio.sleep(1)
    return data + 3

async def process_pipeline(data):
    result1 = await stage_1(data)
    result2 = await stage_2(result1)
    return result2

async def main():
    # Schedule multiple pipeline runs concurrently
    tasks = [asyncio.create_task(process_pipeline(i)) for i in range(5)]
    results = await asyncio.gather(*tasks)
    print(f"Pipeline results: {results}")

asyncio.run(main())
```

**Takeaways:**

- Each stage simulates an IO wait with `asyncio.sleep`.
- Pipelines run concurrently thanks to `asyncio.create_task`.
- Results are collected and printed after all finish.

## 🧰 Advanced Patterns & Tips

### 1. Cancellation Handling

Make your coroutines cancellable to allow graceful shutdowns.

```python
async def cancellable_task():
    try:
        await asyncio.sleep(10)
    except asyncio.CancelledError:
        print("Task cancelled!")
        raise
```

### 2. Async Context Managers

Use `async with` for resources requiring async setup/teardown (like DB connections).

```python
class AsyncResource:
    async def __aenter__(self):
        print("Acquiring resource")
        return self

    async def __aexit__(self, exc_type, exc, tb):
        print("Releasing resource")

async def main():
    async with AsyncResource():
        print("Using resource")

asyncio.run(main())
```

### 3. Avoid Blocking Calls

Blocking calls like time.sleep or synchronous DB queries freeze the event loop. Always use their async equivalents.

## ⚠️ Common Pitfalls

- Forgetting to await async calls — leads to unexecuted coroutines.
- Mixing sync blocking code inside async functions.
- Running event loops inside already running loops (e.g., Jupyter notebooks).

## 📍 Summary

Async Python is a powerful tool for data engineers and product developers needing high concurrency with minimal threads. Mastering `asyncio` and async libraries like `aiohttp` opens doors to efficient data pipelines, web scrapers, and real-time data processing.
