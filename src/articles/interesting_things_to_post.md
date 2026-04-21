# FW Banyan — Study & Blog Notes
> Owner: Ahmad Faizal | Updated: April 2026
> Purpose: Personal learning notes, blog content source, interview prep reference

---

# Stateful versus stateless methods

In software development projects, the term state is used to describe the condition of the execution environment at a specific moment in time. As your code executes line by line, values are stored in variables. At any moment during execution, the current state of the application is the collection of all values stored in memory.

Some methods don't rely on the current state of the application to work properly. In other words, stateless methods are implemented so that they can work without referencing or changing any values already stored in memory. Stateless methods are also known as static methods.

For example, the Console.WriteLine() method doesn't rely on any values stored in memory. It performs its function and finishes without impacting the state of the application in any way.

Other methods, however, must have access to the state of the application to work properly. In other words, stateful methods are built in such a way that they rely on values stored in memory by previous lines of code that have already been executed. Or they modify the state of the application by updating values or storing new values in memory. They're also known as instance methods.

Stateful (instance) methods keep track of their state in fields, which are variables defined on the class. Each new instance of the class gets its own copy of those fields in which to store state.

A single class can support both stateful and stateless methods. However, when you need to call stateful methods, you must first create an instance of the class so that the method can access state.

#### -- improvements to be senior --
The current understanding is correct — but it stops at *what* stateful/stateless is. A senior thinks about *consequences and design decisions*:

- **Thread safety:** Stateless methods are thread-safe by default. Stateful methods are not — if two threads call a stateful method on the same instance simultaneously, they can corrupt each other's data. This is why Singleton services in .NET DI must be designed carefully.
- **Testability:** Stateless methods are trivial to unit test — no setup, no mocking. Stateful methods require you to set up state before asserting. Prefer stateless where possible for this reason alone.
- **DI lifetime implications:** In .NET, a `Singleton` service is stateful (one shared instance). A `Transient` service is effectively stateless (new instance per use). Choosing the wrong lifetime is a common senior interview trap — a `Scoped` dependency injected into a `Singleton` is called a "captive dependency" and causes bugs.
- **Interview reframe:** Instead of "static = stateless", say: *"I push as much logic as possible into stateless methods and isolate mutable state to the edges — the database, the cache, the DI container. It makes the core logic predictable and testable."*

---

# Random for strings

```csharp
Random rnd = new();
string[] malePetNames = [ "Rufus", "Bear", "Dakota", "Fido",
                        "Vanya", "Samuel", "Koani", "Volodya",
                        "Prince", "Yiska" ];
string[] femalePetNames = [ "Maggie", "Penny", "Saya", "Princess",
                          "Abby", "Laila", "Sadie", "Olivia",
                          "Starlight", "Talla" ];

int mIndex = rnd.Next(malePetNames.Length);
int fIndex = rnd.Next(femalePetNames.Length);

Console.WriteLine("Suggested pet name of the day: ");
Console.WriteLine($"   For a male:     {malePetNames[mIndex]}");
Console.WriteLine($"   For a female:   {femalePetNames[fIndex]}");
```

#### -- improvements to be senior --
This example teaches `Random` — but a senior knows `Random` has limits that matter in production:

- **`Random` is NOT cryptographically secure.** Never use it for tokens, passwords, session IDs, or anything security-related. For those, use `System.Security.Cryptography.RandomNumberGenerator`.
- **`Random` is not thread-safe.** If two threads call `rnd.Next()` on the same instance simultaneously, the internal state can corrupt and start returning 0 for everything. In multi-threaded code, use `Random.Shared` (.NET 6+) which is thread-safe, or lock the instance.
- **Seed awareness:** `new Random()` uses the system clock as a seed. Two instances created in rapid succession (e.g. in a tight loop) may produce identical sequences. In older .NET, this was a real gotcha.

```csharp
// Thread-safe (.NET 6+)
int index = Random.Shared.Next(array.Length);

// Cryptographically secure (for tokens, not names)
byte[] token = RandomNumberGenerator.GetBytes(32);
```

- **Banyan relevance:** Session token generation in Banyan.Commerce must use `RandomNumberGenerator`. Using `Random` for security tokens is a code review failure.

---

# New things to pick up

possesses the specific technical skills of an experienced BI and data warehouse senior developer, including technical skills in the following:
- (i) SQL Relational Database
- (ii) Data warehouse design
- (iii) Dimensional modelling
- (iv) Extraction, transformation and loading (ETL) using Microsoft SQL Server Integration Services (SSIS)
- (v) Solution Design
- (vi) Online Analytical Processing (OLAP) cube development using Microsoft SQL Server Analysis Services (SSAS) and Multidimensional Expressions (MDX)
- (vii) Report development using Microsoft SQL Server Reporting Services (SSRS)
- (viii) Microsoft Data Visualization tools such as Power Pivot and Power BI
- (ix) Advanced skill level and advanced knowledge in SQL Language (Query) and Stored Procedure
- (x) Big Data skills and knowledge (added advantage)

