# Java Fundamentals Checklist

**Recorded:** 2026-10-22

## Context

Java language fundamentals checklist, delivered October 22, 2026. The goal is to cover only the Java language itself. The Spring Boot application layer Robin writes every day, the Kafka and Elasticsearch pipeline, and LeetCode in JavaScript are not on this list. The method: work top to bottom. For each item, read "How they ask it", cover the answer, and say the 30-second version out loud, then check whether the follow-up depth can be handled. Status: open. The quiz offer is standing and unanswered.

## Study order advice

First, take Tier 1 items 1 to 4 (HashMap, ConcurrentHashMap, synchronized versus volatile, ThreadPoolExecutor). These four are the opening set for a Staff backend round, and missing them costs points directly. Then work through Tier 2, with the focus on item 6 (double-checked locking, the practical volatile question) and item 8 (garbage collection, and checking which garbage collector the production systems use so the answer becomes personal). Tier 3 is as needed; items 11 and 12 give the best return for the effort.

## Tier 1: asked in almost every round

### 1. HashMap internals

**How they ask it:** "How is HashMap implemented? What actually happens when you put a key-value pair in?"

**One-sentence answer:** An array plus a linked list. The hash code of the key goes through a spread function (the high 16 bits are XORed with the low 16 bits), then the result is taken modulo the array length to find a bucket. If the bucket is empty, the entry goes straight in. If the bucket already has entries, walk the linked list looking for a node with an equal key; if one is found, overwrite it, and if not, append to the tail. When a linked list grows past 8 nodes and the array length is over 64, the list turns into a red-black tree. The array length is always a power of two, and resizing doubles it.

**Follow-up depth:**

- Why must the array length be a power of two? Because the modulo is done with the bit operation `hash & (length - 1)`, and that expression is only equal to a modulo when the length is a power of two, and it is fast.
- What happens when two keys have exactly the same hash code? They land in the same bucket, walk the list or tree, and are told apart by `equals`.
- What is the hashCode and equals contract? Two objects that are equal must have equal hash codes; the reverse is not required. Breaking the contract means the same logical key gets stored twice in the HashMap and cannot be found with `get`.

**Trap notes:** When reciting "a list longer than 8 turns into a red-black tree", do not leave out the second condition, that the array length must also be over 64. Do not call the spread function optional; it exists so the high bits also take part in choosing the bucket, which reduces collisions.

**Self-test:** `new HashMap<>(100)` allocates an array of what size? What happens when the 13th element is put in?

### 2. ConcurrentHashMap (Java 8 and later)

**How they ask it:** "How does ConcurrentHashMap stay thread-safe? How is it different from Hashtable?"

**One-sentence answer:** From Java 8 on, it is an array plus linked lists or red-black trees, the same structure as HashMap. A write happens in two steps: first use CAS (Compare-And-Swap, an atomic compare-and-replace operation that needs no lock) to claim the bucket, and once claimed, use `synchronized` to lock only the head node of that bucket. Reads are almost lock-free. So concurrency is high: different buckets can be written at the same time.

**Follow-up depth:**

- How is it different from version 1.7? Version 1.7 used segment locks, splitting the whole table into 16 segments and locking one segment at a time. Version 1.8 locks a single bucket, which is a finer lock.
- Why not lock the whole table with a synchronized method, like Hashtable does? Locking the whole table means only one thread can read or write at a time, and the table blocks as soon as concurrency rises.

**Trap notes:** This is the most common place to fail. Many study guides still describe the 1.7 segments. If the first words out of your mouth are "segment locks, 16 segments", the interviewer assumes you memorized the outdated version. Say the 1.8 answer first, CAS plus bucket-head lock, and then add one sentence: "1.7 used segment locks, that is the old version."

**Self-test:** Two threads put into the same empty bucket at the same time. What happens? Answer: one CAS (Compare-And-Swap) succeeds and places the entry directly, the other CAS (Compare-And-Swap) fails and then queues on the synchronized lock of the bucket head.

### 3. synchronized and volatile

**How they ask it:** "What is the difference between synchronized and volatile?"

**One-sentence answer:** `synchronized` is a lock. It guarantees that only one thread enters the critical section at a time, and it also guarantees visibility (unlocking flushes changes back to main memory, and locking reads them again). `volatile` is a light marker. It guarantees visibility (when one thread writes, every thread immediately sees the new value) and it forbids instruction reordering, but it does not guarantee atomicity.

**Follow-up depth:**

