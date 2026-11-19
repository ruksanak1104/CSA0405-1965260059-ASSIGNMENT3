
# Paging and Logical Address Translation

## 1. Aim
To implement paging and translate logical addresses into physical addresses using a page table.

---

## 2. External Fragmentation

External fragmentation occurs when free memory is divided into small, non-contiguous blocks.

Paging eliminates external fragmentation because a process can be stored in separate physical frames instead of requiring contiguous memory.

---

## 3. Address Translation

A logical address is divided into two parts:

- **Page Number (p):** Identifies the page.
- **Offset (d):** Identifies the location within the page.

### Formula

\[
\text{Physical Address} = \text{Frame Number} \times \text{Page Size} + \text{Offset}
\]

---


## 4. Page-Table Size Calculation

### Given

- Page size = 4 KB = \(2^{12}\) bytes
- Page-table entry size = 4 bytes (assumption)

### For a 32-bit Address Space

Number of pages:

\[
\frac{2^{32}}{2^{12}} = 2^{20}
\]

Page-table size:

\[
2^{20} \times 4 = 4\ MB
\]

### For a 48-bit Address Space

Number of pages:

\[
\frac{2^{48}}{2^{12}} = 2^{36}
\]

Page-table size:

\[
2^{36} \times 4 = 256\ GB
\]

---

## 5. Effective Memory Access Time (EAT)

### Given

- Memory access time = 100 ns
- TLB lookup time = 10 ns
- TLB hit ratio = 90%

### Without TLB

\[
EAT = 2 \times 100
\]

\[
EAT = 200\ ns
\]

### With TLB

\[
EAT = 0.9(10+100) + 0.1(10+200)
\]

\[
EAT = 99 + 21
\]

\[
\boxed{EAT = 120\ ns}
\]

---

## 6. Evaluation

- Paging removes external fragmentation.
- Internal fragmentation may occur in the last page.
- 1 KB pages require more page-table entries.
- 4 KB pages provide a balance between memory usage and page-table size.
- 2 MB pages reduce page-table entries but may waste memory.
- Hierarchical page tables divide page tables into multiple levels.
- Hashed page tables use hashing to locate page entries.
- Inverted page tables maintain one entry per physical frame.
- Protection bits control read, write, and execute permissions.
- Shared pages allow multiple processes to use the same code.

---

## 7. Conclusion

I recommend using 4 KB pages for the college server because they provide a reasonable balance between memory utilization, page-table size, and fragmentation.

