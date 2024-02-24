# Distributed Web Infrastructure

![Image of a distributed web infrastructure](1-distributed_web_infrastructure.PNG)

## Description

This is a distributed web infrastructure designed to reduce traffic to the primary server by distributing some of the load to a replica server, aided by a load balancer responsible for balancing the load between the two servers (primary and replica).

## Specifics About This Infrastructure

+ **Load Balancer Distribution Algorithm:** The HAProxy load balancer is configured with the *Round Robin* distribution algorithm. This algorithm works by using each server behind the load balancer in turns, according to their weights. It’s a smooth and fair algorithm, ensuring equally distributed processing time. *Round Robin* is a dynamic algorithm, allowing server weights to be adjusted on the go.

+ **Load-Balancer Enabled Setup:** The HAProxy load balancer enables an *Active-Passive* setup rather than an *Active-Active* setup. In an *Active-Active* setup, workloads are distributed across all nodes to prevent any single node from overloading. This setup improves throughput and response times. In contrast, an *Active-Passive* setup has not all nodes active at all times. For example, if the first node is active, the second node must be passive or on standby, ready to become active if the preceding node is inactive.

+ **Primary-Replica (Master-Slave) Database Cluster:** A *Primary-Replica* setup designates one server as the *Primary* server and the other as a *Replica* of the *Primary* server. The *Primary* server handles read/write requests, while the *Replica* server only handles read requests. Data synchronization occurs whenever the *Primary* server executes a write operation.

+ **Difference Between Primary and Replica Nodes:** The *Primary* node handles all write operations for the site, while the *Replica* node processes read operations. This distribution decreases read traffic to the *Primary* node.

## Issues With This Infrastructure

+ **Multiple Single Points Of Failure (SPOF):** For example, if the Primary MySQL database server goes down, the entire site would be unable to make changes (including adding or removing users). The server containing the load balancer and the application server connecting to the primary database server are also SPOFs.

+ **Security Concerns:** Data transmitted over the network isn't encrypted using an SSL certificate, making it susceptible to spying by hackers. There is also no way of blocking unauthorized IPs since there's no firewall installed on any server.

+ **Lack of Monitoring:** Without monitoring, there's no way to know the status of each server, leaving potential issues undetected.
