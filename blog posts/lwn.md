## Reconsidering Swap
Link: https://lwn.net/Articles/690079/

* Memory on a linux system is divided into two broad classes:
 * File-backed - Corresponds to a segment of a file on desk. These pages can easily be reclaimed if they do not contain newly written data that has not yet made it back to persistent storage.
 * Anonymous - Do not corespond to a file on disk as they hold run-time data generated and used by a process. Reclaiming an anonymous page requires writing to swap.
* It is generally considered slower to reclaim anon vs file memory as file memory can be read from storage in large contiguous chunks,  while anon pages tend me scattered randonly on the swap device.
* Not setting up swap will deprive thekernel of any way to reclaim anon pages, regardless of whetehr that memory could be put to better use elsewhere.
* A swappiness of 100 means reclaim anonymous and file-backed pages equally.
* Reclaim logic will perform a scan of the LRU cache of pages and unset the referenced bit on each page. Any pages used thereafter will have that bit sit again. Pages which still do not will be the first to be rclaimed.
* A larger page cache will mean a longer amount of time that is used to scan for pages. 


## Resource Control at Facebook
Link: https://lwn.net/Articles/764761/

* Work conserving - Machines stay busy. They do not go idle if there is work to do.
* `memory.high` and `memory.max` are not work conserving as creating artficial limits on memory led to reduced performance and more fragile systems.
* The kerne's OOM killer does not protect the health of the workload and cannot detect if a workload is thrashing; it can only detect if the kernel can make progress. This means if a workload is thrashing, but the kernel is not directly affected, then the OOM killer will not run.
* Priority Inversion - A situation in which a lower-priority task holds a resource that a higher-priority task needs. This can result in a scenario where the higher-priority task is blocked and unable to proceed, even though it should take precedence.
* Priority inversions occur when low-priority cgroups get throttled, but generate I/O from filesystem operations or from swapping.
* `memory.low` and `memory.min` provide more work conservation because there are no artificial limits. If a cgroup needs more more memory and it is available, then the cgroup should get that memory.
* PSI (Pressure Stall Information) can help answer "If I had more of this resource, I might have been able to run this percentage faster". 
* PSI is used by OOMD (a user-space OOM killer) to check the health of the system; it will remediate the problem before the kernel OOM killer gets involved.
* The team moved to BTRFS to fix priority inversion. They added aborts for page-cache readahead to solve issues with `mmap_sem` contention.
	* `mmap_sem` is a lock used in the memory management subsystem which has known scalability problems.
	* It is used to control access to the memory mapping data structures in the kernel (ie: the `mmap` system call).
* Without swap enabled, all anonymous memory becomes "memlocked".