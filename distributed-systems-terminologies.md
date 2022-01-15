## Availability 
* Quorum: Maintains enough number of R/W nodes as per R + W > N to tolerate temporary failure. 
* Leader and Follower: Offloads responsiblity from client to Leader to update Quorum nodes
* Heartbeat: Detects permanent failure and replaces the node with healthy ones. 
* Lease: Distributed system locking to prevent resource unavailability due to deadlock, process crash, etc. As opposed to client control lock leasing; this is time based lock leasing. 

## Consistency
* Write-Ahead Log: Each node writes to a log before it writes to the system. Quorum replicates the log. 
* Segmented Log: Split large log files into smaller files. 
* High-Water mark: A mark until system allows clients to read to maintain concistency. In case, leader goes down and WAL are not properly replicated to all other nodes. 

## Partition 
* Consistent Hashing: Parition data by using CH so that it is faster to add or remove a node. Each node does maintain multiple VNodes to divide keys by range. 
* Bloom Filters: Even with CH, searching is hard. Hence Bloom Filters gives you faster search result with a possibility of false positive. 
* 