#### -- improvements to be senior --
This is a JD skills dump — which is fine as a capture. A senior turns a skills list into a **gap analysis with priority**:

| Skill | Your Status | Priority |
|---|---|---|
| SQL / Stored Procedures | ✅ Strong — Petronas, Ace Resources | Formalise window functions |
| Power BI | ✅ Strong — Petronas | Document and quantify impact |
| Data warehouse design | ⚠️ Partial | Learn star vs snowflake schema |
| Dimensional modelling | ❌ Gap | Fact tables, dimension tables, SCDs |
| ETL via SSIS | ❌ Gap | You built ETL in Node.js — learn SSIS tooling |
| SSAS / OLAP / MDX | ❌ Gap | Low priority unless targeting BI roles |
| SSRS | ❌ Gap | Transferable from Power BI knowledge |
| Big Data | ⚠️ Partial | Azure Data Lake — expand if needed |

- **Senior move:** Don't study everything. Decide if this role cluster is a target. If yes, prioritise SSIS + dimensional modelling first — those are the hardest to learn and most frequently screened. If no, deprioritise entirely and don't spread thin.
- **For current Sephora SEA targets:** This cluster is not relevant. Park it.

---

# For loop VS Foreach loop

Take a minute to consider the choice between a for statement and a foreach statement when iterating through the ourAnimals array.

The goal is to iterate through each animal in the ourAnimals array one at a time. So why not use a foreach loop? After all, you know that the foreach statement is designed for cases when you want to iterate through each item in an array of items.

The reason why you don't use a foreach loop in this situation is because the ourAnimals array is multidimensional array. Since ourAnimals is a multidimensional string array, each element contained within ourAnimals is a separate item of type string. If you used a foreach loop to iterate through ourAnimals, the foreach would recognize each string as a separate item in a list of 48 string items (8 x 6 = 48). The foreach statement wouldn't process the two array dimensions separately. In other words, a foreach loop won't recognize 8 rows of string elements, where each row contains a column of 6 items. Since you want to work with one animal at a time, and process all six animal characteristics during a single iteration, a foreach statement isn't the right choice.

However, if the ourAnimals array was a jagged array configured as an array of string arrays, you could use a foreach statement. In this case, you would create a foreach for an outer loop and second foreach for an inner loop. The outer loop would iterate through the "string array" elements in the jagged array. The string arrays are the "rows" in the two-dimensional array. The inner loop would iterate through the "string" elements contained in the string arrays. The string elements in the string arrays are the "columns" in the two-dimensional array.

#### -- improvements to be senior --
The current note explains *why* for beats foreach for multi-dimensional arrays — good. A senior also carries a complete **decision table**:

| Scenario | Use | Reason |
|---|---|---|
| Simple list, no index needed | `foreach` | Intent is clearest |
| Multi-dimensional array | `for` | `foreach` flattens all elements |
| Need the index during iteration | `for` | `foreach` doesn't expose index natively |
| Modifying the collection while iterating | Neither — collect changes first | Throws `InvalidOperationException` |
| Performance-critical inner loop | `for` | Marginal but measurable at scale |
| LINQ alternative | `.Select()` / `.Where()` | Often cleaner than either loop for transformations |

- **The senior reflex:** Before writing any loop, ask: *"Do I need the index? Am I modifying the collection? Is this multi-dimensional?"* If all no — write `foreach`. It signals intent clearly.
- **Interview trap:** "Can you modify a list inside a foreach?" — The answer is no, it throws. The senior answer adds: *"If I need to remove items, I iterate backwards with for, or I build a removal list and clear after."*

---

# Why use 'New'

Reference types include arrays, classes, and strings. Reference types are treated differently from value types regarding the way values are stored when the application is executing.

A value type variable stores its values directly in an area of storage called the stack. The stack is memory allocated to the code that is currently running on the CPU (also known as the stack frame, or activation frame). When the stack frame has finished executing, the values in the stack are removed.

A reference type variable stores its values in a separate memory region called the heap. The heap is a memory area that is shared across many applications running on the operating system at the same time. The .NET Runtime communicates with the operating system to determine what memory addresses are available, and requests an address where it can store the value. The .NET Runtime stores the value, and then returns the memory address to the variable. When your code uses the variable, the .NET Runtime seamlessly looks up the address stored in the variable, and retrieves the value that's stored there.

```csharp
// Value type — copy on assign
int val_A = 2;
int val_B = val_A;
val_B = 5;
// val_A is still 2

// Reference type — same memory address
int[] ref_A = new int[1];
ref_A[0] = 2;
int[] ref_B = ref_A;
ref_B[0] = 5;
// ref_A[0] is now 5 — both point to same heap location
```

#### -- improvements to be senior --
The current note explains stack vs heap correctly. A senior extends this into **real consequences**:

