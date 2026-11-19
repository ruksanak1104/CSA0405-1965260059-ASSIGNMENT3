##  Complete C Program

```c
#include <stdio.h>

#define PAGE_SIZE 1024
#define PAGES 8

int page_table[PAGES] = {5, 2, -1, 7, 0, -1, 3, -1};

int main()
{
    unsigned int logical[] = {0, 1500, 3100, 6200, 9000, 2049};

    for (int i = 0; i < 6; i++)
    {
        unsigned int page = logical[i] / PAGE_SIZE;
        unsigned int offset = logical[i] % PAGE_SIZE;

        if (page >= PAGES || page_table[page] == -1)
        {
            printf("%u -> invalid address\n", logical[i]);
        }
        else
        {
            unsigned int physical =
                page_table[page] * PAGE_SIZE + offset;

            printf("%u -> page %u offset %u : physical %u\n",
                   logical[i], page, offset, physical);
        }
    }

    return 0;
}
```

### Compilation

```bash
gcc paging.c -o paging
```

### Execution

```bash
./paging
```

### Sample Output

```text
0 -> page 0 offset 0 : physical 5120
1500 -> page 1 offset 476 : physical 2524
3100 -> invalid address
6200 -> page 6 offset 56 : physical 3128
9000 -> invalid address
2049 -> page 2 offset 1 : invalid address
```

---
