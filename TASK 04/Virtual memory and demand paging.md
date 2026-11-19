
# Virtual Memory, Demand Paging, Copy-on-Write and Thrashing

## 1. Aim

To study virtual memory, demand paging, page faults, copy-on-write, and thrashing.

---

## 2. Demand Paging

Demand paging loads a page into RAM only when it is required.

### Given

- Program memory required = 24 GB
- Available RAM = 16 GB

The program can still run because only the currently required pages need to remain in RAM. Other pages can remain on secondary storage.

---

## 3. Steps in Handling a Page Fault

1. CPU requests a page.
2. Page table checks whether the page is present.
3. If absent, a page fault occurs.
4. OS checks whether the memory reference is valid.
5. OS locates the required page on disk.
6. A free frame is selected or another page is replaced.
7. Required page is loaded into RAM.
8. Page table is updated.
9. The interrupted instruction is restarted.

### Page Fault Handling Flow

```text
CPU Requests Page
       |
       v
Page Table Check
       |
       v
Page Present?
   /        \
 Yes         No
  |           |
  v           v
Continue   Page Fault
Execution      |
               v
       Check Validity
               |
               v
       Locate Page on Disk
               |
               v
       Select Free Frame
               |
               v
       Load Page into RAM
               |
               v
       Update Page Table
               |
               v
       Restart Instruction
```

---

## 4. Effective Access Time Calculation

### Given

- Memory access time = 100 ns
- Page fault service time = 8 ms = 8,000,000 ns
- Page fault rate = \(p\)

### Formula

\[
EAT = (1-p)(100) + p(8,000,000)
\]

### Case 1: 1 in 1,000

\[
p = 0.001
\]

\[
EAT = 0.999(100) + 0.001(8,000,000)
\]

\[
EAT = 99.9 + 8000
\]

\[
\boxed{EAT = 8099.9\ ns}
\]

### Case 2: 1 in 100,000

\[
p = 0.00001
\]

\[
EAT = 0.99999(100) + 0.00001(8,000,000)
\]

\[
EAT = 99.999 + 80
\]

\[
\boxed{EAT = 179.999\ ns}
\]

### Case 3: 1 in 1,000,000

\[
p = 0.000001
\]

\[
EAT = 0.999999(100) + 0.000001(8,000,000)
\]

\[
EAT = 99.9999 + 8
\]

\[
\boxed{EAT = 107.9999\ ns}
\]

### Results

| Page Fault Rate | Effective Access Time |
|---|---:|
| 1 in 1,000 | 8099.9 ns |
| 1 in 100,000 | 179.999 ns |
| 1 in 1,000,000 | 107.9999 ns |

---

## 5. Copy-on-Write (COW)

Copy-on-write allows parent and child processes to share the same physical memory pages after `fork()`.

### Working

- Initially, both processes share memory pages.
- Shared pages are marked read-only.
- When one process modifies a page, the OS creates a separate copy.
- This reduces unnecessary memory copying and makes `fork()` faster.

### Advantages

- Reduces memory usage.
- Improves process creation efficiency.
- Avoids unnecessary copying of memory.

---

## 6. Thrashing and Working-Set Model

### Thrashing

Thrashing is a condition in which the system spends more time swapping pages than executing processes.

### Causes

- Insufficient physical memory.
- Too many processes.
- Frequent page faults.

### Working-Set Model

The working-set model keeps the actively used pages of a process in memory.

### Methods to Reduce Thrashing

- Reduce the degree of multiprogramming.
- Allocate sufficient frames.
- Monitor page-fault frequency.
- Suspend some processes.

---


## 7. Evaluation

- Equal frame allocation gives each process the same number of frames.
- Proportional allocation gives more frames to larger processes.
- The OS can detect thrashing using page-fault frequency.
- A 10% slowdown limit means the acceptable effective access time is approximately:

\[
100 \times 1.10 = 110\ ns
\]

Using the given 8 ms fault service time, the page-fault rate must be approximately no greater than:

\[
p \leq \frac{10}{8,000,000 - 100}
\]

\[
\boxed{p \leq 1.25 \times 10^{-6}}
\]

---

## 8. Conclusion

Demand paging allows a 24 GB program to run on a laptop with 16 GB RAM. However, frequent page faults reduce performance.

Improving program locality and reducing page faults is preferable to depending only on virtual memory.

