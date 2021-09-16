```python
from multiprocessing import shared_memory
import psutil

def print_mem_stats() -> None:
  mem_stats = psutil.virtual_memory()
  print("Available:", mem_stats.available / 2**20)
  print("Shared:", mem_stats.shared / 2**20)
  print("Cached:", mem_stats.cached / 2**20)

def main() -> None:
  print("-- Before opening shared memory --")
  print_mem_stats()
  shm_size = 2**30 
  shm_a = shared_memory.SharedMemory(create=True, size=shm_size)
  buffer = shm_a.buf
  for i in range(shm_size):
    buffer[i] = 1
  print("-- After accessing shared memory --")
  print_mem_stats()
  shm_a.close()
  shm_a.unlink()

if __name__ == '__main__':
  main()
```
```
-- Before opening shared memory --
Available: 13041.63671875
Shared: 0.56640625
Cached: 2901.5390625
-- After accessing shared memory --
Available: 12011.625
Shared: 1024.51171875
Cached: 3925.75390625
```

```
dd if=/dev/zero of=mmap.dat  bs=1G  count=1
```

```python
import mmap
import psutil
import os

def print_mem_stats() -> None:
  mem_stats = psutil.virtual_memory()
  print("Available:", mem_stats.available / 2**20)
  print("Shared:", mem_stats.shared / 2**20)
  print("Cached:", mem_stats.cached / 2**20)
  print("Buffers:", mem_stats.buffers / 2**20)

def main() -> None:
  print("-- Before opening shared memory --")
  print_mem_stats()
  mm_size = 2**30
  with open("mmap.dat", "r+b") as f:
    mm = mmap.mmap(f.fileno(), 2**30, flags=mmap.MAP_SHARED)
    mm.read(2**30)
    print("-- After accessing shared memory --")
    print_mem_stats()
    mm.close()

if __name__ == '__main__':
  main()
```

```
-- Before opening shared memory --
Available: 8276.15625
Shared: 0.56640625
Cached: 721.35546875
Buffers: 12.40234375
-- After accessing shared memory --
Available: 5336.5703125
Shared: 0.56640625
Cached: 1749.42578125
Buffers: 19.6953125
```

```python
import mmap
import psutil
import os

def print_mem_stats() -> None:
  mem_stats = psutil.virtual_memory()
  print("Available:", mem_stats.available / 2**20)
  print("Shared:", mem_stats.shared / 2**20)
  print("Cached:", mem_stats.cached / 2**20)
  print("Buffers:", mem_stats.buffers / 2**20)

def main() -> None:
  print("-- Before opening shared memory --")
  print_mem_stats()
  mm_size = 2**30
  with open("mmap.dat", "r+b") as f:
    mm = mmap.mmap(f.fileno(), 2**30, flags=mmap.MAP_PRIVATE)
    mm.read(2**30)
    print("-- After accessing shared memory --")
    print_mem_stats()
    mm.close()

if __name__ == '__main__':
  main()
```

```
-- Before opening shared memory --
Available: 13206.79296875
Shared: 0.56640625
Cached: 848.71484375
Buffers: 61.17578125
-- After accessing shared memory --
Available: 13203.22265625
Shared: 0.56640625
Cached: 1881.8515625
Buffers: 66.890625
```
