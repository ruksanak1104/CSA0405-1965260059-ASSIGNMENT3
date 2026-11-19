
# Memory Management Design for a Smart Factory Edge Device

## 1. Aim

To design and evaluate a memory-management plan for a smart factory edge device with 2 GB RAM and no swap space.

---

## 2. Stage 1 – Interpret the Problem 
### Given Requirements

- Total RAM = 2 GB.
- Real-time control task must never be delayed by a page fault.
- Machine-learning task requires 1.5 GB.
- Logging task stores machine data.
- Future requirement: adding more tasks.

### Task Analysis

| Task | Memory Requirement | Timing Requirement | Page Faults |
|---|---|---|---|
| Real-time control | Small, fixed memory | Strict deadline | Not acceptable |
| Machine learning | 1.5 GB | Can tolerate some delay | Acceptable |
| Logging | Variable memory | Less time-critical | Acceptable |

### Measurable Requirements

1. Control-task memory must remain resident in RAM.
2. The control task must not perform dynamic memory allocation during its critical execution.
3. Machine-learning memory usage must be limited to the available memory budget.
4. Logging must not consume memory reserved for real-time control.

---

## 3. Stage 2 – Compare Candidate Mechanisms 

| Mechanism | Advantages | Disadvantages | Suitable Task |
|---|---|---|---|
| Contiguous Allocation | Simple and fast | External fragmentation | Fixed control memory |
| Paging | Eliminates external fragmentation | Page-table overhead and page faults | Machine learning |
| Segmentation | Logical memory division | External fragmentation | Code, stack and data organization |
| Demand Paging | Loads pages when needed | Page faults cause delays | Machine learning and logging |

### Recommended Memory Allocation

| Component | Memory Allocation |
|---|---:|
| Operating system and reserved memory | 256 MB |
| Real-time control | 128 MB |
| Machine learning | 1,536 MB |
| Logging | 128 MB |
| Safety reserve | 0 MB |
| **Total** | **2,048 MB (2 GB)** |

This is an illustrative budget. Actual OS memory overhead must be measured and included before deployment.

### Memory Locking Using `mlock()`

- `mlock()` locks a memory region into RAM.
- Locked pages cannot be swapped out.
- The control task can use preallocated and locked memory.
- The OS must reserve sufficient RAM for other system activities.

---



### Linux Memory Lock Limit

If `mlock()` fails because of the system's memory-lock limit, check:

```bash
ulimit -l
```

---

## 4. Page-Replacement Simulation

For the three tasks, page replacement should be handled differently:

- **Control task:** Its critical pages remain locked in memory.
- **Machine-learning task:** Uses its allocated frames and can tolerate page faults.
- **Logging task:** Uses a limited buffer and writes data to storage periodically.

The control task must not have its locked frames selected for replacement.

---

## 5. Stage 4 – Defend the Design 

### Why Is This Design Suitable?

1. Locked memory prevents the control task's critical pages from being swapped out.
2. Fixed memory allocation reduces unpredictable allocation delays.
3. Machine learning can use the remaining memory with controlled page replacement.
4. Logging receives a limited memory allocation to prevent interference.
5. Memory monitoring helps prevent memory exhaustion.

### If Another 500 MB Task Is Added

- Available RAM must be recalculated.
- The machine-learning allocation may need to be reduced.
- Logging buffers may be adjusted.
- The control task's reserved memory must not be reduced.
- If memory is insufficient, the new task should be moved to another device or rejected.

### Remaining Limitation

Even with locked memory, CPU scheduling, interrupts, hardware delays, and I/O can affect the real-time task.

A real-time OS and worst-case execution-time testing are needed for strict guarantees.

---

## 6. Conclusion

The proposed design uses locked memory for the real-time control task, paging for machine learning, and controlled buffers for logging.

This reduces page-fault-related delays and improves memory utilization. Therefore, memory locking with controlled allocation is suitable for the smart factory edge device.
```



