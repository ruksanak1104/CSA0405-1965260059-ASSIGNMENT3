##  Stage 3 – Implement and Verify 

### Complete C Program

This program allocates and locks the control-task memory, measures a critical loop, and simulates memory-frame allocation for the three tasks.

```c
#define _POSIX_C_SOURCE 200809L

#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <sys/mman.h>
#include <time.h>

#define CONTROL_SIZE (128 * 1024 * 1024)
#define FRAMES 8

double getTime()
{
    struct timespec t;

    clock_gettime(CLOCK_MONOTONIC, &t);

    return t.tv_sec + t.tv_nsec / 1000000000.0;
}

int main()
{
    char *control;
    int frames[FRAMES];
    int tasks[] = {1, 2, 3};

    control = malloc(CONTROL_SIZE);

    if (control == NULL)
    {
        printf("Memory allocation failed\n");
        return 1;
    }

    memset(control, 0, CONTROL_SIZE);

    if (mlock(control, CONTROL_SIZE) == -1)
    {
        perror("mlock failed");
        free(control);
        return 1;
    }

    printf("Control memory allocated: 128 MB\n");
    printf("Control memory locked successfully\n");

    for (int i = 0; i < FRAMES; i++)
        frames[i] = -1;

    for (int i = 0; i < 3; i++)
    {
        frames[i] = tasks[i];
    }

    printf("\nMemory frame allocation:\n");

    for (int i = 0; i < FRAMES; i++)
    {
        if (frames[i] == -1)
            printf("Frame %d: Free\n", i);
        else
            printf("Frame %d: Task %d\n", i, frames[i]);
    }

    double start = getTime();

    volatile int count = 0;

    for (int i = 0; i < 10000000; i++)
        count++;

    double end = getTime();

    printf("\nControl loop completed\n");
    printf("Execution time: %.6f seconds\n", end - start);
    printf("Counter: %d\n", count);

    munlock(control, CONTROL_SIZE);
    free(control);

    printf("Memory unlocked and released\n");

    return 0;
}
```

### Compilation

```bash
gcc -std=c11 -Wall -Wextra q7.c -o q7
```

### Execution

```bash
./q7
```
