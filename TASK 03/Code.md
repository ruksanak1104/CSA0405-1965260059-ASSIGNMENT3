##  Complete C Program

```c
#include <stdio.h>

int main()
{
    int base[] = {1000, 4000, 7000, 9000};
    int limit[] = {500, 800, 300, 600};

    int segment, offset;

    printf("Enter segment number (0-3): ");
    scanf("%d", &segment);

    printf("Enter offset: ");
    scanf("%d", &offset);

    if (segment < 0 || segment >= 4)
    {
        printf("Invalid segment number\n");
    }
    else if (offset < 0 || offset >= limit[segment])
    {
        printf("Segmentation fault: Invalid offset\n");
    }
    else
    {
        printf("Physical address = %d\n",
               base[segment] + offset);
    }

    return 0;
}
```

### Compilation

```bash
gcc segmentation.c -o segmentation
```

### Execution

```bash
./segmentation
```

### Sample Output

```text
Enter segment number (0-3): 1
Enter offset: 200
Physical address = 4200
```

---