- **Passing to methods:** Value types are *copied* into the method. Changing them inside doesn't affect the caller. Reference types pass the *address* — changes inside the method affect the original object. This is why `ref` and `out` keywords exist — to pass a value type by reference explicitly.
- **Garbage collection:** When no variable holds a reference to a heap object, the GC reclaims it. This is why long-lived objects (stored in Singletons) can cause memory leaks if they hold references to short-lived objects — the GC can't collect them.
- **`IDisposable` + `using`:** For heap objects that hold unmanaged resources (file handles, DB connections, HTTP clients), the GC alone isn't enough. Always use `using` to ensure deterministic cleanup:
```csharp
using var connection = new SqlConnection(connectionString);
// connection.Dispose() is called automatically at end of block
```
- **Banyan relevance:** Every `new DbContext()`, every service registered in DI — these are heap allocations. DI lifetime (Singleton, Scoped, Transient) controls when they're created and when the GC can collect them. This is a senior-level interview topic.
- **Interview reframe:** Don't just say "`new` creates a heap object." Say: *"`new` allocates memory on the heap and returns a reference. The lifetime of that object is managed by the GC — or by `IDisposable` for unmanaged resources. Choosing the right lifetime is why DI container configuration matters."*

---

# Narrowing or widening conversion

The term narrowing conversion means that you're attempting to convert a value from a data type that can hold more information to a data type that can hold less information. In this case, you may lose information such as precision (that is, the number of values after the decimal point). An example is converting value stored in a variable of type decimal into a variable of type int. If you print the two values, you would possibly notice the loss of information.

```csharp
int first = 5; first.ToString()          // int to string
string second = "6"; int.Parse(second)   // string to int

int[] arrayTest = new int[3];            // reference type needs 'new'

if (int.TryParse(value, out result))     // value: input, result: output — safe parse
Array.Clear(pallets, 0, 2);              // clears from index 0 and 1
```

#### -- improvements to be senior --
The current note captures the mechanics. A senior thinks about **failure modes and defensive coding**:

- **Never use `Parse()` on user input.** It throws `FormatException` on invalid input and crashes the app. Always use `TryParse` when the source is user-supplied or external:
```csharp
// Dangerous — throws if input is "abc" or empty
int value = int.Parse(userInput);

// Safe — handles invalid input without exception
if (int.TryParse(userInput, out int value)) {
    // use value
} else {
    // handle bad input gracefully
}
```
- **`Convert.ToInt32` vs `int.Parse`:** `Convert` handles `null` (returns 0). `Parse` throws on null. Know which to use based on whether null is a valid input.
- **Narrowing is a data integrity risk:** Casting `decimal` to `int` truncates, not rounds. `(int)9.99` gives `9`, not `10`. If you need rounding, use `Math.Round()` before casting.
- **`Array.Clear` nuance:** It doesn't remove elements — it resets them to their default value (0 for int, null for reference types). The array length stays the same. Don't confuse with "removing from memory."
- **Interview reframe:** *"When I see a Parse, I ask: what's the source? If it's user input or an external API, I always use TryParse and handle the failure path explicitly. Unhandled format exceptions in production are a reliability issue."*

---

# String interpolation and composite formatting

```csharp
string first = "one";
string second = "two";

// Composite formatting
Console.WriteLine("{0}, {1}", first, second);   // "one, two"

// String interpolation
Console.WriteLine($"{first} {second}");          // "one two"

// Format specifiers
:N    // numerical with comma separator
:P    // percentage
.PadLeft(10)         // right-align in 10 characters
.PadLeft(10, '-')    // pad with dashes instead of spaces
```

#### -- improvements to be senior --
The current note is a syntax reference — useful. A senior adds **performance awareness and context**:

- **String interpolation allocates on the heap.** Every `$"..."` creates a new string object. In a tight loop running millions of times, this adds up:
```csharp
// Bad in hot path — creates thousands of heap objects
for (int i = 0; i < 100000; i++) {
    string s = $"Item {i}";  // heap allocation every iteration
}

// Better — use StringBuilder for repeated concatenation
var sb = new StringBuilder();
for (int i = 0; i < 100000; i++) {
    sb.Append("Item ").Append(i).AppendLine();
}
string result = sb.ToString();  // one allocation at the end
```
- **Interpolated strings vs composite:** Interpolation (`$""`) is almost always preferred for readability. Composite (`{0}, {1}`) is useful when the format string is stored separately or localised — you can swap in different format strings at runtime.
- **Common format specifiers to know cold:**
```csharp
$"{1234.567:N2}"    // 1,234.57  — number, 2 decimal places
$"{0.75:P1}"        // 75.0%     — percentage, 1 decimal
$"{42:D5}"          // 00042     — integer padded to 5 digits
$"{DateTime.Now:yyyy-MM-dd}"  // 2026-03-31 — date format
```
- **Interview reframe:** *"I prefer interpolation for clarity. In performance-sensitive code — logging in a hot path, building large strings in a loop — I switch to StringBuilder or `string.Create()` to avoid repeated heap allocations."*

---

# IndexOf() and Substring() helper methods

