## Why Aren't We SIEVE-ing?
Link: https://brooker.co.za/blog/2023/12/15/sieve.html

* SIEVE is a cache eviction algoritm, or a way to decide which cached item to evict when the cache is full.
* Based on a FIFO queue eviction policy, but doesn't require changing the queue order on access, which reduces the need to synchronize in a multi-tenant environment.
* SEIVE is not scan-resistant. This can be done by replacing the access boolean with a counter. The counter is incremented rather than set on access and decremented when scanned.
* Experiments with workloads show SIEVE-K not a silver bullet, but it seems to outperform SIEVE in many cases.

## It's About Time!
Link: https://brooker.co.za/blog/2023/11/27/about-time.html

* Wall-clock physical time is for humans and not for machines.
* How we entangle physical time more deeply in our system designs:
 * Level 0: Observability, and the Amusement of Humans.
	 * Typically used in the troubleshooting of issues by finding causality in logs.
 	 * Accurate clocks make observing systems significantly easier.
 * Level 1: A Little Smarter about Wasted Work.
	 * Marking work with an expiry time, or TTL, to indicate when the system should not bother continuing with work.
	 * 