### "Clusters, Servers, Instances, and Virtual Machines: Basics of Distributed Infrastructure"

**Physical Machines**
- Actual computer. It has dedicated hardware and OS runs directly on the hardware. Usually specialized differently to fit use cases like Kafka and Redis.
	- Kafka: high disk throughput, low latency networking, high memory
	- Redis: fast SSDs, lots of memory, low-latency networking

**Virtual Machines**
- Software emulation of a physical computer that runs on a physical machine. A physical machines can run multiple virtual machine so virtual machines to tend to have lower performance.
- Runs on a Hypervisor which is a layer of software that allows multiple VMs to share the same physical machine's resources.
- Each VM runs it's own OS.

**Instance**
- An instance is a virtual or physical machine running an operating system and applications. It's also referred to as a "server" because it serves some function, like running a webapp, database, or message broker.
- Instance:server isn't necessarily a 1:1 mapping though. An instance (like my physical computer) can have multiple servers running.
- So what does "spinning up a server" mean?
	- In this context, a server is "process" that listens for requests on a port like "localhost:3000" (e.g., Express sever, PostgreSQL server, Flask, etc.).
	- The process runs in an instance's operating system and uses resource.
- How does an instance run multiple servers?
	- Each "server" or "process" is managed by the OS and it schedules them to run on the CPU and allocates memory as needed.
	- A single CPU core typically as 2 threads. Modern OS use task scheduling so multiple servers can run concurrently.

**Cluster**
- A cluster is a group of instances working together to achieve a common goal such as kafka cluter for distributed messaging and redis cluster for distributed caching.

