### The Bourne shell: /bin/sh

A shell is aprogram that runs commands, like the ones that users enter. The shell also serves as a small programming environment.

Linux uses an enhanced version of the Bourne shell called **bash**:  “**B**ourne-**A**gain” **sh**ell.

## Command-Line Editing

| Keybind | What it does |
| -- | :-- |
| **ctrl-B** | Move the cursor left |
| **ctrl-F** | Move the cursor right |
| **ctrl-P** | View the previous command (or move the cursor up) |
| **ctrl-N** | View the next command (or move the cursor down) |
| **ctrl-A** | Move the cursor to the beginning of the line |
| **ctrl-E** | Move the cursor to the end of the line |
| **ctrl-W** | Erase the preceding word |
| **ctrl-U** | Erase from cursor to beginning of line |
| **ctrl-K** | Erase from cursor to end of line |
| **ctrl-Y** | Paste erased text (for example, from ctrl-U) |

----

#### standart error redirecting
- `ls /fffffffff > f 2> e`
    The number 2 specifies the stream ID that the shell modifies. Stream ID 1 is standard output (the default), and 2 is standard error.


##### Standard Input Redirection
use < as in:  `head < /proc/cpuinfo`

## Listing and Manipulating Processes
Process is a running program. Each process on the system has a numeric process ID (PID).

### Listing
**ps x**   Show all of your running processes.
**ps ax** Show all processes on the system, not just the ones you own.
**ps u** Include more detailed information on processes.
**ps w** Show full command names, not just what fits on one line.


### Killing Processes

```bash
# killing
kill pid
# stop signal
kill -STOP pid
# continue signal
kill -CONT pid
# temporary stop signal
kill -TSTP pid
```


- Using ``ctrl-c`` to terminate a process that is running in the current terminal is the same as using kill to end the process with the INT (interrupt) signal.
### Job Control


You can send a TSTP signal with `ctrl-Z`, then start the process again by entering ``fg`` (bring to foreground) or ``bg`` (move to background).



But despite its utility and the habits of many experienced users, job control is not necessary and can be confusing for beginners: It’s common for users to press ``ctrl-Z`` instead of ``ctrl-C``, forget about what they were running, and eventually end up with **numerous suspended processes hanging around.**


### Background Processes
Normally, when you run a Unix command from the shell, you don’t get the shell prompt back until the program finishes executing. However, you can detach a process from the shell and put it in the “background” with the ampersand (&); this gives you the prompt back.

