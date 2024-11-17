* What are the different types of source mounts? For example in [this example](https://github.com/containerd/rust-extensions/blob/main/crates/client/examples/container_spec.json) we can see the following sources and destinations:
  * `proc`-> `/proc`
  * `tmpfs` -> `/dev` and `/dev/shm`
  * `devpts` -> `/dev/pts`
  * `mqueue` -> `/dev/mqueue`
  * `sysfs` -> `/sys`
  * `cgroup` -> `/sys/fs/cgroup`
* What do the various options mean which are used in mounts?
  * `nosuid` - Specifies tha the filesystem should be mounted with the `setuid` and `setgid` permissions disabled. When these bits are set, it allows users to execute the fail to temporarily gain permissions of the file's owner or group. This is typically set to help prevent privilege escalation attacks.
  * `noexec` - Specifies that the filesystem should be mounted with the execution of binaries disabled.
  * `nodev` - Specifies that the filesystem cannot contain special devices. Typically this is a security precaution as it prevents users for accessing or manipulating hardware devices.
  * `ro` - Mounts the filesystem as read-only.
  * `realtime` - Used to enable or disable real-time scheduleing for files on the mounted filesstem. Real-time scheduling allows certain processes to have higher priority access to CPU resources.
  * `mode=1777` - Speicies the permissions to be applied to the root directory of the mounted filesystem.
  * `size=65536k` - Maximum size of the filesystem in bytes.
  * `newinstance` - Specifies that the filesystem should be mounted as a new instance.
  * `ptmxmode=0666` - Specifies the permissions mode for the psudeo-terminal multiplexer device (/dev/ptmx).
  * `gid=5` - Specifies the group ID that should be used for the root directory of the mounted filesystem.
  * `strictatime` - Ensures access times of files are updated when it is actually read or executed.
* What are `maskedPaths`?
 * Paths which are hidden fron the container's view. Processes running within the container won't be able to access or see these paths.
* What are the following paths?
  * `/proc/kcore` - 
  * `/proc/latency_stats` - 
  * `/proc/timer_list` - 
  * `/proc/timer_stats` - 
  * `/proc/sched_debug` - 
  * `/sys/firmware` - 
  * `/proc/asound` -
  * `/proc/bus` -
  * `/proc/fs` -
  * `/proc/irq` -
  * `/proc/sys` -
  * `/proc/sysrq-trigger` -
* What is `types.containerd.io/opencontainers/runtime-spec/1/Spec`?