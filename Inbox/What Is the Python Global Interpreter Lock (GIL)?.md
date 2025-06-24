#python #article #consume

[The link to the article](https://realpython.com/python-gil/)

==^This article might not cover all details.
Understand better why the GIL is a problem and when threading/multiprocessing might have no effect or only limited effect.==

Because of the GIL multi-threading cannot actually be used.
Using multiprocessing might be **expensive** and it is a workaround.
We have to resort to C-libs like: NumPy, Cython, PyTorch, to circumvent restrictions???


```
import threading, time

def cpu_task(n):
    s = 0
    for i in range(n): s += i*i

N = 100_000_000

def run():
    threads = [threading.Thread(target=cpu_task, args=(N,)) for _ in range(4)]
    start = time.time()
    for t in threads: t.start()
    for t in threads: t.join()
    print(f'{time.time() - start:.2f} сек')

run()
```

Python 3.13: `~13.5 s` 
Python 3.13t (without GIL): `~3.7 s`

[[Python Global Interpreter Lock (GIL)]]