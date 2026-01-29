# Basic Linux Documentation (Notes)

*This document provides an overview of essential Linux commands, filesystem hierarchy, permissions, networking, processes, and basic shell scripting. 
It is intended for developers and new Linux users to understand and navigate Linux systems effectively*  

<br>

Below are the overall topics to be covered in this `linux commands` documentation.

| Topic        | Subtopic                             |
| ------------ | ------------------------------------ |
| Linux        | Linux Intro, Commands, FS Hierarchy  |
|              | File & Directory operations          |
|              | Permissions & ownership              |
|              | Users & groups                       |
|              | Processes & system resources         |
|              | Package management & services        |
|              | Networking commands & diagnostics    |
|              | Logs & troubleshooting               |
|              | Shell scripting basics               |

<br>

## Linux OS Timeline

| Year          | Invention                            |
| ------------- | ------------------------------------ |
| 1969          | UNIX                                 |
| 1971          | Thompson shell                       |
| 1977          | Bourne shell (sh / bash)             |
| 1983          | GNU project                          |
| 1985          | FSF + GPL                            |
| 1989          | Bash                                 |
| 1991          | Linux kernel                         |

<br>

## 1. Unix (1969)
* Created at AT&T Bell Labs
* Proprietary licensing → source unavailable to most users
* costlier source to most of the users
* Set the design model: multiuser, multitasking, small composable tools

<br>

## 2. Shell History

2.1 `Thompson Shell (1971)`
* First UNIX shell

2.2 `Bourne Shell — sh (1977)`
* Written by Stephen Bourne
* Introduced scripting syntax still used today
* Became POSIX shell standard

2.3 `Korn Shell — ksh (1983)`
* Added job control, arithmetic, arrays

2.4 `Bash — Bourne Again Shell (1989)`
* GNU replacement for `sh`
* Superset of Bourne shell
* Default shell on most Linux systems

<br>

## 3. `GNU Project (1983)`

* The full form of GNU is `"GNU's Not Unix"`
* This recursive acronym signifies that while GNU is similar to Unix, it is not Unix
* The GNU operating system is a complete free software system that is upward-compatible with Unix
* It was initiated by `Richard Stallman` in September 1983 with the goal of creating a fully free and open-source operating system
* The GNU project includes a variety of software tools such as the `GNU Compiler Collection` (GCC), the `GNU Debugger` (GDB), and the `GNU C Library` (glibc), among others
* The philosophy of GNU is that the user must be free to *run*, *study*, *modify*, *share software*, etc.
* What GNU delivered ?

1. `Development Tools`:
    - `GCC` — GNU Compiler Collection (C, C++, etc.)
    - `GDB` — GNU Debugger
    - `Binutils` — assembler (as), linker (ld), object tools (objdump, nm)
    - `Make` — build automation tool
    - `Autotools` — autoconf, automake, libtool

2. `Core utilities` (GNU Coreutils)
    - File ops: `ls`, `cp`, `mv`, `rm`, `ln`, `stat`
    - Text ops: `cat`, `head`, `tail`, `cut`, `sort`, `uniq`, `wc`
    - Search/filter: `grep`, `find`, `xargs`
    - System info: `df`, `du`, `uptime`, `who`, `uname`

3. `Libraries`
     - glibc — GNU C Library (standard C/POSIX API layer)

4. `Shells`
    - `Bash` — Bourne Again Shell (default Linux shell)
    - Also: `dash`, zsh not GNU, but Bash is GNU’s

5. `Build system tools`
   - `make`
   - `autoconf`
   - `automake`
   - `libtool`
   - `pkg-config`

6. `Other critical system software`
   - `sed`, `awk` (gawk) — stream/text processing
   - `tar`, `gzip`, `bzip2`, `xz` — archiving/compression
   - `diffutils`, `patch` — source control utilities
   - inetutils, core networking tools
   - `gettext` — localization framework

* GNU delivered useful compiler, dev tools, utilities, libraries, shell, but it is missing the `kernel`
* GNU created most of the OS, but its own `Kernel` (GNU Hurd) remains incomplete, which will be later shared by Linux

<br>

## 4. `Free Software Foundation (FSF) (1985)`

* Organization founded by `Richard Stallman` to fund and protect GNU
* FSF enforces the software freedom
* Created the GNU `General Public License` (GPL):
  - Code must remain free
  - Derivatives must also be GPL

<br>

## 5. `Linux Kernel (1991) — Linus Torvalds`

* Wrote a `UNIX-like kernel` for his personal use
* Inspired by `MINIX` (Minimal Unix - a teaching UNIX like OS, written by American-Dutch computer scientist Andrew S. Tanenbaum)
* Released `Linux` under GPL (General Public License)
* Later became — *Linux kernel* + *GNU userland* = `complete OS`

<br>

> [!NOTE]
> So what Linux actually is ? <br>
> *Linux — Kernel only* <br>
> *GNU — Tools & Utilities* <br>
> *together — `GNU/Linux OS` a.k.a. `Linux OS`*


- ![#f03c15](https://placehold.co/15x15/f03c15/f03c15.png) `#f03c15`
- ![#c5f015](https://placehold.co/15x15/c5f015/c5f015.png) `#c5f015`
- ![#1589F0](https://placehold.co/15x15/1589F0/1589F0.png) `#1589F0`

```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```