```csharp
string message = "Find what is (inside the parentheses)";

int openingPosition = message.IndexOf('(');   // 13
int closingPosition = message.IndexOf(')');   // 36

int length = closingPosition - openingPosition;
Console.WriteLine(message.Substring(openingPosition, length));
// "(inside the parentheses"

// With arrays of characters
string message = "Help (find) the {opening symbols}";
char[] openSymbols = { '[', '{', '(' };
int openingPosition = message.IndexOfAny(openSymbols);
int openingPosition2 = message.IndexOfAny(openSymbols, startPosition: 5);
```

#### -- improvements to be senior --
The current note covers the basics well. A senior extends into **patterns and gotchas**:

- **Strings are immutable.** Every `Replace`, `ToLower`, `Substring` returns a *new* string — the original is unchanged. This surprises juniors who expect in-place modification:
```csharp
string s = "Hello";
s.ToLower();            // does nothing useful — result discarded
s = s.ToLower();        // correct — reassign
```
- **`IndexOf` returns -1 when not found — always check:**
```csharp
int pos = message.IndexOf('(');
if (pos == -1) {
    // handle: character not found
    // skipping this check and passing -1 to Substring throws ArgumentOutOfRangeException
}
```
- **Modern alternative — Range syntax (.NET 6+):**
```csharp
// Old way
string extracted = message.Substring(openingPos, closingPos - openingPos);

// Modern way — cleaner
string extracted = message[openingPos..closingPos];
```
- **When to use `StringBuilder` instead:** If you're doing many `Replace` or concatenation operations in a loop, each call creates a new string. Use `StringBuilder.Replace()` for bulk manipulation.
- **Other useful string methods to know:**
```csharp
message.Contains("inside")     // bool
message.StartsWith("Find")     // bool
message.Split(',')              // string[] — split on delimiter
message.Trim()                  // remove whitespace
message.Replace("(", "[")       // returns new string
```
- **Interview reframe:** *"Before I reach for Substring, I check if the delimiter actually exists with IndexOf and guard against -1. In production code, missing that check has caused ArgumentOutOfRangeException in edge cases that tests didn't catch."*

---

# Node.js Interview Debrief *(from interview — April 2026)*

### Event Loop
Node.js is single-threaded but handles concurrency through a non-blocking event loop. Delegates I/O to the OS, registers a callback, moves on. When the OS signals completion, the callback queues and runs when the call stack is empty.

**Phases:** Timers → I/O callbacks → Idle/Prepare → Poll → Check (setImmediate) → Close

*What I said:* setTimeout delays a response; setInterval runs repeatedly. **Correct but incomplete.**

#### -- improvements to be senior --
- The right answer goes one level deeper: *"Node.js is concurrent without being multi-threaded because it never blocks the thread waiting for I/O. It offloads I/O to the OS and handles the result via a callback when the call stack is free. setTimeout and setInterval are the Timer phase of the event loop — the first callbacks to run after each I/O poll cycle."*
- Know the phases cold — interviewers at senior level ask about `setImmediate` vs `setTimeout(fn, 0)`. They run in different phases (Check vs Timers) so execution order can be non-deterministic depending on context.

---

### app.get vs app.post

**What I actually know (from years of use):**
- GET sends data in the URL
- POST sends data in the request body
- GET is less secure because the values are visible in the URL, browser history, and server logs

**What I said in the interview that was inaccurate:**
- Said POST "controls who can access what" — this is wrong. POST does not restrict access. That is the job of auth middleware (JWT verification, session checks), not the HTTP verb.
- Mentioned headers but didn't specify — the correct point would have been: *"Authorization headers carry the token that the middleware validates, regardless of whether the request is GET or POST."*

**The full correct picture:**

| | GET | POST |
|---|---|---|
| Data location | URL query string | Request body |
| Visible in URL / logs | ✅ Yes | ❌ No |
| Cached by browser/CDN | ✅ Can be | ❌ Not by default |
| Idempotent | ✅ Yes — same request, same result | ❌ No |
| Use for | Reading data | Creating or modifying data |
| Security | Same as POST when using HTTPS | Same as GET when using HTTPS |

HTTPS encrypts both equally. The verb alone controls nothing about who can access what.

#### -- improvements to be senior --
- **What actually controls access:** Auth middleware — not the verb. A `GET /admin/users` route is just as restricted as `POST /admin/users` if both have the same JWT verification middleware in front of them. Saying "POST is more secure" conflates the verb with the auth layer — two completely separate concerns.
- **Why GET feels less secure:** Because developers sometimes put sensitive data in GET query strings (`/reset?token=abc123`), which then lands in browser history, server logs, and Referer headers. The fix is not to switch to POST — it's to not put sensitive data in URLs at all.
- **The header point you were reaching for:** The `Authorization: Bearer <token>` header is what carries identity on every request, GET or POST. Middleware validates it before the route handler runs. That's the access control layer — completely independent of the HTTP verb.
- **Interview reframe:** *"GET and POST differ in semantics and data location — GET is for reads, data in the URL, idempotent and cacheable. POST is for writes, data in the body, non-idempotent. Access control is a separate concern handled by auth middleware that validates the Authorization header — the verb itself doesn't restrict or grant access to anything."*

