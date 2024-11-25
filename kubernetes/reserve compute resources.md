Ref: https://kubernetes.io/docs/tasks/administer-cluster/reserve-compute-resources/


### Notes
* By default, pods can consume all capacity on a node. This is a problem since there are daemons that run both the OS & k8s itself. Unless resources are explicitly set aside for these daemons, then you risk starving the node for resources.
* The kublet has a feature called "Node Allocatable" to reserve resources for system daemons.
* **Allocatable** - The amount of comput resources on a node which are available for pods. 
 * The scheduler does not over-subscribe allocatable CPU, memory, and ephemeral-storage.
* **`kube-reserved`** - Meant to capture resource reservations for the k8s system daemons such as the kubelet, container runtime, node problem detector, etc.
* **`system-reserved`** - Meant to capture resource reservation for OS system daemons like sshd, udev, etc. This should reserve memory for the kernel since it is not accounted to pods.

### Questions
* **What is `udev`?** A userspace system that enables the OS Admin to register userspace handlers for events. These events are primarily generated by the Linux kernel in response to physical events relating to peripheral devices (ie: hot-plugging). **Source**: https://wiki.archlinux.org/title/udev