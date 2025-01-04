Functional
1. Users should be able to view a given problem and code a solution in multiple languages
2. Users should be able to submit their solution and get instant feedback
3. Users should be able to compete in competitions and view a live leaderboard to see who's winning
4. 300k dau, max 100k/competition

Non-functional
- availability >> consistency
- low latency for submission (<5s)
- security and isolation for code execution

Entities
- User
- Problem (id, description, code stubs, test cases, metadata)
- Submission (id, submittedAt, test case results, runtime, error)
- Competition

APIs
- GET /problems?page=1&limit=100 -> Partial\<Problem>\[]
- GET /problems/:id -> Problem
- POST /problems/:id -> Submission
  body: {
    code: string,
    language: string
  }
- GET /leaderboard/:competitionId?page=1&limit=100 -> Leaderboard
- GET /check:id (this is for checking code submission status like running, error, success, etc.)

High level design
- VM security
	- docker for isolation
	- cpu and memory bounds
	- explicit timeout per execution
	- read only filesystem (writes to tmp)
	- network isolation
	- no system calls (seccomp)
- leaderboard
	- use Redis Sorted Sets

![[distributed-systems/system-design/code-judge/design.png]]

