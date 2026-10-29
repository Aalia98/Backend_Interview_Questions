## Golang Fundamentals

Variables/types - done
Struct - done
Pointer - done
Slice - done
Map - done
Interface - done
Methods - done
Generics - done
Error handling - 
defer - 
panic - 
recover - 

## Go Concurrency

Goroutine
Channel
Buffered/unbuffered
Mutex
RWMutex
WaitGroup
Select
Context
Race condition
Deadlock
Worker pool
Goroutine leak

Go Concurrency ka complete roadmap
LEVEL 1 — FUNDAMENTALS
1. Concurrency kya hai? Concurrency vs Parallelism
2. Goroutines — go keyword
3. Go scheduler ka basic idea
4. sync.WaitGroup
   
   LEVEL 2 — COMMUNICATION
5. Channels — unbuffered aur buffered
6. Channel send, receive aur close
7. select statement
8. Nil channels aur deadlocks
   
   LEVEL 3 — SYNCHRONIZATION
9. sync.Mutex aur sync.RWMutex
10. Race conditions aur go run -race
11. sync.Once, sync.Cond aur sync.Map
12. sync/atomic
    
    LEVEL 4 — ADVANCED & PRACTICAL
13. Context, cancellation aur timeout
14. Worker pools aur fan-in/fan-out
15. Goroutine leaks aur graceful shutdown
16. Practical projects aur concurrency interview questions

## HTTP + REST API

HTTP request/response
GET/POST/PUT/PATCH/DELETE
Status codes
Headers
JSON
REST
Idempotency
Validation
Error handling
Middleware

## Authentication

Authentication vs authorization
JWT
Access token
Refresh token
JWT structure
JWT flow
Middleware
RBAC

## SQL

SELECT
WHERE
GROUP BY
HAVING
JOIN
Subqueries
CTE basics
Window functions basics
INSERT/UPDATE/DELETE
Aggregate functions

## MySQL

Primary key - done
Foreign key
Unique key - done
Index
Composite index
Clustered index concept
Query optimization
EXPLAIN
Transactions
ACID
Isolation levels
Deadlocks
Normalization

## Redis Fundamentals

What Redis is
Key-value
Strings
Lists
Sets
Sorted sets
Hashes
TTL
Expiration

## Redis Caching

Cache-aside
Write-through
Write-back
Cache invalidation
Cache miss
Cache hit
TTL
Hot keys
Cache stampede

## Kafka Fundamentals

Understand:

    Producer
    ↓
    Topic
    ↓
    Partition
    ↓
    Consumer
    ↓
    Consumer Group

Then:

Offset
Ordering
Replication
Consumer group
Partitioning
Rebalancing
Delivery semantics

## Kafka Deep Dive

Why Kafka?
Kafka vs Redis?
Kafka vs RabbitMQ?
What happens if consumer crashes?
What happens if consumer processes same event twice?
How do you handle duplicate messages?
How does partitioning work?
How does ordering work?
What is consumer lag?
At-most-once vs at-least-once?
Exactly-once basics?

## Backend Architecture

Study:

    Handler
    ↓
    Service
    ↓
    Repository
    ↓
    Database

Then:

Dependency injection
Repository pattern
MVC
Clean architecture basics
Error handling
Validation
Logging
Configuration
Environment variables

## Microservices + Distributed Systems

Monolith vs microservices
Service communication
Sync vs async
Load balancing
Service discovery basics
Fault tolerance
Retry
Timeout
Circuit breaker
Event-driven architecture
CAP theorem
Replication
Sharding basics

WEEK 3 — PROJECT + EXPERIENCE + SYSTEM DESIGN

This week is extremely important.

Because your CV has some strong claims.

For example:

20+ real-time slot games
10K+ concurrent users
40% API latency improvement
69% database-load reduction
90% stability improvement
DB CPU from 80% → 5%

These are excellent resume points.

But they also invite deep questioning.

So we need to make sure you can defend them.

Day 15 — RNGplay Experience

We reconstruct your actual work.

For every bullet:

What was the problem?
        ↓
Why was it happening?
        ↓
How did you investigate?
        ↓
What solution did YOU implement?
        ↓
Why that solution?
        ↓
What changed?
        ↓
How did you measure it?

This is much more important than memorizing theoretical answers.

Day 16 — Memory Leak + Concurrency Story

Your CV says you fixed production memory leaks and concurrency issues.

We need to prepare:

What was the symptom?
How did you identify the leak?
What tools did you use?
Was it goroutine leak?
Was memory retained by data structures?
What caused it?
What did you change?
How did you verify the fix?

Only based on what actually happened.

We won't invent an impressive story.

Day 17 — Redis + API Optimization Story

Prepare:

Before
 ↓
Problem
 ↓
Investigation
 ↓
