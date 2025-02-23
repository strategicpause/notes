## Terms
* **BPF_HASH** - Creates a hash map with a given name.
* **BPF_MAP_TYPE_PROG_ARRAY** - An array whose map values contain only file descriptors referring to other eBPF programs. Both the `key_size` and `value_size` must be exactly four bytes. This kind of map is used in conjunction with `bpf_tail_call`.
* **BPF_PROG_ARRAY** - A program array where each entry is either a file descriptor to a BPF program or `NULL`. The array acts as a jump table so that bpf programs can "tail-call" other BPF programs.

## Syscalls
* **execve** - Executes a program referred to by a given pathname. The program is loaded into the current process's memory space, replaces the process's memory image with the new program, and starts its execution from the entry point of the new program.
* 
## Tracepoints
Tracepoints are static instrumentation points in the kernel code that allow developers to trace execution flow of various kernel functions and system calls without modifying the kernel source code.
* **sys_enter** - Represents the entry point of a system call.This is called just before the kernel starts executing the syscall which allows tracing and analyze sytem call parameters, execution time, and other information.

## Macros
* **RAW_TRACEPOINT_PROBE** - Instruments the raw tracepoint defined by a given event.
* **SEC** - Used to place programs, maps, license in different sections in elf_bpf files. Parsed by bpf loaders such as libbpf parse.
* 