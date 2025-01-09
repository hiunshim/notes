stream processor

Has a feature called "checkpointing" where the processor periodically writes its state to a persistent storage like S3. If it goes down, it will read the last checkpoint and resume processing from there.
- The shorter the window, the less we need checkpointing since we would lose only a very small amount of data, and stream is already persistent to store the events.