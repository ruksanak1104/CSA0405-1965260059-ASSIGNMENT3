
# Static and Dynamic Linking in Linux

## 1. Aim
To compare static and dynamic linking, load a shared library at runtime, and study the memory addresses of loaded libraries.

---

## 2. Static Linking

Compile the encryption library and link it directly with the bank application.

### Commands

```bash
gcc -c encryption.c -o encryption.o
ar rcs libencryption.a encryption.o
gcc bank.c ./libencryption.a -o static_app
./static_app
```

### Explanation
- The encryption source file is compiled into an object file.
- The object file is converted into a static library.
- The static library is linked directly with the bank application.
- The required library code becomes part of the executable.

---

## 3. Dynamic Linking

Compile the encryption library as a shared library and link it dynamically.

### Commands

```bash
gcc -fPIC -c encryption.c -o encryption.o
gcc -shared -o libencryption.so encryption.o
gcc bank.c -L. -lencryption -o dynamic_app
LD_LIBRARY_PATH=. ./dynamic_app
```

### Explanation
- `-fPIC` generates Position Independent Code.
- `-shared` creates a shared library.
- `-L.` specifies the current directory as the library search path.
- `-lencryption` links the encryption library.
- `LD_LIBRARY_PATH=.` allows Linux to locate the shared library during execution.

---

## 4. Compare Executable Sizes and Libraries

### Commands

```bash
ls -lh static_app dynamic_app
ldd static_app
ldd dynamic_app
```

### Explanation
- `ls -lh` displays the sizes of the executables.
- `ldd` displays the shared libraries required by an executable.
- Static linking generally produces a larger executable.
- Dynamic linking generally produces a smaller executable.

---


### Explanation
- `dlopen()` loads the shared library at runtime.
- `dlsym()` retrieves the address of a function from the library.
- `dlclose()` unloads the library when it is no longer required.
- `RTLD_LAZY` loads symbols when they are needed.

---

## 5. Display Loaded Memory Addresses

### Command

```bash
cat /proc/self/maps
```

### Inspect Memory Map While the Program Is Running

```bash
./dynamic_app &
cat /proc/$!/maps
```

### Explanation
- `/proc/self/maps` displays the memory map of the current process.
- It shows memory regions, permissions, and mapped files.
- Shared libraries are mapped into the process memory.
- Memory addresses help understand how libraries are loaded into memory.

---

## 6. Address Binding

### Types of Address Binding

1. **Compile-Time Binding:** Addresses are determined during compilation.
2. **Load-Time Binding:** Addresses are determined when the program is loaded into memory.
3. **Execution-Time Binding:** Addresses are translated while the program is executing.

---

## 7. Linker and Loader

### Linker
- Combines object files and libraries.
- Resolves external function references.
- Creates the executable file.

### Loader
- Loads the executable into main memory.
- Loads required shared libraries.
- Prepares the program for execution.

---

## 8. Evaluation

### Static Linking vs Dynamic Linking

| Static Linking | Dynamic Linking |
|---|---|
| Larger executable | Smaller executable |
| Library code is included in the executable | Library code is loaded from a shared library |
| No external library required at runtime for the statically linked code | Shared library must be available at runtime |
| Updating the library requires relinking | Library can be updated separately |
| Uses more disk space | Reduces duplicated library code |

### Position Independent Code (PIC)

Position Independent Code allows shared library code to be loaded at different memory addresses without requiring the code to be rewritten.

---

## 9. Conclusion

Dynamic linking is recommended for the bank application because multiple branches can share a single encryption library. Updating the library becomes easier and reduces duplicated memory usage.

However, the bank must ensure that the correct shared library is installed and available at runtime.

