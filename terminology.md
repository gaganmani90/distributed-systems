# CAP Theorem Techniques
## Availability 
* Quorum: Maintains enough number of R/W nodes as per R + W > N to tolerate temporary failure. 
* Leader and Follower: Offloads responsiblity from client to Leader to update Quorum nodes
* Heartbeat: Detects permanent failure and replaces the node with healthy ones. 
* Gossip Protocol: Algorithm to implement Heartbeat efficiently. Each node keeps track of state information about other nodes in the cluster and gossip (i.e., share) this information to one other random node every second. This way, eventually, each node gets to know about the state of every other node in the cluster.
* Phi Accrual Failure Detection: Real failure detected by heartbeat OR a genuine slow server ? This algo considers historical heartbeat data to make the timeout threshold adaptive. It returns probability of server crash rather than a boolean value like heartbeat.
* Lease: Distributed system locking to prevent resource unavailability due to deadlock, process crash, etc. As opposed to client control lock leasing; this is time based lock leasing. 
* Split Brain: A leader was incorrectly declared dead (due to GC paude, network interruptions, etc) but it (zombie leader) came back online after a new leader was choosen; resulting the system with two active leaders. As a solution, a genration number is attached with every leader, nodes receiving requests from leaders can detect who is the real leader (higher the number, real the leader). Generation number is persisted in WAL. The number can be included in gossip message, if the number in message is higher - it recepient node will know that the server is restarted. 
* Fencing: Prevent zombie leader from corrupting the system by either revoking access to shared resource or power off/reset the node. 

## Consistency
* Write-Ahead Log: Each node writes to a log before it writes to the system. Quorum replicates the log. 
* Segmented Log: Split large log files into smaller files. 
* High-Water mark: A mark until system allows clients to read to maintain concistency. In case, leader goes down and WAL are not properly replicated to all other nodes. 
* Checksum: Calculate a checksum and store it with data so that client can assure data integrity while fetching, if corrupted then fetch from another node.
* Vector clock: Use Vector clocks to keep track of value history and reconcile divergent histories at read time. A vector clock is effectively a (node, counter) pair
* Hinted Handoff: Backfill the recovering node with the data that it lost while it was gone to maintain consistency. When a node is down or is not responding to a write request, the node which is coordinating the operation, writes a hint in a text file on the local disk. This hint contains the data itself along with information about which node the data belongs to. When the coordinating node discovers that a node for which it holds hints has recovered, it forwards the write requests for each hint to the target.
* Read Repair & Markle Trees: Repair stale data during the read operation, since at that point, we can read data from multiple nodes to perform a comparison and find nodes that have stale data. System uses checksum to identify inconsistency, if yes - then perform a read repair with latest data across all Quorum read nodes. When failure is long lasting, then use Markle Trees data structure to compare data copies for Synchronizing divergent replicas in the background. 

## Partition 
* Consistent Hashing: Parition data by using CH so that it is faster to add or remove a node. Each node does maintain multiple VNodes to divide keys by range. 
* Bloom Filters: Even with CH, searching is hard. Hence Bloom Filters gives you faster search result with a possibility of false positive. 

## Examples 