Redis solution
 ↓
After

Especially:

"How exactly did Redis reduce DB load by 69%?"

Day 18 — Large File Upload Story

Your CV says chunked processing reduced database CPU utilization from 80% to 5%.

This is potentially a very strong interview discussion.

We'll prepare:

Why was DB CPU high?
What was the original implementation?
What does chunking mean?
How did chunking reduce DB load?
How were chunks processed?
What happens if one chunk fails?
How did you verify 80% → 5%?
Day 19 — E-commerce Project Architecture

We will build/understand the project properly:

                    Client
                       ↓
                    Router
                       ↓
                   Middleware
                       ↓
                    Handler
                       ↓
                    Service
                  ↙    ↓     ↘
             MySQL    Redis   Kafka

Features:

Register
Login
JWT
RBAC
Product
Category
Cart
Order
Redis
Kafka
Day 20 — E-commerce Deep Dive

Questions like:

Why MySQL?

Why Redis?

Why Kafka?

Why JWT?

Why Repository Pattern?

How does order creation work?

What if Redis is down?

What if Kafka is down?

What if MySQL is down?

What if two users purchase the last item simultaneously?

How do you prevent duplicate orders?

How do you handle transaction failure?

This is where you'll start feeling like a real backend engineer rather than someone memorizing interview answers.

Day 21 — System Design Mock

We'll design something like:

E-commerce order system
Client
 ↓
Load Balancer
 ↓
API
 ↓
Order Service
 ↓
MySQL
 ↓
Kafka
 ↓
Notification Service

Then discuss:

Scaling
Caching
DB
Kafka
Failure
Consistency
Availability
🗓️ WEEK 4 — Interview Mode

Now we stop behaving like students.

We behave like job candidates.

Day 22 — DSA Intensive

You have 300+ LeetCode problems listed as an achievement.

So don't spend this month learning 100 new problems.

Focus on patterns:

Arrays
Strings
HashMap
Two pointers
Sliding window
Binary search
Stack
Queue
Linked list
Trees
Heap

Target:

2 problems/day, but focus on explaining the solution.

Day 23 — DSA + Go Coding

Solve problems without looking at the solution.

Then explain:

Brute force
↓
Optimization
↓
Time complexity
↓
Space complexity
↓
Go implementation
Day 24 — System Design

Study:

Requirements
APIs
DB
Cache
Message queue
Scaling
Load balancer
Replication
Sharding
Bottlenecks
Failure handling
Day 25 — React / TypeScript / JavaScript

Important:

Do NOT spend 5 days on React.

Your target is backend.

Your CV lists React/TypeScript and you have professional experience with dashboards and internal tools.

So prepare enough to defend:

React
Components
Props
State
Hooks
useEffect
useState
API calls
Rendering
Basic optimization
JavaScript
let/const
closures
promises
async/await
event loop basics
map/filter/reduce
TypeScript
interface
type
generics basics
optional properties
Day 26 — Docker + Linux + Git
Docker

Know:

Image
Container
Dockerfile
Volume
Network
Docker Compose
Linux

Know basic:

ps
top
grep
curl
chmod
kill
netstat/ss
tail
Git

Know:

clone
branch
checkout/switch
add
commit
push
pull
merge
rebase basics
stash
reset basics
Day 27 — Full CV Interview

I will behave like:

Round 1

Resume

Round 2

Golang

Round 3

Backend

Round 4

SQL

Round 5

Redis/Kafka

Round 6

Project

Round 7

DSA

Day 28 — Fix Weaknesses

This day depends completely on your mock interview.

Example:

If you fail:

Kafka
SQL indexes
Go interfaces

then Day 28 becomes:

Kafka → 2 hours
SQL → 2 hours
Go → 2 hours

Not a predetermined syllabus.

Day 29 — Final Mock

Real interview simulation.

No notes.

No ChatGPT help.

No Google.

You answer naturally.

I'll cross-question you.

Day 30 — Interview Readiness

We revise:

Your introduction
Experience
Project
Technical stack
DSA patterns
System design
HR

And most importantly:

We identify your remaining weak areas.

1. Your numbers

You have many impressive metrics:

10K+ concurrent users
40% latency
69% DB load
90% stability
80% → 5% DB CPU
60% read latency
25% query performance

These are great only if you can explain how they were achieved/measured.

We'll prepare each one.

2. "Distributed Systems"

You have it right in your headline and technical skills.

So an interviewer may ask:

"What distributed systems have you worked on?"

You need a clear answer connecting:

Go + Kafka + Redis + MySQL + real-time applications.

3. "Microservices"

Your CV lists microservices.

So you need to know:

What is a microservice?
Why use it?
Monolith vs microservices
Service communication
Database per service
Distributed transactions
Failure handling
Service discovery basics