# Assignment 3 - Complete Documentation

**Student Name**: [atyaf sultan alammar]  
**Student ID**: [445052083]  
**Date Submitted**: [Submission Date]

---

## 🎥 VIDEO DEMONSTRATION LINK (REQUIRED)

> **⚠️ IMPORTANT: This section is REQUIRED for grading!**
> 
> Upload your 3-5 minute video to your **PERSONAL Gmail Google Drive** (NOT university email).
> Set sharing to "Anyone with the link can view".
> Test the link in incognito/private mode before submitting.

**Video Link**: https://drive.google.com/file/d/1SRdlWSHAlPuuKupy3pn5eGOVp28bVmz1/view?usp=sharing

**Video filename**: `445052083_Assignment3_Synchronization.mp4`

**Verification**:
- [✔ ] Link is accessible (tested in incognito mode)
- [✔ ] Video is 3-5 minutes long
- [✔ ] Video shows code walkthrough and commits
- [✔ ] Video has clear audio
- [ ✔] Uploaded to PERSONAL Gmail (not @std.psau.edu.sa)

---

## Part 1: Development Log (1 mark)

Document your development process with **minimum 3 entries** showing progression:

### Entry 1 - [5/5,2:46]
What I implemented:
I started understanding the Round Robin scheduler code and tested the original program.

Challenges encountered:
I was confused about how threads execute together.

How I solved it:
I followed the Process class step by step and checked the output.

Testing approach:
I ran the program several times to understand the execution.

Time spent:
1 hour
---

### Entry 2 - [5/5, 4:00]
What I implemented:
I added ReentrantLock for shared counters and execution logs.

Challenges encountered:
I got some compilation errors while adding synchronization.

How I solved it:
I fixed the errors and used try-finally blocks correctly.

Testing approach:
I checked if the counters updated correctly after execution.

Time spent:
1.5 hours

---

### Entry 3 - [5/5,5:30 ]
What I implemented:
I implemented Semaphore to control CPU access.

Challenges encountered:
The program became slow and I had InterruptedException problems.

How I solved it:
I added try-catch-finally blocks and tested smaller values first.

Testing approach:
I tested if only one process used the CPU at a time.

Time spent:
2 hours

---

### Entry 4 - [5/5, 7:30]
What I implemented:
I tested the final output and verified the synchronization results.

Challenges encountered:
I accidentally duplicated code inside the run method.

How I solved it:
I removed the duplicated code and cleaned the method.

Testing approach:
I verified completed processes, waiting time, and context switches.

Time spent:
1 hour
---

### Entry 5 - [5/5, 10:00]
What I implemented:
I finished the final testing and prepared the project for submission.

Challenges encountered:
I had some issues with GitHub push and output formatting.

How I solved it:
I fixed the errors and checked the program output again before submission.

Testing approach:
I ran the program multiple times and reviewed the final results.

Time spent:
45 minutes
---

## Part 2: Technical Questions (1 mark)

### Question 1: Race Conditions
**Q**: Identify and explain TWO race conditions in the original code. For each:
- What shared resource is affected?
- Why is concurrent access a problem?
- What incorrect behavior could occur?

**Your Answer**:
Race condition means more than one thread tries to use the same variable at the same time. This can make the result wrong.

The first shared resource was the counters like contextSwitchCount and completedProcessCount. If many threads update them together, some values may not be counted correctly.

The second shared resource was the execution log. If many threads write in the log together, it can cause errors or missing data.

I used ReentrantLock to make sure only one thread can access the shared resource at a time.


---

### Question 2: Locks vs Semaphores
**Q**: Explain the difference between ReentrantLock and Semaphore. Where did you use each in your code and why?

**Your Answer**:
ReentrantLock is used to protect shared variables from being changed by many threads together. I used it for counters and logs.

Semaphore is used to control access to the CPU. I used one permit so only one process can use the CPU at one time.

Locks protect variables while semaphores control resource access between processes.

---

### Question 3: Deadlock Prevention
**Q**: What is deadlock? Explain TWO prevention techniques and what you did to prevent deadlocks in your code.

**Your Answer**:

Deadlock happens when threads wait for each other and the program stops working correctly.

I used try-finally blocks to make sure locks and semaphores are released after use.

I also avoided using too many locks together to keep the synchronization simple.

This helped prevent deadlock in the program.
---

### Question 4: Lock Granularity Design Decision 
**Q**: For Task 1 (protecting the three counters), explain your lock design choice:
- Did you use ONE lock for all three counters (coarse-grained) OR separate locks for each counter (fine-grained)?
- Explain WHY you made this choice
- What are the trade-offs between the two approaches?
- Given that the three counters are independent, which approach provides better concurrency and why?

**Your Answer**:

