- functional
	- users can schedule jobs to be immediately or at a future date
	- users can monitor the status of their jobs
- non-functional
	- availability > consistency
	- scale to 10k/s
	- ensure at-least-once guarantee
		- ensures that jobs are not lost and are executed even in case of failures - availability
	- *I knew availability > consistency, but failed to identify at-least-once guarantee*
- entities
	- user
	- job
	- task
	- *I knew to include job, but forgot about user and task*
- apis
	- POST /jobs -> job
		- body: {content: string, tasks: task_id, executionTime: datetime}
	- GET /jobs -> job\[\]
		- header: {user_id}
- high level
	- How will jobs be scheduled and executed?
		- - design does not persist the job data
		- - design does talk about how it will be executed, but could be unnecessarily complex
	- How will users view the status of all of their scheduled or executed jobs?
		- Redis does store job information and is fast, but it does not persist it
		- Lacks a mechanism for updating job statuses after execution. This is important since users may receive outdated for incorrect information
		- No clear way to query for job statuses, especially for completed jobs that may no longer be in Redis
		- We need to use a persistent storage solution. This ensures that users can query the status of their jobs even after redis expiration.
		- Use Global Secondary Index [[dynamodb]]
			- Partition Key: user_id
			- Sort Key: execution_time
	- How can we ensure the system executes jobs within 2s of their scheduled time?
		- + Redis TTL is "creative" and demonstrates some understanding of distributed systems.
		- + Shows consideration for system reliability by talking about redundancy and error handling.
		- - Using multiple Redis can introduce unnecessary complexity because of data inconsistency. Consider using a "delay queue" like AWS SQS which handles precise timing and retries.
		- - Lacks clear strategy for handling urgent jobs and prioritization. Need a way to directly execute or direct insure to queue.
		- Use a "two-phase" architecture using a database for persistent storage and a delay queue (like AWS SQS) for precise execution timing. This allows periodic DB scans to load upcoming jobs and direct queue insertion for urgent tasks.
		- SQS has built-int delivery delays and retries. This will decouple job scheduling from execution and achieve better scalability and maintainability.
	- How can we scale job execution to support up to 10,000 concurrent jobs executing in parallel?
		- - Just saying API gateway and load balancing to horizontally scaled servers is not enough
		- Talk about auto-scaling containers using orchestration platforms like Kubernetes or ECS.
		- Also talk about potential solutions with serverless functions.
		- Discuss load balancing strategy for distributing jobs acorss task services to prevent bottlenecks.
	- How should we handle retrying jobs that fail during execution?
		- - Current solution does not effectively handle worker crashes or application errors.
		- Look into SQS's visibility timeout which can handle both visible and invisible failures.
			- SQS's visibility timeout temporarily hides a message from other consumers while a worker processes it, ensuring that if the worker fails, the message becomes visible again for reprocessing. This prevents job loss and ensures reliability. Communicating this demonstrates an understanding of fault tolerance and resilience.
		- - Current design lacks a specific retry strategy or backoff mechanism. Implementing an exponential backoff strategy would help preventing overwhelming the system with immediate retries.
		- Explain how you would update the offset in the eventbus and the process for inserting failed jobs. Clarify the offset mechanism for retrying.


### Deep Dives
1. How can we ensure the system executes jobs within 2s of their scheduled time?
	1. user creates a job which is written to the DB
	2. cron job runs every 5 mins and puts the jobs that need to be executed in the next 5 mins to SQS with delay
	3. workers get job from SQS when they're ready
	4. <5 min jobs are sent directly to SQS with appropriate delay
2. How can we ensure the system is scalable to support up to 10k jobs per second?
	1. **Job Creation**
		1. Most likely have same recurring jobs and job creation itself has relatively low QPS. If it was high, we can introduce MQ between the API and Job Creation service that acts as a buffer during traffic spikes and lets us horizontally scale creation and consumption.
	2. **Jobs DB**: DynamoDB or Cassandra because they can handle thousands of writes per second per partition.
		1. Jobs table: partitioned by job_id to distribute load across tables
		2. Execution table: partitioned by execution_time to enable efficient querying of upcoming jobs
		3. These tables should be able to handle 10K easily with proper provisioning
	3. **Message Queue Capacity**
		1. 3 million jobs / 5 mins = (10k jobs/second * 300 seconds)
		2. SQS handles scaling and message distribution across consumers.
	4. **Workers**
		1. 2 main choices: containers and lambda functions
			1. Containers: more cost-effective for steady and long-running workloads. has more operational overhead.
			2. Lambda functions: serverless with minimal operational overhead. perfect for <15 min jobs and auto-scales. Cold-start is relatively slow and expensive under constant load.
3. How can we ensure at-least-once execution of jobs?
	1. A job can fail within a worker for one of two reasons:
		1. **Visible failure**: The job fails visibly probably due to a bug in the task code or incorrect input parameters.
		2. **Invisible failure**: The job fails invisibly, which most likely means the worker went down.
	2. Visible failure is easy. We wrap the task code in try/catch and get logs. We retry with exponential backoff and write to the Executions table and update the job status to RETRYING with the number of attempts made so far. SQS handles exponential backoff out of the box.
	3. For invisible failures, we leverage SQS. It has a built-in mechanism for handling worker failure through visibility timeouts. If the worker takes the job, SQS makes it invisible for other workers until the timeout. If processed, the message is deleted from SQS.
		1. SQS will have a 30 second timeout. If the worker doesn't process it in the timeout period, it makes the job visible again.
		2. To handle longer than 30 second jobs, workers will call SQS's ChangeMessageVisibility API to extend the timeout every 15 seconds. If the worker crashes, it won't extend the timeout so SQS will know to make it visible, and if it's a long running job, worker will keep extending the timeout period to keep it invisible in SQS.
4.  How do we ensure our task code is idempotent (can execute multiple times without affect of executing multiple times)?
	1. This is important for our "at-least-once" execution. Without it, we'd be executing jobs like sending email multiple times.
	2. We could have a deduplication table that checks if the job execution is processed already. If the job id is in the table, skip the execution. But this needs another table and the table needs to be cleaned up regularly. There's also a race condition window between checking and writing to the deduplication table.
	3. Best way is to design the jobs themselves to be idempotent using idempotency keys and conditional operations. "set counter to X" is better than "increment counter" and instead of "send email" check if the email flag is already set. So *instead of deciding whether to execute the job or not, make it so that it doesn't matter even if it executes multiple times.*

practice: ![[whiteboard-before.png]]
better: ![[whiteboard-after.png]]
