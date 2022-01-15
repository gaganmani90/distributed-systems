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

## Partition 
* Consistent Hashing: Parition data by using CH so that it is faster to add or remove a node. Each node does maintain multiple VNodes to divide keys by range. 
* Bloom Filters: Even with CH, searching is hard. Hence Bloom Filters gives you faster search result with a possibility of false positive. 
* 