---

### Express-Validator
Input sanitisation and validation middleware. Prevents malformed or malicious data reaching business logic. First line of defence against injection.

#### -- improvements to be senior --
- Connect it to security: *"Express-validator is the enforcement point for the input validation layer of defence-in-depth. Without it, you're trusting the client — which you should never do."*
- Know the difference between **validation** (is this email format correct?) and **sanitisation** (strip HTML tags, normalise email to lowercase). Express-validator does both. Senior engineers know which they're applying and why.

---

### Cluster vs Single-Thread
Single thread = one CPU core used. Cluster forks one worker per CPU core, all sharing the same port. Master restarts crashed workers — resilience without full app downtime.

#### -- improvements to be senior --
- Know the trade-off: Cluster gives you multi-core utilisation but workers don't share memory. If you have in-memory state (session cache, rate limit counters), it must live in Redis — not in the process — or each worker will have its own divergent copy.
- For containerised deployments (Docker/Kubernetes), you typically let the orchestrator handle scaling (multiple pods) rather than using Cluster inside one pod. Know both patterns and when to use which.

---

### Multi-Device Token Handling (the one I was unsure on)
Each login creates a separate refresh token row in the DB with device metadata. First login can be the "primary session." Global logout = wipe all rows for that userId. Token reuse after rotation signals theft — invalidate the entire token family.

#### -- improvements to be senior --
- The complete senior answer adds **token versioning** as an alternative: store a `tokenVersion` integer on the user record. Embed it in the JWT. On every auth check, compare the token's version to the DB. Increment the version to instantly invalidate all tokens across all devices without querying a sessions table.
- Know the trade-off: Token versioning requires a DB hit on every request (defeating stateless JWT). Refresh token rows in DB are more scalable — only hit the DB on token refresh, not every request.

---

# Stack vs Heap Memory *(from interview prep — April 2026)*

A value type variable stores its values directly on the **stack** — memory allocated to the currently running function. When the function returns, the stack frame is popped and those values are gone.

A reference type variable stores a memory address. The actual object lives on the **heap** — a shared memory region. The .NET Runtime allocates heap memory, returns the address, and the GC reclaims it when no references remain.

| | Stack | Heap |
|---|---|---|
| What lives here | Value types, local variables, method frames | Reference type objects |
| Managed by | CPU automatically (LIFO) | .NET GC |
| Speed | Extremely fast | Slower |
| Lifetime | Dies when function returns | Until GC collects |

#### -- improvements to be senior --
- **Stack overflow:** Deep recursion without a base case fills the stack. The senior answer to "what causes a stack overflow" is not "too many variables" — it's *"unbounded recursion that keeps pushing frames without ever popping them."*
- **Heap fragmentation:** In long-running apps, repeated allocations and collections can fragment the heap. The .NET GC has a Large Object Heap (LOH) for objects >85KB that isn't compacted by default — this is a real production concern for apps handling large files or byte arrays.
- **Interview reframe:** *"Understanding stack vs heap shapes how I design method signatures. Value types passed to methods are copied — safe but potentially expensive for large structs. Reference types pass the address — efficient, but callers must know the method might mutate the object. That's why I'm explicit about whether a method should take ownership of data or just read it."*

---

# Signed vs Unsigned Integral Types *(from interview prep — April 2026)*

Signed types reserve one bit for the sign (positive/negative). Unsigned types use all bits for magnitude — double the upper range, but zero and above only.

| Type | Signed Range | Unsigned Range |
|---|---|---|
| 8-bit | -128 → 127 | 0 → 255 |
| 16-bit | -32,768 → 32,767 | 0 → 65,535 |
| 32-bit | -2.1B → 2.1B | 0 → 4.29B |
| 64-bit | -9.2×10¹⁸ → 9.2×10¹⁸ | 0 → 1.8×10¹⁹ |

Negatives stored via Two's complement: flip all bits, add 1.

Classic bug: `byte x = 255; x++;` → x is 0 (wraps around).

#### -- improvements to be senior --
- **When it actually matters in your stack:** In C# day-to-day, you rarely touch unsigned types. They appear in binary protocol parsing (payment systems, file formats, network packets) and interop with C libraries. If you're building Banyan.Commerce's payment integration, you may encounter byte streams where signed vs unsigned interpretation of a byte changes the value entirely.
- **SQL awareness:** `TINYINT UNSIGNED` (0–255) vs `TINYINT` (-128–127) matters when designing columns for status flags or small counters. Getting this wrong means either wasting range or needing a schema migration later.
- **Interview reframe:** *"In C#, I default to `int` unless there's a domain reason — like a column that physically can't be negative (a count, an ID), where `uint` communicates that intent. In binary parsing, I always check whether the spec defines a field as signed or unsigned before reading it — the same byte has a completely different value depending on interpretation."*

