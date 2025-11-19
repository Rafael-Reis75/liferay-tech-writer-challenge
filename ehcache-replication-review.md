## Enhanced Ehcache Cluster Replication (EE Feature)

This document explains the technical problems addressed by Liferay's new Ehcache cluster replication mechanism, which is exclusive to the Enterprise Edition (EE).

### The Problem with Default RMI Replication

By default, Ehcache uses Remote Method Invocation (RMI) for cluster replication. This **point-to-point** communication graph presents two major scalability issues in large Liferay Portal environments:

1.  **Network Overhead:** In a cluster with $N$ nodes, every node must send $N-1$ identical replication events. As the cluster size grows, this $1$ to $(N-1)$ communication model leads to a disastrous increase in network traffic.
2.  **Excessive Threads:** Ehcache creates a separate replication thread for every cache entity. In Liferay, this can easily result in over 100 replication threads per node. Since these threads are mostly idle, they needlessly consume system resources (CPU and memory stack).
    * Considering a default 2MB stack size, 100 threads require over 200MB of stack memory alone. This resource waste also introduces frequent context switching overhead.

### The Liferay Solution: ClusterLink

Liferay Portal addresses these bottlenecks by leveraging its existing **ClusterLink** facility, an abstract communication channel that uses JGroups' UDP multicast by default.

1.  **Network Fix:** Integrating ClusterLink immediately resolves the $1$ to $(N-1)$ network communication issue by using a more efficient multicast method.
2.  **Thread Fix and Coalescing:** We reduced thread overhead by introducing a small, dedicated group of **dispatching threads** responsible for all cache event delivery.
    * Centralizing event delivery allows for **coalescing**: if multiple modifications occur to the same cache object within a short window, the system sends only a single notification to remote peers, saving network bandwidth.

***

*(Note: While newer Ehcache versions support JGroups, they resolve only the network communication issue, failing to fix the massive thread bottleneck or provide coalescing.)**

**For EE customers interested in implementing this feature, please contact our support engineers for detailed configuration information.**
