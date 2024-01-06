**Link**: https://facebookmicrosites.github.io/cgroup2/docs/overview.html

* The cgroup architecture has two main components: the **core** and subsystem **controllers** for CPU, IO, PID, and RDMA.
* The core is where you group processes into logical units and define hierarchical relationships. 
* All cgroups on the system for a signle hierarchy or tree. It is comprised of the root cgroup with children. Controlled via a psuedo-filesystem (cgroupfs) located at `/sys/fs/cgroup`. New cgroups are created by created by creating a subdirectory.
* Controllers manage the distribution of their respective resources along the hierarchy according to their configuration.
* A process can be added to a cgroup by writing its PID into the cgroup's `cgroup.procs` file: `echo 24982 > /sys/fs/cgroup/cg1/cgroup.procs`. A process can be in only one cgroup at a time.
* `cgroup.controllers` - List the controllers available in a cgroup. The root cgroup has all cgroups enabled.
* `cgroup_subtree_control` - Lists the controllers that are enabled in the cgroups's subtrees.
* A cgroup can either contain tasks or control resources for child cgroups, but not both.
* Pressure Stat Information (**PSI**) metrics provide a canonical way to see resource pressure increases as they develop. PSI information is tracked for CPU, Memory, and IO.
* Memory and IO show metrics for `some` and `full` with running averages over the last 10s, 1m, and 5m. The total is the accumulated microseconds.
 * `some` - Percentage of walltime that **some** tasks were delayed due to lack of resources.
 * `full` - Percentage of walltime that **all** tasks were delayed by lack of resources.
* Memory Controller
 * `memory.high` - Memory usage throttle limit. If a cgroup's memory use goes over the high boundary specified here, the cgroup's processes are throttled and put under heavy reclaim pressure. 
 * `memory.max` - Memory usage hard limit. If a cgroup's memory usage reaches this limit and can't be reduced, the system OOM killer is invoked on the group. When used with `memory.high` this acts as a safety net since cgroup usage may go over the high limit.
 * `memory.low` - A best-effort memory protection, or "soft guarantee" that the cgroup and all of its descendents are below this threshold. The cgroup's memory won't be reclaimed unless memory can't be reclaimed from any unprotected groups.
 * `memory.min` - Specifies a minimum amount of memory the cgroup must always retain, which can never be reclaimed by the system.
 * `memory.swap.max` - Swap usage hard limit. If a cgroup's swap usage reaches this limit, anonymous memory of the cgroup will not be swapped out.
 * The problem with `memory.high` is that it made processes more prone to thrashing and OOMs. Instead `memory.low` provides a soft-guarantee memory to cgroups.
 * In order to find an optimal `memory.low` setting, you may need to first determine a working set size for the memory. This can be done by applying memory pressure and read `memory.current` on a number of machines and averaging the result. 
* Memory overcommit is a strategy based on the assumption that not all of the assigned memory will be needed at the same time. When demand exceeds the total memory available, the system OOM handler tries to reclaim memory by killing a process.
* Load shedding is a technique to avoid overloadidng and crashing a system by temporarily rejecting new requests. The assumption is that all loads will be better served if the system rejects a few and continues to run.
* [oomd](https://facebookincubator.github.io/oomd) is a userspace tool, similiar to the kernel OOM handler, but uses memory pressure to provide greater control over when processes start getting killed and which processes are selected.
* The kernel's OOM killer main job is to protect the kernel. It is triggered only after failing at multiple attempts to allocate memory. It will typically kill whichever process will free the most memory and can fail when the system is thrashing.
* IO Controller
 * Cgroups1 accounts for only direct IO and buffered reads. This does not account for buffered IO.
 * Cgroups2 provides comprehensive control and accounting of all IOs per-cgroup: buffered, filesystem metadata, swap, and direct IOs.
 * `io.latency` - A mechanism to guarantee a cgroup's level of IO completion latency. If the average completition latency are longer than the target set here, other processes are throttled to provide more IO.
 * `io.pressure` - Gives the percentage of wall time in which some or all tasks are waiting for a block device, or IO.
 * `io.stat` - IO usage statistics
 * `io.max` - Specify IO limits.
* Workloads are protected by specifying a latency target with `io.latency`. If the protected workload experiences average completion latency longer than its latency target value, the controller throttles any peers that have a more relaxed latency target than the protected workload.
* 