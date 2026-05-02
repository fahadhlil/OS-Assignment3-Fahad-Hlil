# Assignment 3 - Complete Documentation

**Student Name**: [fahad hlil]  
**Student ID**: [443052255]  
**Date Submitted**: [Submission Date]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: [Paste your personal Gmail Google Drive link here]

**Video filename**: `[YourStudentID]_Assignment3_Synchronization.mp4`

**Verification**:
- [ ] Link is accessible (tested in incognito mode)
- [ ] Video is 3-5 minutes long
- [ ] Video shows code walkthrough and commits
- [ ] Video has clear audio
- [ ] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [Date, Time]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 

---

### Entry 2 - [Date, Time]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 

---

### Entry 3 - [Date, Time]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 

---

### Entry 4 - [Date, Time]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 

---

### Entry 5 - [Date, Time]
**What I implemented**: 

**Challenges encountered**: 

**How I solved it**: 

**Testing approach**: 

**Time spent**: 

---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?
The first race condition occurs in the shared counter variables such as contextSwitchCount. Multiple threads may execute contextSwitchCount++ at the same time, which is not an atomic operation. This can lead to lost updates where increments are overwritten, resulting in incorrect counts. The second race condition occurs in the executionLog ArrayList, where multiple threads call executionLog.add(message) concurrently. Since ArrayList is not thread-safe, this can lead to inconsistent state or even a ConcurrentModificationException. Without synchronization, the program may produce incorrect statistics or crash during execution.
---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

ReentrantLock is a mutual exclusion mechanism that allows only one thread to access a critical section at a time, ensuring data consistency. Semaphore, on the other hand, controls access to a resource by allowing a fixed number of threads to enter simultaneously. In my implementation, I used ReentrantLock to protect shared counters and the execution log because these require exclusive access. I used a Semaphore with one permit to control CPU access, ensuring that only one process executes at a time. This design reflects real CPU scheduling behavior and prevents concurrent execution conflicts.

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

Deadlock is a situation where two or more threads are blocked forever, waiting for each other to release resources. One prevention technique is using try-finally blocks to guarantee that locks are always released, even if an exception occurs. Another technique is minimizing lock scope to reduce the chance of circular waiting. In my code, I used try-finally blocks for every lock and semaphore to ensure proper release. This prevents threads from holding resources indefinitely and avoids deadlock conditions.
---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

I used one lock (coarse-grained locking) to protect all three counters: contextSwitchCount, completedProcessCount, and totalWaitingTime. This simplifies the design and reduces the risk of deadlocks because only one lock is used. The trade-off is reduced concurrency since threads must wait even when accessing independent variables. Fine-grained locking would allow better concurrency by using separate locks for each counter, but it increases complexity and risk of errors. Since the counters are independent, fine-grained locking would provide better performance in highly concurrent systems. However, for this assignment, simplicity and correctness were prioritized.

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

**Which variables**: 
contextSwitchCount, completedProcessCount, totalWaitingTime
**Why they need protection**: 
They are shared among multiple threads and updated concurrently, leading to race conditions and incorrect values.
**Synchronization mechanism used**: 

**Code snippet**:
```java
// Paste your implementation here
```counterLock.lock();
try {
    contextSwitchCount++;
} finally {
    counterLock.unlock();
}

**Justification**: 
Ensures atomic updates and prevents inconsistent results caused by concurrent access.
---

### Critical Section #2: Execution Log

**What resource**: 
executionLog (ArrayList)
**Why it needs protection**: 
ArrayList is not thread-safe and concurrent modification can cause exceptions or data corruption.
**Synchronization mechanism used**: 
ReentrantLock
**Code snippet**:
```java
// Paste your implementation here
```logLock.lock();
try {
    executionLog.add(message);
} finally {
    logLock.unlock();
}

**Justification**: 
Guarantees safe modification of the list and prevents runtime exceptions.
---

### Critical Section #3: CPU Semaphore

**Purpose of semaphore**: 
To control access to CPU and prevent multiple processes from executing simultaneously.
**Number of permits and why**: 
1 permit to simulate a single CPU system.
**Where implemented**: 
Inside run() and runToCompletion() methods.
**Code snippet**:
```java
// Paste your implementation here
```SharedResources.cpuSemaphore.acquire();
try {
    // process execution
} finally {
    SharedResources.cpuSemaphore.release();
}

**Effect on program behavior**: 

---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
**What I tested**: Running program multiple times to verify consistent results

**Testing procedure**: 
```bash
# Commands used (run the program at least 5 times)
```
javac SchedulerSimulationSync.java
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
java SchedulerSimulationSync
**Results**: 
(Show that running multiple times produces consistent, correct results)
All runs produced consistent values for counters and no unexpected behavior occurred.
**Why synchronization is necessary**: 
(Explain what race conditions COULD occur without synchronization, even if you didn't observe them. Explain which shared resources need protection and why.)
Without synchronization, race conditions could lead to incorrect counter values and inconsistent execution logs due to concurrent updates.
**Conclusion**: 
Synchronization ensures deterministic and correct results across multiple runs.
---

### Test 2: Exception Testing
What I tested: Checking for ConcurrentModificationException

Testing procedure:
Ran the program multiple times while monitoring execution log operations.

Results:
No exceptions occurred after applying synchronization.

What this proves:
Proper locking prevents unsafe concurrent modifications to shared data structures.
---

### Test 3: Correctness Verification
What I tested: Verifying correct final values

Expected values:
All processes complete, counters reflect actual operations

Actual values:
Matched expected results in all runs

Analysis:
Synchronization ensures accurate tracking of execution statistics.

---

### Test 4: Different Scenarios
**Scenario tested**: [e.g., different time quantum, more processes, etc.]
Different time quantum and number of processes
**Purpose**: 
To verify behavior under varying workloads
**Results**: 
Program remained stable and produced correct results
**What I learned**: 
Synchronization works reliably regardless of workload changes.
---

## Part 5: Reflection and Learning

### What I learned about synchronization:

[6-8 sentences about key concepts, challenges, insights]
I learned that synchronization is essential in multithreaded environments to ensure data consistency. Race conditions can lead to unpredictable and incorrect results if not handled properly. Locks provide mutual exclusion, while semaphores control resource access. Proper use of try-finally is critical to avoid deadlocks. I also understood the trade-offs between simplicity and performance in lock design. Debugging synchronization issues requires careful analysis of shared resources. Overall, synchronization is a fundamental concept in operating systems.
---

### Real-world applications:

Give TWO examples where synchronization is critical:

**Example 1**: Banking systems where multiple transactions update account balances

**Example 2**: Operating system schedulers managing CPU access among processes

---

### How I would explain synchronization to others:

[Explain to someone who just finished Assignment 1 - use simple terms and analogies]

---
Synchronization is like controlling access to a shared resource, similar to a single key for a locked room. Only one person can enter at a time to prevent conflicts. Without it, multiple users may interfere with each other and cause problems. It ensures safe and orderly execution of tasks in concurrent systems.
## Part 6: GitHub Repository Information

**Repository URL**: 

**Number of commits**: 8

**Commit messages**: 
1. Add synchronization primitives (locks and semaphore)
Protect shared counters using ReentrantLock
Protect execution log using ReentrantLock
Add semaphore for CPU synchronization
Apply semaphore to runToCompletion method
. 


---

## Summary

**Total time spent on assignment**: 6-8 hours

**Key takeaways**: 
1. 
2. 
3. 

**Most challenging aspect**: 

**What I'm most proud of**: 

---

**End of Documentation**
2