- What does `synchronized` lock? On an instance method it locks `this`, the object itself. On a static method it locks the Class object. In `synchronized(someObject)` it locks the object inside the parentheses. Locking the wrong object is the same as not locking at all.
- Is `i++` on a volatile variable thread-safe? No. `i++` is three steps, read, add, write. Volatile only guarantees that each step sees the latest value; it does not guarantee that the three steps together cannot be interrupted.

**Trap notes:** "Volatile guarantees atomicity" is a question designed to catch you. The answer is always "it does not guarantee atomicity".

**Self-test:** A boolean flag controls whether a thread stops looping. Why must it be volatile? Answer: without it, the working thread may keep reading the old value from its own CPU cache forever and never stop.

### 4. ThreadPoolExecutor

**How they ask it:** "What parameters does a thread pool have? How does a submitted task flow through it?"

**One-sentence answer:** Seven parameters: core thread count, maximum thread count, idle thread keep-alive time, time unit, task queue, thread factory, and rejection policy. When a task arrives, first check whether the core threads are full. If not full, start a new thread. If full, the task waits in the queue. If the queue is also full, start threads up to the maximum count. If still full, trigger the rejection policy.

**Follow-up depth:**

- What are the four rejection policies? Throw an exception (Abort, the default), let the submitting thread run the task itself (CallerRuns), drop the oldest task at the head of the queue (DiscardOldest), or drop the new task directly (Discard).
- How do you pick the core thread count? For CPU-bound work, core count plus 1. For input and output bound work, about core count times 2, because the threads spend most of their time waiting on input and output, so extra threads do not waste CPU (Central Processing Unit) time. A Kafka consumer, which waits on input and output, belongs to the second group.
- Why call shutdown when the pool is done? Without it, the non-daemon threads stay alive and the JVM (Java Virtual Machine) cannot exit.

**Trap notes:** `Executors.newFixedThreadPool` uses an unbounded queue (LinkedBlockingQueue), so piled-up tasks can eat all the memory. Writing a ThreadPoolExecutor by hand with a bounded queue is a plus in production code, and the Citi consumer scenario is a place to mention it.

**Self-test:** Core is 2, maximum is 4, queue capacity is 10. What happens when the 15th task is submitted? Answer: 2 core threads run 2 tasks, 10 go into the queue, the 13th and 14th trigger new threads up to the maximum of 4, and the 15th goes to the rejection policy.

### 5. equals and hashCode contract (warm-up question)

**How they ask it:** "Why must you override hashCode whenever you override equals?"

**One-sentence answer:** Because HashMap and HashSet first use hashCode to find the bucket, then use equals to find the entry. If you override equals but not hashCode, two logically equal objects land in different buckets, the map stores two copies, and `get` cannot find the entry.

**Self-test:** A class overrides only equals. Two objects that are equal to each other are added to a HashSet. What is the size of the set? Answer: 2, and that is the bug.

## Tier 2: asked in most rounds

### 6. Double-checked locking singleton (the classic volatile use case)

**How they ask it:** "Write a thread-safe singleton."

**One-sentence answer:** Double-checked locking: check for null outside the lock first, so you avoid locking on every call, then take the synchronized lock, then check for null a second time inside, so two threads that both passed the first check do not both create an instance, and then create it. The instance field must be volatile, because creating an object is three steps (allocate memory, initialize, assign the reference), and without volatile, instruction reordering can let another thread see a reference to an object that is not yet fully initialized.

**Trap notes:** Writing double-checked locking but leaving out volatile means the code is worthless. That is exactly what the question is testing.

**Self-test:** Without volatile, what kind of instance might thread B see? Answer: a non-null instance whose fields are all still default values, 0 or null, a half-finished object.

### 7. ReentrantLock versus synchronized

**How they ask it:** "What does ReentrantLock do that synchronized cannot?"

**One-sentence answer:** Three abilities synchronized does not have: interruptible locking (`lockInterruptibly`, so a thread waiting for a lock can be interrupted), timeout (`tryLock` waits a few seconds and then gives up), and fair locking (locks are handed out in queue order). In addition, a Condition supports multiple wait and notify sets, while synchronized `wait` and `notify` have only one wait set.

**Follow-up depth:** Underneath, both are built on AQS (AbstractQueuedSynchronizer, a lock framework built from a state variable plus a waiting queue). ReentrantLock, CountDownLatch, and Semaphore are all built on it. Saying that one sentence earns depth points.

### 8. Garbage collection basics

**How they ask it:** "How is JVM (Java Virtual Machine) memory divided? What is the difference between a minor garbage collection and a full garbage collection?"

