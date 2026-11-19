##  Runtime Loading Using `dlopen()` and `dlsym()`

### Program

```c
#include <stdio.h>
#include <dlfcn.h>

int main()
{
    void *handle;
    void (*encrypt)();

    handle = dlopen("./libencryption.so", RTLD_LAZY);

    if (handle == NULL)
    {
        printf("Library loading failed\n");
        return 1;
    }

    encrypt = (void (*)())dlsym(handle, "encrypt");

    if (encrypt == NULL)
    {
        printf("Function not found\n");
        dlclose(handle);
        return 1;
    }

    encrypt();

    dlclose(handle);
    return 0;
}
```

### Compilation

```bash
gcc runtime.c -o runtime -ldl
```

### Execution

```bash
./runtime
```

