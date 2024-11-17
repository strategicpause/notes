* **2014-**. https://github.com/google/cadvisor/pull/93
* **2015-**. https://github.com/google/cadvisor/issues/685
* **2016-07-07**. https://github.com/kubernetes/kubernetes/issues/28619
* **2016-07-15**. cAdvsior changed the working set memory calcuation from `usage - total_inactive_anon - total_inactive_file` to `usage - total_inactive_file` since the previous calculation.
  * PR: https://github.com/google/cadvisor/pull/1380
  * Commit: https://github.com/google/cadvisor/commit/307d1b1cb320fef66fab02db749f07a459245451
* **2017-05-13**. Cache introduced in Docker. https://github.com/docker/cli/pull/80. 
* **2018-05-11**. Containerd followed suit by following cadvisor
  * PR: https://github.com/containerd/cri/pull/766
  * Commit: https://github.com/containerd/cri/commit/5d29598a6d5db2405befb15c83c7f95cd42ae5fe
* **2020-03-19**. Docker followed suit.
  * PR: https://github.com/moby/moby/issues/40727

Notes

* In a NUMA system, banks of memory may be assigned to each CPU. There is a cost to accessing memory, depending on the “distance” from the CPU.
* Zone - Represent ranges within memory.
  * ZONE_DMA - Memory in the lower physical memory ranges.
  * ZONE_NORMAL - Directly mapped by the kernel into the upper region of the linear address space. Some kernel operations can only take place here.
  * ZONE_HIGHMEM - The remaining available memory in the system and is not directly mapped by the kernel.
* Watermarks - Helps keep track how much pressure a zone is under.
  * `pages_low` - When this watermark is reached, kswapd is woken up and starts to free pages. This is twice the value of pages_min by default.
  * `pages_min` - The allocator will do the kswapd work in a synchronous fashion. This is referred to as the direct-reclaim path.
  * `pages_high` - This is the value when the zone is “balanced” and kswapd will go back to sleep. By default this is 3x pages_min.
  * https://www.kernel.org/doc/gorman/html/understand/understand005.html
* What is the `watermark_boost_factor`?
  * Used to help reduce the probability of an external fragmentation event. 
  * sysctl setting that allows a zone watermark to be temporarily boosted when an external fragmentation event occurs. This will help stall allocations that would decrease free memory below the boosted low watermark.
  * https://github.com/torvalds/linux/commit/1c30844d2dfe272d58c8fc000960b835d13aa2ac
* How does the kernel calculate memory?
  * Sum the low watermark values for each zone (including boost_factor.
  * Calculate the number of free pages across all zones.
  * Take the average size of the page cache (active file + inactive file). The assumption is that freeing the entire page cache will lead to swapping / thrashing. 
  * Subtract either this average or the low watermark low values (whichever is lower) from the page cache memory. Add the final resulting value to available memory.
  * Find the amount of memory which is reclaimable from the kernel (slab + misc). Subtract either the average or watermark_low - whichever is lower. Add this to the available memory.
  * If the final value is < 0, then report 0.

  

