- Shell
	- An application program that runs programs on behalf of the user
	- e.g.
		- sh: original Unix shell
		- csh/tcsh: BSD Unix C shell
		- bash: Bourne-Again shell
	- Simple Shell Implementation
		- Basic loop
			```c
			int main(int argc, char** argv){
				/* command line */
				char cmdline[MAXLINE];
				while (1){
					/* read */
					printf("> ");
					fgets(cmdline, MAXLINE, stdin);
					if(feof(stdin)) exit(0);
					
					/* eval */
					eval(cmdline);
				}
				```
			- Read line from command line
			- Execute the requested operation (Built-in command, Load and execute program from a file)
			- Execution is a sequence of read & evaluation
		- eval
			```c
			void eval(char *cmdline){
				char *argv; /* Argument linst execuve() */
				char buf[MAXLINE]; /* Holds modified command line */
				int bg; /* Should the job run in bg or fg? */
				pid_t pid /* Process id */
				
				strcpy(buf, cmdline);
				bg = parseline(buf, argv); // returns if input line ended in '&'
				if(argv[0] == NULL) return; /* Ignore empty lines */
				
				if(!builtin_command (argv)){
					if((pid = Fork()) == 0){ /* Child runs user job */
						if(execve(argv[0], argv, environ) < 0){
							printf("%s: Command not found.\n", argv[0]);
							exit(0);
						}
					}
				
					if(!bg){
						int status;
						if(waitpid(pid, &status, 0) < 0)
							unix_error("waitfg: waitpid error");
					}
					else printf("%d %s", pid, cmdline);
				}
				return;
			}
```
			- Is everything is okay? No, bg를 reap해야 함
	- What is a "Background Job"?
		- Users generally run one command at a time
		- Some programs run for a long time
		- A background job is a process we do not want to wait for
	- Problem with the Simple Shell Example
		- The shell program is designed to run indefinitely (i.e. until the user input is `quit`)
			- Should not accumulate resources unnecessarily
		- Our example shell correctly waits for and reaps foreground jobs
		- __BUT__ What about background jobs?
			- Will become zombies when they terminate
			- Will never be reaped because shell (typically) will not terminate
			- Will create a memory leak that could run the kernel out of memory
	- Solution: exceptional control flow
		- The kernel interrupts regular processing to alert the shell (parent) when a background process (child) completes
		- Such an alert mechanism is called a signal in UNIX

- Signal
	- A small message that notifies a process of a system event
		- Akin to exceptions and interrupts
		- Sent from the kernel to a process (sometimes at the request from another process)
		- Signal type is identified by small integer ID's (1~30)
		- The only information in a signal is its ID (and its arrival for sure)
	- Signal Concepts
		- Sending a Signal
			- Kernel sends (delivers) a signal to a destination process by updating some state in the context of the destination process
			- Kernel sends a signal for one of the following reasons
				- Kernel has detected a system event such as divide-by-zero(`SIGFPE`) or the termination of a child process (`SIGCHLD`)
				- Another process has invoked the `kill` system call to explicitly request the kernel to send a signal to the destination process
		- Receiving a Signal
			- A destination process receives a signal when it is forced by the kernel to react in some way to the delivery of the signal (Context switching에 의하여 해당 process가 불릴 때 강제적으로 받게 한다)
			- Three possible ways to react
				- Ignore the signal (do nothing)
				- Terminate the process (with optional core dump)
				- Catch the signal by executing a user-level function called signal handler
		- Pending and Blocked Signal
			- A signal is pending if sent but not yet received
				- There can be at most one pending signal of any particular type
				- Important: signals not queued; if a process has a pending signal of k, then subsequent type-k signals will be discarded (Signal은 왔다 또는 안왔다 만 표시할 수 있다)
				- A pending signal is received at most once
			- A process can block the receipt of certain signals
				- A blocked signal can be delivered, but will not be received until unblocked
				- Signal이 block되면 receive되지 않고, pending 상태로 남아있음
			- Kernel keeps bit vectors `pending/blocked` in the context of each process
				- Represent the sets of pending/blocked signals
				- Kernel sets (clears) bit k in pending when a signal of type k is delivered (received)
				- A process can (un)block a signal of type k by setting (clearing) bit k in blocked (also referred to as the signal mask) using the `sigprocmask` function
	- Sending Signal
		- Process Group
			- Every process belongs to exactly one process group
			- `setpgid()`: changes the process group of a process
			- `getpgrp()`: returns the current process group
			- Convention
				- exec $\rightarrow$ new group ID
				- fork $\rightarrow$ same group ID
		- Sending Signals with Keyboard
			- CTRL-C(CTRL-Z) sends a SIGINT(SIGTSTP) to every job in the foreground process group; default action is to terminate(suspend) each process
	- Receiving Signal
		- Suppose kernel is returning from an exception handler and is ready to pass control to process _p_
		- Kernel computes `pnb = pending & ~blocked` (i.e. the set of pending non-blocked signal for process _p_)
		- If (`pnb == 0`): pass control to the next instruction in the logical flow for _p_
		- Otherwise
			- Choose the nonzero LSB _k_ in `pnb` and force process _p_ to receive signal _k_
			- The receipt of the signal triggers the corresponding action by _p_
			- Repeat for all nonzero bits in `pnb`
			- Pass control to the next instruction in logical flow for _p_ 
	- Installing Signal Handlers
		- The `signal` function modifies the default action for signal `signum`
			- `handler_t *signal(int signum, handler_t *handler)`
		- A signal handler is a separate logical flow (not process)
			- Runs concurrently with the main program
			- Exists only until returns to the main program
		- Nested Signal Handler
			- Handlers can be interrupted by other handlers
	- Blocking and Unblocking Signals
		- Implicit blocking mechanism
			- Kernel blocks any pending signals of the type currently being handled
		- Explicit blocking and unblocking mechanism: `sigprocmask` function
		- Supporting functions
			- `sigemptyset`: create an empty set
			- `sigfillset`: add every signal number to set
			- `sigaddset`: add signal number to set
			- `sigdelset`: delete signal number from set
	- Safe Signal Handling
		- Handlers are tricky because they are concurrent with main program and share the same global data structures
			- Shared data structures can become corrupted
		- Async-Signal-Safety
			- A function is async-signal-safe if it meets either of the following conditions
				- all variables stored on stack frame (= Reentrant)
				- Non-interruptible by signals
	- Correct Signal Handling
		- Pending signals are not queued
			- Only one bit in the pending bit vector for each signal type, At most one pending signal of any particular type
			- You cannot use signals to count events, such as children terminating
		- Must call wait for all terminated child processes
			- Put wait in a loop to reap all terminated children