---

# Web Performance: McMaster-Carr Pattern *(from architecture session — April 2026)*

McMaster-Carr's website looks outdated but is one of the fastest on the internet. It runs on ASP.NET with SSR — the server sends fully-formed HTML. No waiting for JavaScript to build the page.

Key techniques: SSR, inline critical CSS, page-specific JS only, HTML prefetching, CDN (Akamai), service workers, fixed image dimensions.

For Banyan.Discovery (Next.js): Server Components for SSR, `next/link` for prefetching, `next/image` for fixed dimensions, `dynamic()` for code splitting, Redis for API cache, Cloudflare as CDN.

#### -- improvements to be senior --
- The senior insight is not *"SSR is fast"* — it's understanding *why* and the trade-off: SSR increases server load and latency for the first byte (TTFB) compared to serving a static HTML shell. The win is Time to First Contentful Paint and SEO — the browser and crawlers see real content immediately.
- **Core Web Vitals** — know these cold for the Sephora Discovery interview:
  - **LCP** (Largest Contentful Paint) — how fast the main content loads. McMaster-Carr achieves 174ms.
  - **CLS** (Cumulative Layout Shift) — how much the page jumps around as it loads. Fixed image dimensions prevent this.
  - **INP** (Interaction to Next Paint) — responsiveness to user input.
- **Interview reframe:** *"I modelled Banyan.Discovery's performance architecture on McMaster-Carr — SSR for immediate content and SEO, inline critical CSS to eliminate render-blocking, prefetching on link hover for perceived instant navigation, and CDN caching to reduce origin server load. The principle is: the browser should never wait for something it could have had earlier."*

---

# Blog Article Ideas

| Article | Source | Audience |
|---|---|---|
| "Stateful vs Stateless: The Decision That Shapes Your Architecture" | §1 | Mid → Senior |
| "Why `new` Matters: Stack, Heap, and What Your Code Does in Memory" | §5 | Junior → Mid |
| "Why McMaster-Carr's 15-Year-Old Tech Beats Your React App" | §Web Perf | All |
| "JWT Multi-Device Sessions: The Primary Token Pattern Explained" | §Node.js interview | Mid → Senior |
| "foreach vs for in C#: It's Not About Performance, It's About Intent" | §4 | Junior → Mid |
| "String Immutability in C#: The Hidden Allocation You're Not Thinking About" | §7 | Mid |
| "Code Hardening in Node.js: What Every Senior Should Ship With" | §Node.js interview | Mid → Senior |

---

*Last updated: 01 April 2026*
*Companion files: FW_BANYAN_MASTER_CHECKLIST.md, FW_BANYAN_SEPHORA_DISCOVERY_ADDENDUM.md, FW_BANYAN_SEPHORA_SEA_ADDENDUM.md*


---

# Seniori-fy a Beginners code
Been following this tutorial to better understand the fundamentals.

Question now is, this code, is a basic entry but how do we make it into a proper working code, with the bells and whistles.

Here is the code:
// #1 the ourAnimals array will store the following: 

string animalSpecies = "";
string animalID = "";
string animalAge = "";
string animalPhysicalDescription = "";
string animalPersonalityDescription = "";
string animalNickname = "";

// #2 variables that support data entry
int maxPets = 8;
string? readResult;
string menuSelection = "";
decimal decimalDonation = 0.00m;
string suggestedDonation = "";

// #3 array used to store runtime data, there is no persisted data
string[,] ourAnimals = new string[maxPets, 7];

// #4 create sample data ourAnimals array entries
for (int i = 0; i < maxPets; i++)
{
    switch (i)
    {
        case 0:
            animalSpecies = "dog";
            animalID = "d1";
            animalAge = "2";
            animalPhysicalDescription = "medium sized cream colored female golden retriever weighing about 45 pounds. housebroken.";
            animalPersonalityDescription = "loves to have her belly rubbed and likes to chase her tail. gives lots of kisses.";
            animalNickname = "lola";
            suggestedDonation = "85.00";
            break;

        case 1:
            animalSpecies = "dog";
            animalID = "d2";
            animalAge = "9";
            animalPhysicalDescription = "large reddish-brown male golden retriever weighing about 85 pounds. housebroken.";
            animalPersonalityDescription = "loves to have his ears rubbed when he greets you at the door, or at any time! loves to lean-in and give doggy hugs.";
            animalNickname = "gus";
            suggestedDonation = "85.00";
            break;

        case 2:
            animalSpecies = "cat";
            animalID = "c3";
            animalAge = "1";
            animalPhysicalDescription = "small white female weighing about 8 pounds. litter box trained.";
            animalPersonalityDescription = "friendly";
            animalNickname = "snow";
            suggestedDonation = "85.00";
            break;

        case 3:
            animalSpecies = "cat";
            animalID = "c4";
            animalAge = "3";
            animalPhysicalDescription = "Medium sized, long hair, yellow, female, about 10 pounds. Uses litter box.";
            animalPersonalityDescription = "A people loving cat that likes to sit on your lap.";
            animalNickname = "Lion";
            suggestedDonation = "85.00";
            break;

        default:
            animalSpecies = "";
            animalID = "";
            animalAge = "";
            animalPhysicalDescription = "";
            animalPersonalityDescription = "";
            animalNickname = "";
            suggestedDonation = "85.00";
            break;

    }

    ourAnimals[i, 0] = "ID #: " + animalID;
    ourAnimals[i, 1] = "Species: " + animalSpecies;
    ourAnimals[i, 2] = "Age: " + animalAge;
    ourAnimals[i, 3] = "Nickname: " + animalNickname;
    ourAnimals[i, 4] = "Physical description: " + animalPhysicalDescription;
    ourAnimals[i, 5] = "Personality: " + animalPersonalityDescription;
    if (!decimal.TryParse(suggestedDonation, out decimalDonation))
    {
        decimalDonation = 45.00m; // if suggestedDonation NOT a number, default to 45.00
    }
    ourAnimals[i, 6] = $"Suggested Donation: {decimalDonation:C2}";
}

