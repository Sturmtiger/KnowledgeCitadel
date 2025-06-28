Python threads share the same memory.
With multiple threads running simultaneously, we don't know the order in which the threads access shared data.
Threads are "racing" to access/change the data.

The [[Python Global Interpreter Lock (GIL)|GIL]] was invented because CPython's memory is not thread-safe. With only one thread running at a time, CPython can rest assured there will never be [[Race condition|race conditions]].

## An example of race condition
This is what would hypothetically happened if the GIL were gone.
```python
a = 2
```
Let's suppose we have 2 threads performing following operations:
- **thread_1**: a = a + 2
- **thread_2**: a = a * 3

The result would depend on which thread accesses the variable `a`  first:
If **thread_1**:
- a = 2 + 2, `a` is 4.
- a = 4 * 3, `a` is 12.
If **thread_2**:
- a = 2 * 3, `a` is 6.
- a = 6 + 2, `a` is 8.