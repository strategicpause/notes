* **What is steal time?** The percentage of time a virtual CPU involuntarily waits on a physical CPU for its processing time.
* **What is the run queue?** A queue of processes in a runnable state. This includes running tasks.
* **What is the run queue length?** 
* **How do you interpret `/proc/loadavg` output?** 

~~~~
$ cat /proc/loadavg 
25.72 23.19 23.35 42/3411 43603
~~~~
* 25.72 - Avg number of jobs in the run queue (state R) or waiting for disk I/O (state D) for the last minute period.
* 23.19 - ... for the last five minute period.
* 23.35 - ... for the last ten minute period.
* 42 - currently running process.
* 3411 - Total number of processes.
* 43603 - Last process ID used.
* Since the 1m avg is greater than the 5m and 10m avg, this indicates that load is increasing.