// #5 display the top-level menu options
do
{
    // NOTE: the Console.Clear method is throwing an exception in debug sessions
    Console.Clear();

    Console.WriteLine("Welcome to the Contoso PetFriends app. Your main menu options are:");
    Console.WriteLine(" 1. List all of our current pet information");
    Console.WriteLine(" 2. Display all dogs with a specified characteristic");
    Console.WriteLine();
    Console.WriteLine("Enter your selection number (or type x to exit the program)");

    readResult = Console.ReadLine();
    if (readResult != null)
    {
        menuSelection = readResult.ToLower();
    }

    // use switch-case to process the selected menu option
    switch (menuSelection)
    {
        case "1":
            // list all pet info
            for (int i = 0; i < maxPets; i++)
            {
                if (ourAnimals[i, 0] != "ID #: ")
                {
                    Console.WriteLine();
                    for (int j = 0; j < 7; j++)
                    {
                        Console.WriteLine(ourAnimals[i, j]);
                    }
                }
            }
            Console.WriteLine("\n\rPress the Enter key to continue");
            readResult = Console.ReadLine();

            break;

        case "2":
            string petCharacteristic = "";
            bool noMatchesPet = true;
            string dogDescription = "";
            string[] searchingIcons = { ".  ", ".. ", "..." };

            // #1: Get the characteristics and checks if entered
            while (petCharacteristic.Length == 0)
            {
                Console.WriteLine($"\nEnter dog characteristics to search for separated by commas");
                readResult = Console.ReadLine();

                if (readResult != null)
                {
                    petCharacteristic = readResult.ToLower().Trim();
                }
            }

            string[] petSearches = petCharacteristic.Split(",");

            for (int i = 0; i < maxPets; i++)
            {
                // Have to add dogs else it'll be too long

                if (ourAnimals[i, 1].Contains("dog"))
                {
                    dogDescription = ourAnimals[i, 4] + "\r\n" + ourAnimals[i, 5];
                    Console.WriteLine($"{ourAnimals[i, 3]}");
                    // Iterating through the animals
                    for (int j = 5; j > -1; j--)
                    {
                        // #5 update "searching" message to show countdown 
                        foreach (string icon in searchingIcons)
                        {
                            Console.Write($"\rsearching our animal {ourAnimals[i, 3]} for {icon}");
                            Thread.Sleep(100);
                        }
                        Console.Write($"\r{new String(' ', Console.BufferWidth)}");
                    }
                    foreach (string petSearch in petSearches)
                    {
                        if (dogDescription.Contains(petSearch))
                        {
                            Console.WriteLine($"\nOur dog {ourAnimals[i, 3]} is a match!");

                            noMatchesPet = false;
                        }
                    }
                }
            }
            
            if (noMatchesPet)
            {
                Console.WriteLine("None of our dogs are a match found for: " + petCharacteristic);
            }
            Console.WriteLine("Press the Enter key to continue.");
            
            readResult = Console.ReadLine();
            break;

        default:
            break;
    }

} while (menuSelection != "x");

Don't judge, as I brushing up on the fundamentals to better understand the reasons and remind myself that there is something called a documentation to follow through any code.

lets update the code to be like this:
ContosoPetFriends/
├── Models/
│   └── Animal.cs
├── Interfaces/
│   └── IAnimalRepository.cs
├── Repositories/
│   └── AnimalRepository.cs
├── Services/
│   └── AnimalDisplayService.cs
└── Program.cs

string[] address = ipv4Input.Split(".", StringSplitOptions.RemoveEmptyEntries);

# Named and unnamed arguments
Notice that the named arguments don't have to appear in the original order. However, the unnamed argument Tony is a positional argument, and must appear in the matching position.

# Software testing
Software testing categories can be organized under the types of testing, the approaches to testing, or a combination of both. One way to categorize the types of testing is to split testing into Functional and Nonfunctional testing. The functional and nonfunctional categories each include subcategories of testing. For example, functional and nonfunctional testing could be divided into the following subcategories:

