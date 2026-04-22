# The Relearning
*A senior developer's blog and learning milestones*

I'm a senior engineer who has spent years building by doing — shipping with a working-level understanding of the patterns underneath. I've watched peers who took the time to truly master those patterns break through a ceiling I haven't crossed yet. So this is me slowing down. This blog is where I relearn and readapt the foundations — not to catch up, but to finally stand where they stand, and break my own ceiling.

---

## Changelog

### 2025-10-19
Set up the site templating following Drew DeVault's approach, and published the first article documenting progress. The pipeline from idea → post is now real.

### 2025-08-31
Worked through a new tutorial to sharpen my fundamentals and identify the next things worth learning.

### 2025-08-29
Polished the codebase, using a proper tutorial as the reference point. Cleaner structure, clearer intent.

### 2025-08-26
Pipeline now automatically opens a PR after a successful development build. Small win, but a satisfying one.

### 2025-08-25
First page deployed successfully. The thing exists on the internet.

---

## What I'm covering

These are the areas I'm actively deepening my understanding in:

1. **OOP** — the foundation everything else sits on
2. **SOLID principles** — practical understanding and real application in C++, JavaScript, and PHP
3. **PHP and Laravel** — legitimately good tools, and I'll defend that choice
4. **Azure Functions vs App Service** — knowing which to reach for and why
5. **Serverless architectures**
6. **Docker** and containerization
7. **SQL and NoSQL** — and when each is the right call
8. **Scaling** — patterns, tradeoffs, and what actually matters in practice

---

## Scalability Reference Notes

Baseline numbers I keep around for system design discussions. *(Estimates surfaced with help from Claude — useful as orders of magnitude, not absolutes.)*

### Web Applications
- Single server: ~1,000–5,000 concurrent users
- Load-balanced setup: ~10,000–50,000 concurrent users
- Well-architected system: 100,000+ concurrent users

### Databases
- Single MySQL / PostgreSQL: ~10,000 reads/sec, ~1,000 writes/sec
- Properly indexed and optimized: ~50,000 reads/sec, ~5,000 writes/sec

### Request Handling
- Simple API endpoint: ~1,000–10,000 req/sec per server
- Complex business logic: ~100–1,000 req/sec per server

### What Actually Matters in a System Design Conversation
- Scalability isn't just "bigger servers" — it's architecture
- Know the usual suspects for bottlenecks: database, memory, I/O
- Be fluent in the patterns: caching, load balancing, microservices
- "It depends" is a real answer — the tradeoffs are the whole point