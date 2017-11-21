# Docker Swarm

### What Docker Swarm is, and why we need it

Docker Swarm makes it possible to run a cluster network with multiple Docker hosts. 
A host can be a manager, worker or both. Managers delegate tasks (containers) to the worker 
nodes. If a worker node fails, Docker will schedule that node’s task to other nodes. Docker 
Swarm comes with load balancing out of the box, which will make sure the workload is 
distributed between nodes.


### Why it can help us to eliminate bottlenecks

As mentioned, load balancing will distribute workload and increase the likelihood that 
an incoming request will be handled speedily. Heavy requests will not be as taxing, as 
they will only affect a single instance. Swarm also makes it possible to configure and 
create nodes at runtime, making it possible to actively put more resources in areas of 
the system that are taxed and need additional computing power.

## Running Docker Swarm