Functional testing - Unit testing - Integration testing - System testing - Acceptance testing
Nonfunctional testing - Security testing - Performance testing - Usability testing - Compatibility testing
Although most developers probably don't consider themselves to be testers, some level of testing is expected before a developer hands off their work. When developers are assigned a formal role in the testing process, it's often at the level of unit testing.

# Debugging
The Visual Studio Code debugger for C# uses the .NET runtime to launch and interact with an application. When you start the debugger, it creates a new instance of the runtime and runs the application within that instance. The runtime includes an application programming interface (API), which the debugger uses to attach to the running process (your application).

Once your application is running and the debugger is attached, the debugger communicates with the running process using the .NET runtime's debugging APIs and a standard debug protocol. The debugger can interact with the process (the application running within the .NET runtime instance) by setting breakpoints, stepping through code, and inspecting variables. Visual Studio Code's debugger interface enables you to navigate the source code, view call stacks, and evaluate expressions.

The most common way to specify a debug session is a launch configuration in the launch.json file. This approach is the default option enabled by the debugger tools. For example, if you create a C# console application and select Start Debugging from the Run menu, the debugger uses this approach to launch, attach to, and then interact with your application.

# Error exception handling
Common scenarios that require exception handling include:

User input: Exceptions can occur when code processes user input. For example, exceptions occur when the input value is in the wrong format or out of range.

Data processing and computations: Exceptions can occur when code performs data calculations or conversions. For example, exceptions occur when code attempts to divide by zero, cast to an unsupported type, or assign a value that's out of range.

File input/output operations: Exceptions can occur when code reads from or writes to a file. For example, exceptions occur when the file doesn't exist, the program doesn't have permission to access the file, or the file is in use by another process.

Database operations: Exceptions can occur when code interacts with a database. For example, exceptions occur when the database connection is lost, a syntax error occurs in a SQL statement, or a constraint violation occurs.

Network communication: Exceptions can occur when code communicates over a network. For example, exceptions occur when the network connection is lost, a timeout occurs, or the remote server returns an error.

Other external resources: Exceptions can occur when code communicates with other external resources. Web Services, REST APIs, or third-party libraries, can throw exceptions for various reasons. For example, exceptions occur due to network connections issues, malformed data, etc.

Reference materials
You can find additional information about exception properties here: https://learn.microsoft.com/dotnet/standard/exceptions/exception-class-and-properties and https://learn.microsoft.com/dotnet/api/system.exception.

You can find additional information about exceptions here: https://learn.microsoft.com/dotnet/csharp/language-reference/language-specification/exceptions.

You can find additional information about using specific exception types here: https://learn.microsoft.com/dotnet/standard/exceptions/how-to-use-specific-exceptions-in-a-catch-block.

You can find additional information about the try-catch-finally patterns here: https://learn.microsoft.com/dotnet/csharp/language-reference/keywords/try-catch-finally.

ArgumentException invalidArgumentException = new ArgumentException(); <- can create an instance
ArgumentException invalidArgumentException = new ArgumentException("ArgumentException: The 'GraphData' method received data outside the expected range.");
throw invalidArgumentException; <- custom message

Specific to the method:
try
    {
        BusinessProcess1(userEntries);
    }
    catch (Exception ex)
    {
        if (ex.StackTrace.Contains("BusinessProcess1") && (ex is FormatException))
        {
            Console.WriteLine(ex.Message);
        }
    }

The following list identifies practices to avoid when throwing exceptions:

Don't use exceptions to change the flow of a program as part of ordinary execution. Use exceptions to report and handle error conditions.
Exceptions shouldn't be returned as a return value or parameter instead of being thrown.
Don't throw System.Exception, System.SystemException, System.NullReferenceException, or System.IndexOutOfRangeException intentionally from your own source code.
Don't create exceptions that can be thrown in debug mode but not release mode. To identify runtime errors during the development phase, use Debug.Assert instead.

lowerbound should not be bigger than upperbound else divideByZeroError:
There are two exception types that align with the issue:

ArgumentOutOfRangeException - An ArgumentOutOfRangeException exception type should only be thrown when the value of an argument is outside the allowable range of values as defined by the invoked method. Although AverageOfEvenNumbers doesn't explicitly define an allowable range for lowerBound or upperBound, the value of lowerBound does imply the allowable range for upperBound.
InvalidOperationException: An InvalidOperationException exception type should only be thrown when the operating conditions of a method don't support the successful completion of a particular method call. In this case, the operating conditions are established by the input parameters of the method.

if (lowerBound >= upperBound)
{
    throw new ArgumentOutOfRangeException("upperBound", "ArgumentOutOfRangeException: upper bound must be greater than lower bound.");
}

# Q17 — Heap explained:

Stack → value types (int, bool, struct) and local variables
Heap → class instances created with new
The variable sits on the stack, but it holds a reference (pointer) to the actual object living on the heap. That's why classes are called reference types.