I used separate locks for different shared resources. This is called fine-grained locking.

I chose this because the counters and execution log are different resources and do not depend on each other.

Using one lock for everything is easier but slower because all threads must wait.

Separate locks give better concurrency because more than one thread can work at the same time.

---

## Part 3: Synchronization Analysis (1 mark)

### Critical Section #1: Counter Variables

Which variables:
contextSwitchCount, completedProcessCount, and totalWaitingTime.

Why they need protection:
Many threads can update these counters at the same time which may cause incorrect values.

Synchronization mechanism used:
ReentrantLock.
**Code snippet**:
private static final ReentrantLock counterLock = new ReentrantLock();

public static void incrementContextSwitch() {
    counterLock.lock();
    try {
        contextSwitchCount++;
    } finally {
        counterLock.unlock();
    }
}

**Justification**: 
The lock makes sure only one thread updates the counters at a time.
---

### Critical Section #2: Execution Log

What resource:
executionLog ArrayList.

Why it needs protection:
Multiple threads writing to the log together may cause errors or missing log messages.

Synchronization mechanism used:
ReentrantLock.
**Code snippet**:
private static final ReentrantLock logLock = new ReentrantLock();

public static void logExecution(String message) {
    logLock.lock();
    try {
        executionLog.add(message);
    } finally {
        logLock.unlock();
    }
}

Justification:
The lock protects the ArrayList and keeps log entries correct.
---

### Critical Section #3: CPU Semaphore

Purpose of semaphore:
To control CPU access between processes.

Number of permits and why:
One permit because only one process should use the CPU at a time.

Where implemented:
Inside the run method before process execution.

**Code snippet**:
SharedResources.cpuSemaphore.acquire();

try {
    // process execution
} finally {
    SharedResources.cpuSemaphore.release();
}

Effect on program behavior:
It prevents multiple processes from accessing the CPU together.
---

## Part 4: Testing and Verification (2 marks)

### Test 1: Consistency Check
What I tested:
Running the program multiple times to check if the results stay correct.

Testing procedure:
I ran the program 5 times using VS Code.

Results:
The program completed successfully every time and the output values were correct.

Why synchronization is necessary:
Without synchronization, shared counters and logs may have incorrect values because many threads can access them together.

Conclusion:
Synchronization helped keep the program stable and correct.
---

### Test 2: Exception Testing
What I tested:
Checking for ConcurrentModificationException.

Testing procedure:
I tested the execution log while many processes were running together.

Results:
No ConcurrentModificationException happened after using locks.

What this proves:
The synchronization protected the shared ArrayList correctly.
---

### Test 3: Correctness Verification
What I tested:
Checking final values like completed processes and waiting time.

Expected values:
All processes should complete correctly and counters should show correct totals.

Actual values:
The output showed all processes completed and the counters worked correctly.

Analysis:
The synchronization methods worked correctly and prevented incorrect updates.
---

### Test 4: Different Scenarios
Scenario tested:
Changing time quantum and number of processes.

Purpose:
To see how the scheduler behaves in different situations.

Results:
The program worked correctly with different values.

What I learned:
Different values affect execution speed and context switching behavior.
---

## Part 5: Reflection and Learning

### What I learned about synchronization:

I learned how synchronization helps protect shared resources between threads. I understood why race conditions happen and how locks can prevent them. I also learned how semaphores control access to the CPU. At first, synchronization was confusing because many threads run together. After testing the program, I understood how threads interact with each other. I also learned the importance of try-finally blocks when using locks and semaphores. This assignment helped me understand multithreading better.

---

### Real-world applications:

Give TWO examples where synchronization is critical:

Example 1:
Bank systems where many users access the same account balance.

Example 2:
Hospital systems where many employees update patient information together.

---

### How I would explain synchronization to others:

Synchronization means organizing thread access to shared resources to avoid problems. It works like a queue where only one person can use something at one time. Locks protect shared variables while semaphores control access to resources like the CPU. Without synchronization, programs may give wrong results because many threads work together at the same time.
---

## Part 6: GitHub Repository Information

**Repository URL**: https://github.com/atyaf-sultan-2083/OS-Assignment3-Atyaf-sultan.git

**Number of commits**: 7

**Commit messages**: 
1. set my student id
2.implemented synchronization mechanisms 
3. add semphore
4. fix the code

---

## Summary

**Total time spent on assignment**: 
15h
**Key takeaways**: 
1. Synchronization is important for protecting shared resources.
2. Locks and semaphores help prevent race conditions.
3. Multithreading requires careful resource management. 

**Most challenging aspect**: 
Understanding synchronization and fixing thread-related errors in the program.
**What I'm most proud of**: 
Successfully implementing synchronization using ReentrantLock and Semaphore and getting the correct output.
---

**End of Documentation**
