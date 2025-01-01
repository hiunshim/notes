**LLM** optimization
- Retrieval Augmented Generation
- Chain of thought*
- Few-Shot prompting
- Vector search
- Memory
- Parallelization
- Orchestration workers
- Evaluator

**Chaining**
Chain the responses if the requests depends on each other.

Bad example
```python
result = api_call("do task 1 and 2")
```
Better example
```python
result1 = api_call("do task 1")
result2 = api_call(result1, "do task 2")
```

**Routing**
Just like the assignment, route the user request to an agent based on intent.

Bad example
```python
api_call("respond", "what happened to the order?")
```
Better example
```python
api_call("find intent", "what happened to the order?")
intent.agent.api_call("respond", ...)
```

**Parallelization**
Bad example
```python
api_call("write two different essays")
```
Better example
```python
essay1 = api_call("write essay type 1")
essay2 = api_call("write essay type 2")
api_call("combine two essays", essay1, essay2)
```

**Orchestration workers**
Used when request and output are ambiguous.
Manager agent organizes and creates tasks. Each worker agent executes the tasks.

**Evaluator**
Similar to orchestration but used when the output is deterministic. Keep providing feedback to improve until threshold met.