**One-sentence answer:** The heap is split into a young generation and an old generation. Objects are born in the young generation, most die young, and one minor garbage collection (which scans only the young generation, and is fast and frequent) reclaims them. Objects that survive several collections are promoted to the old generation. When the old generation fills up, a full garbage collection runs (it scans the whole heap, and is slow and rare, with a long stop-the-world time). The method area (metaspace) holds class information and is not in the heap.

**Follow-up depth:**

- Which garbage collector is used in production? The standard answer is G1 (Garbage First, which cuts the heap into many small regions and reclaims the regions with the most garbage first, so the stop-the-world time is predictable). This is a favourite Staff interview question about production experience. The garbage collector used by the Elasticsearch service and the Kafka consumer Robin runs every day is worth checking, so the answer becomes personal.
- What is stop-the-world? It means the garbage collector pauses all application threads while it runs. When an interviewer asks about garbage collection, they are really asking whether you know where the pause time comes from.

### 9. Thread states and deadlock

**How they ask it:** "What thread states are there? What is a deadlock, and how do you find one?"

**One-sentence answer:** Six states: NEW (not started yet), RUNNABLE (running or waiting for the CPU), BLOCKED (waiting for a synchronized lock), WAITING (waiting to be woken by another thread, such as `wait` or `join`), TIMED_WAITING (waiting with a timeout, such as `sleep`), and TERMINATED (finished). A deadlock is when two threads each hold a lock the other one wants. It needs four conditions: mutual exclusion, hold and wait, no preemption, and circular wait. Breaking any one of them resolves it.

**Follow-up depth:** How do you find one in production? Use `jstack` to dump the threads and look for BLOCKED threads and which lock each one is waiting for, or use `jconsole` to look at it visually. Being able to say `jstack` is enough.

### 10. Atomic and CAS (Compare-And-Swap)

**How they ask it:** "What is the difference between AtomicInteger and a synchronized counter?"

**One-sentence answer:** AtomicInteger uses CAS (Compare-And-Swap: compare the value in memory with the expected value, and replace it only if they match, otherwise retry). It has no lock, and when conflicts are rare it is much faster than synchronized. Synchronized is a pessimistic lock: it locks first whether or not there is a conflict.

**Follow-up depth:** The ABA problem: thread A sees the value 1, thread B changes it to 2 and then back to 1, and thread A's CAS (Compare-And-Swap) still succeeds even though the value changed in between. Solve it with AtomicStampedReference, which carries a version number. Being able to say "ABA" earns depth points.

## Tier 3: depth rounds and bonus questions

### 11. Happens-before rules

**How they ask it:** "Why does volatile guarantee visibility?"

**One-sentence answer:** The Java memory model defines happens-before rules: a write to a volatile variable happens-before a later read of it, so the written value is always visible to the reader. Other useful rules: unlocking happens-before locking, everything before a thread starts happens-before the work inside the thread, and everything inside a thread happens-before the `join` call returns.

**One-sentence positioning:** This is the theory underneath item 3. Bring it out only when the interviewer asks "why".

### 12. ThreadLocal memory leak

**How they ask it:** "What happens if you use ThreadLocal and never call remove?"

**One-sentence answer:** In the ThreadLocalMap, the key is a weak reference (the garbage collector can reclaim it), but the value is a strong reference. After the key is reclaimed, the value is still there, and it can never be reached or freed again. In a thread pool the threads never die, so that memory leaks forever. Always call remove when done.

### 13. Class-loading parent delegation (low frequency, know it at a glance)

**One-sentence answer:** When a class loader looks for a class, it first asks its parent loader, and only looks itself when the parent cannot find it. This guarantees that core classes, such as `java.lang.String`, always come from the JDK (Java Development Kit), and nobody can override them by writing a class with the same name. It is a safety rule.

## Past mistakes & points to improve

- This checklist is the agreed Java-drilling track from the October 21 two-track strategy: keep applying to JVM (Java Virtual Machine) Staff backend roles for volume, while selectively targeting Node.js and TypeScript backend roles. The quiz is the next step, and the offer is still standing and unanswered.
- Trap to carry into the interview: ConcurrentHashMap 1.8 uses CAS (Compare-And-Swap) plus a bucket-head lock, not the 1.7 segment locks. Say 1.8 first.
- Trap to carry into the interview: volatile does not guarantee atomicity. The answer to that question is always no.
- Trap to carry into the interview: double-checked locking without volatile is worthless. The volatile on the instance field is the point of the question.
- For the garbage collection answer, check which collector the Elasticsearch service and the Kafka consumer use in production, and make the answer personal instead of reciting G1 (Garbage First) from a guide.
