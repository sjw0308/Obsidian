- Latency
	- Time between start and finish of single task
	- Most applicable in interactive applications
- Throughput
	- Number of tasks finished in a given unit of time
	- Most applicable in batch applications
- Throughput is NOT always 1/latency when concurrency is involved

- Performance
	- = 1/Time
	- Shorter latency
	- Higher throughput
- UNIX "time" command
	- Elapsed time (wall-clock time)
	- User CPU time (time spent running your code)
	- System CPU time (time spent running other code on behalf of your code)
	- Elapsed time - (user CPU time + system CPU time) (time running other code unrelated to your code)

- CPU clocking
	- Operation of digital hardware governed by a constant-rate clock
	- Clock period: duration of a clock cycle (sec)
	- Clock frequency(rate): cycles per second (Hz)

- IPC
	- instructions per cycle (inst / cycle)
	- Avg. IPC = 1 / Arithmetic mean of CPI = Harmonic mean of IPC
- CPI
	- cycle per instructions (cycle / inst)
- MIPS
	- million instructions per second (inst / 1million * second)
- GHz
	- $10^9$ cycles per second
- Iron law on performance
	- wall clock time = (time / cycle) * (cycle / inst) * (inst / program)

- FLOPS
	- Nominal number of floating-point operations / program runtime

- X is n times faster than Y means
	- n = ${Performance}_X /{Performance}_Y$ = ${Throughput}_X / {Throughput}_Y$ = ${Time}_Y / {Time}_X$ 
- X is m% faster than Y means
	- 1+m/100 = ${Performance}_X / {Performance}_Y$ 

- Speedup
	- If X is an "enhanced" version of Y, the "speedup" of the enhancement is 
	- Speedup = $Performance_{new} / Performance_{old}$ = $Throughput_{new} / Throughput_{old}$ = $Time_{old} / Time_{new}$ 
	- Amdahl's law on speedup0
		- Suppose an enhancement speeds up a fraction f of a task by a factor of $S_f$
		- $time_{old} = time_{old} * ((1-f) + f)$
		- $time_{new} = time_{old} * ((1-f) + f/S_f)$
		- $S_{overall} = time_{old} / time_{new} = 1/((1-f) + \frac{f}{S_f})$ 
