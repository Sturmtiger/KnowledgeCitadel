#python #GIL #consume

https://realpython.com/python-gil/
https://wiki.python.org/moin/GlobalInterpreterLock
---
In CPython, the **Global Interpreter Lock (GIL)** is a mutex that allows only one thread to run at any time.
The GIL protects access to Python objects, preventing multiple threads from executing Python bytecodes at once.
The GIL must be held by the thread before it can safely access Python objects.
Without the lock, even the simplest operations could cause problems in a multi-threaded program: for example, when two threads simultaneously increment the reference count of the same object, the reference count could end up being incremented only once instead of twice.

[[CPython's memory management is not thread-safe]]
[[The GIL can degrade performance]]
[[CPython extensions must be GIL-aware]]
[[Blocking operations outside the GIL]]
[[Python3.13t without GIL]] ==to delete==

Python uses reference counting for memory management. It means that objects created in Python have a reference count variable that keep track of the number of references that point to the object. When this count reaches zero, the memory occupied by the object gets released.