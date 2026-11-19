## Experimental C Program - Page Faults

### Program

```c
#include <stdio.h>

#define SIZE 100
#define PAGE_SIZE 4096

int main()
{
    int memory[SIZE * PAGE_SIZE];
    int pageFaults = 0;

    for (int i = 0; i < SIZE * PAGE_SIZE; i += PAGE_SIZE)
    {
        memory[i] = 10;
        pageFaults++;
    }

    printf("Pages accessed: %d\n", pageFaults);
    printf("Sequential memory access completed\n");

    return 0;
}
```

### Compilation

```bash
gcc demand.c -o demand
```

### Execution

```bash
./demand
```

### Sample Output

```text
Pages accessed: 100
Sequential memory access completed
```

---

