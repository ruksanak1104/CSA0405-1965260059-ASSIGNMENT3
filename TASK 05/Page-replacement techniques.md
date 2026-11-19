
# Page Replacement Algorithms: FIFO, LRU and Optimal

## 1. Aim

To implement FIFO, LRU, and Optimal page-replacement algorithms and compare their page faults.

---

## 2. Algorithms

### FIFO (First-In, First-Out)
- Replaces the page that entered memory first.
- Simple to implement using a queue.
- May replace frequently used pages.

### LRU (Least Recently Used)
- Replaces the page that has not been used for the longest time.
- Uses recent page-reference history.
- Generally performs well when programs have locality of reference.

### Optimal Page Replacement
- Replaces the page that will not be used for the longest time in the future.
- Produces the minimum possible page faults.
- Used as a theoretical benchmark because future references are unknown.

---



## 3. Comparison of Page Faults

### Reference String

```text
7, 0, 1, 2, 0, 3, 0, 4, 2, 3,
0, 3, 2, 1, 2, 0, 1, 7, 0, 1
```

### Results

| Number of Frames | FIFO | LRU | Optimal |
|---|---:|---:|---:|
| 3 | 15 | 12 | 9 |
| 4 | 10 | 10 | 8 |

---

## 4. Belady's Anomaly

Belady's anomaly occurs when increasing the number of page frames increases the number of page faults.

### Given FIFO Reference String

```text
1, 2, 3, 4, 1, 2, 5, 1, 2, 3, 4, 5
```

### Results

| Frames | FIFO Page Faults |
|---|---:|
| 3 | 9 |
| 4 | 10 |

Therefore, FIFO demonstrates Belady's anomaly because increasing the frames from 3 to 4 increases the page faults from 9 to 10.

LRU and Optimal are stack algorithms and do not suffer from Belady's anomaly.

---

## 5. Evaluation

| Algorithm | Advantages | Disadvantages |
|---|---|---|
| FIFO | Simple to implement | May replace frequently used pages |
| LRU | Uses recent page history | Requires tracking page usage |
| Optimal | Minimum possible page faults | Future references are unknown |

### Clock (Second Chance) Algorithm

- Uses a reference bit for each page.
- If the bit is 0, the page can be replaced.
- If the bit is 1, the bit is cleared and the page receives another chance.
- It approximates LRU without maintaining exact usage timestamps.

---

## 6. Recommendation

LRU is recommended for the web server because frequently accessed pages are likely to be used again. It performs well when the workload has strong locality.

Optimal gives the lowest possible page faults, but it cannot be implemented practically because future page references are unknown.

---

## 7. Conclusion

FIFO is simple but may suffer from Belady's anomaly. LRU provides a practical balance between performance and implementation. Optimal is useful as a theoretical benchmark because it produces the minimum possible page faults.
```

**GitHub file name:** `README.md` or `Page_Replacement_Algorithms.md`

Paste it into your repository, commit the changes, and use **Preview** to view the formatted tables and code. 💻
