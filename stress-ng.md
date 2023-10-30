## stress

## stress-ng
### Description
* Measures the system's ability to maintain a level of efficiency under unfavorable conditions.
* stress workload generator to load and stress all kernel interfaces.
* Over 270 different tests including CPU tests that exercise floating point, integer, bit manipulation, control flow, and memory tests.
### Example Tests
#### CPU
* Floating Point Unit (**FPU**) handles mathematical operations using floating point numbers.
~~~
stress-ng --matrix 1 -t 1m
~~~
Test the floating point on one CPU for 60 seconds.
* `--matrix <N>` - Starts N workers exercising matrix operations.
* `-t <T>` - Timeout after T seconds.


~~~
stress-ng --mq 0 -t 30s --times --perf
~~~
This will test message passing between processes using a POSIX message queue for 30s. This aims for low data cache misses.
* `--mq <N>` - Start N workers passing messages uisng POSIX.
* `--times` - Show run time summary at the end of the run.
* `--perf` - Display performance statistics.

~~~
stress-ng --cpu 2 --matrix 1 --mq 3 -t 5m
~~~
This shows you can run multiple workers running multiple tests.

#### Memory
~~~
stress-ng --vm 2 --vm-bytes 2G --mmap 2 --mmap-bytes 2G --page-in
~~~
This will test memory pressure on a system with 4 GB of memory between 2 workers calling mmap.
* `--vm <N>` - Start N workers spinning on anonymous mmap.
* `--vm-bytes <N>` - Allocate N bytes per vm worker (Default 256MB).
* `--mmap <N>` - Start N workers stressing mmap and munmap.
* `--mmap-bytes <N>` -  mmap and munmap N bytes for each stress iteration.
* `--page-in` - touch allocated pages that are not in core.

mmap / munmap - Syscalls to map or unmap files or devices into memory.