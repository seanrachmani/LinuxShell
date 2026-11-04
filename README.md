# LinuxShell

A lightweight Unix-style command-line shell written in **C**, built as a systems programming project to explore how Linux executes commands and manages processes.

The project implements a basic interactive shell with foreground and background execution, process tracking, command history, I/O redirection, two-command pipelines, and Unix signal handling. It uses Linux system calls such as `fork()`, `execvp()`, `waitpid()`, `pipe()`, `dup()`, and `kill()`.

## Features

- **Command execution:** Run external programs using `fork()` and `execvp()`.
- **Foreground and background processes:** Wait for foreground commands or use `&` to keep the shell available.
- **Process tracking:** View process IDs, commands, and running/suspended/terminated states with `procs`.
- **Process control:** Send signals to stop, resume, or interrupt processes.
- **I/O redirection:** Redirect standard input with `<` and standard output with `>`.
- **Pipelines:** Connect two commands with `|`, passing the first command's output to the second.
- **Command history:** Store the 10 most recent commands, list them with `history`, and recall entries with `!!` or `!n`.
- **Built-in commands:** Includes `cd`, `quit`, and process-management commands.
- **Debug mode:** Use `-d` to print extra process-execution information.

## Requirements

- Linux (including a Linux environment under WSL)
- GCC and Make
- 32-bit development support, because the included `makefile` compiles with `-m32`

On Ubuntu/Debian, you can install the build dependencies with:

```bash
sudo apt update
sudo apt install build-essential gcc-multilib libc6-dev-i386
```

## Build and Run

Clone the repository:

```bash
git clone https://github.com/seanrachmani/LinuxShell.git
cd LinuxShell
```

Build all three executables:

```bash
make
```

Launch the interactive shell:

```bash
./myshell
```

Or enable debug output:

```bash
./myshell -d
```

Remove generated executables and object files:

```bash
make clean
```

## Usage Examples

Enter these commands **inside `./myshell`**:

| Task | Example |
| --- | --- |
| Run a Linux command | `ls -l` |
| Change directory | `cd /tmp` |
| Execute in the background | `sleep 30 &` |
| List tracked processes | `procs` |
| Redirect output to a file | `ls > files.txt` |
| Read input from a file | `cat < files.txt` |
| Connect two commands | `ls -l \| grep .c` |
| Display command history | `history` |
| Repeat the previous command | `!!` |
| Repeat a numbered history entry | `!2` |
| Stop a process | `stop 1234` |
| Resume a process | `wakeup 1234` |
| Interrupt a process | `ice 1234` |
| Exit the shell | `quit` |

Replace `1234` with the actual process ID, which you can find using `procs`.

### Additional demonstration programs

The build also creates two standalone programs:

```bash
./looper
./mypipeline
```

- **`looper`** runs continuously and demonstrates handling Linux signals such as `SIGINT`, `SIGTSTP`, and `SIGCONT`.
- **`mypipeline`** demonstrates inter-process communication by connecting `ps -xl` to `grep 5` through a pipe.

## Project Structure

```text
LinuxShell/
├── myshell.c       # Interactive shell, execution, process control, history
├── LineParser.c    # Command-line parsing implementation
├── LineParser.h    # Command-line parser interface
├── looper.c        # Signal-handling demonstration
├── mypipeline.c    # Two-process pipe demonstration
└── makefile        # Build and clean targets
```

## Technical Concepts

The implementation is an exercise in core operating-system mechanisms:

- **Process creation and execution:** `fork()`, `execvp()`, `waitpid()`
- **Inter-process communication:** `pipe()`, file descriptors, and `dup()`
- **Process signals:** `kill()` and Unix signal constants
- **File operations:** `open()`, `close()`, and standard-stream redirection
- **Data structures and memory:** Linked lists for process tracking and a circular buffer for command history

## Scope and Limitations

This is an educational shell, not a full replacement for Bash. In particular, pipeline execution is implemented for **two commands**, and advanced shell features such as wildcard expansion, environment-variable expansion, and full job control are outside its scope.

---

**Author:** [Sean Rachmani](https://github.com/seanrachmani)
