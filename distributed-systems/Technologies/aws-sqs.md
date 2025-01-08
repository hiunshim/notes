SQS guarantees at least once delivery of messages.
Workers respond back to the queue with heart beat messages to indicate that they're still processing the job.

Supports retries with configurable exponential backoff out of the box. Visibility timeouts make it easy to handle message consumption when consumers go down.