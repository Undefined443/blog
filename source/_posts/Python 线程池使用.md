线程池（Thread Pool）是管理和重用一组线程的机制，它能有效地限制线程的数量，减少线程创建和销毁的开销，提高程序的效率。Python 的 `concurrent.futures` 模块提供了一个高层次的接口来使用线程池。下面是如何使用线程池的一些基本介绍和示例。

1. 导入必要的模块

   首先你需要导入`ThreadPoolExecutor`类，这个类在`concurrent.futures`模块中：

   ```python
   from concurrent.futures import ThreadPoolExecutor
   ```

2. 创建线程池

   你可以通过实例化`ThreadPoolExecutor`来创建一个线程池，线程池的大小可以通过指定`max_workers`参数来设置：

   ```python
   executor = ThreadPoolExecutor(max_workers=5)
   ```

3. 提交任务

   可以通过`submit`方法将任务提交给线程池，`submit`方法接受一个可调用对象（如函数）和它的参数：

   ```python
   def task(n):
       print(f'Processing {n}')
       return n * 2

   future = executor.submit(task, 5)
   ```

4. 获取结果

   `submit`方法返回一个`Future`对象，通过这个对象你可以获取任务的执行结果：

   ```python
   result = future.result()
   print(result)  # 输出 10
   ```

5. 使用上下文管理器

   为了确保线程池在使用完毕后能正确关闭，你可以使用上下文管理器：

   ```python
   with ThreadPoolExecutor(max_workers=5) as executor:
       futures = [executor.submit(task, i) for i in range(10)]
       results = [future.result() for future in futures]
       print(results)
   ```

6. 使用`map`方法

   `ThreadPoolExecutor`还提供了一个`map`方法，它能将一个可迭代对象中的每个元素传递给一个函数，并返回结果的迭代器：

   ```python
   def square(n):
       return n * n

   with ThreadPoolExecutor(max_workers=5) as executor:
       results = list(executor.map(square, range(10)))
       print(results)  # 输出 [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
   ```

### 完整示例

以下是一个完整的示例，演示如何使用线程池来并发地执行多个任务：

```python
from concurrent.futures import ThreadPoolExecutor

def task(n):
    print(f'Processing {n}')
    return n * 2

# 创建线程池
with ThreadPoolExecutor(max_workers=5) as executor:
    # 提交多个任务
    futures = [executor.submit(task, i) for i in range(10)]

    # 获取任务结果
    results = [future.result() for future in futures]
    print(results)  # 输出 [0, 2, 4, 6, 8, 10, 12, 14, 16, 18]
```

这个示例创建了一个包含 5 个工作线程的线程池，然后提交了 10 个任务，最后收集并打印这些任务的结果。
