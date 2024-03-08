## EBS 

Elastic Block Store is a network drive we can connect while they run.

Through EBS  even after instance termination data can be persisted.

We get 30Gb of free storage of  General purpose (SSD) or magnetic type / month. 

 
It uses network to communicate with instance so there is a bit of latency.

It is locked to a availability zone to move across we have to use snap shots of EBS.

## EBS Volume Types

1. gp2/gp3 (SSD) General Purpose SSD volumes thta balances price and performance for wide variety of work loads

2. io1/io2 (SSD) High performance SSD volumes high throughput workloads.

3. st1 (HDD) Low cost HDD volumes  designed for frequently accessed through put workloads.

4. sc1 (HDD) Low cost HDD volumes  designed for frequently accessed through put workloads.


## EFS
Elastic File System is managed network file system.

EFS works with EC2 in multiple AZ's , highly available, scalable and pay per use.



