Article: https://chrisdown.name/2018/01/02/in-defence-of-swap.html

* Disabling swap does not prevent Disk I/O from being a problem under memory contention. Instead it shifts the disk I/O thrashing from anonymous pages to file pages. This means we have a smaller pool o pages to select from and may contribute to getting into a high contention state.
    * Anonymous Pages - Memory which is loaded into main memory and does not have a corresponding file on disk.
    * File Pages - Amount of memory used to cache data loaded from disk.
* Swapping out anonymous pages and reclaiming file pages are equivalent in terms of performance and latency.
* You can achieve better swap behavior under memory pressure and prevent thrashing by utilizing memory.low
* Reclaim - The system can, without losing data, purge pages from physical memory without losing data. For example, clean page cache memory can be dropped without having to do anything special since same contents already live on disk. Dirty page cache memory cannot be reclaimed since the modified memory is not yet be persisted on disk. Anonymous memory cannot be reclaimed as it only exists in memory and has no other backing store.
* Swap enables memory reclaim for anonymous pages since it gives them a file-backed stored. This makes them equally eligible for reclaim as clean file pages. This enables more efficient use of available physical memory.
* Swap is a primary mechanism for equality of reclamation, not for emergency “extra memory”
* Cons: While swap will make us more resilient to temporary spikes, severe memory starvation and thrashing may prolong invoking the OOM killer.
* `memory.low` tells the kernel to prefer other applications for reclaim below a certain threshold of memory used. This allows us to prevent the kernel from swapping out parts of our application, but prefer to reclaim from other applications under memory contention.
* `vm.swappiness` is a sysctl setting that biases memory reclaim either towards reclamation of anonymous pages, or towards file pages. IT accomplishes this by adjusting two separate attributes: `anon_prio` (willingness to reclaim anonymous pages) and `file_prio` (willingness to reclaim file pages). The default value of `file_prio` is 200, so `vm.swappiness` will be subtracted from 200 to set the new `file_prio` value and used as the value for `anon_prio`. ie: `vms.swappiness = 50` => `anon_prio = 50` and `file_prio` = 150.
* This means `vm.swappiness` is a ratio of how costly reclaiming and refaulting anonymous memory is compared to file memory for your hardware and workload.