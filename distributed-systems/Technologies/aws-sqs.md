SQS guarantees at least once delivery of messages.
Workers respond back to the queue with heart beat messages to indicate that theya re still processing the job.