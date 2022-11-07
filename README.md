
# CS313fa22 -- Assignment 6


Name: Brandon Michaud

Bonus Completed: Yes


## Description of how I modified and compiled my code.
I modified the provided thread_incr_psem.c file to have an array of pthread_t's instead of just two, like the original. The size of the array is determined by the third command line argument (where `./a.out` is the first). Then, for each pthread_t in the array, I call `pthread_create` and check for an error. Finally, I call `pthread_join` for each pthread_t in the array and check for an error. Once I made those changes, I compiled the code using `gcc thread_incr_psem.c` which produced an executable called a.out. Next, I made a folder called imgs and pointed the two save commands in the runtest.bash file to that folder. Finally, I ran the experiment using ``bash runtest.bash ./a.out experiment`date +%Y%m%d%M`.csv`` on 4 virtual machines with 2, 4, 8, and 16 cores, respectively.

## Set of all the assumptions made to run the code.
The code operates under the assumption that when a.out is ran, it is given 2 additional command line arguments, e.g. `./a.out x y` where x is the number of loops and y is the number of threads. The executable will not work if it is not ran in this exact format. The runtest.bash file also assumes that the user has required packages installed. For a list of required packages, see `https://oudatalab.com/cs3113fa22/assignments/6/#submission`.

## Any bugs or corner cases I have handled.
When the code runs, depending on the machine, it may try to count to a number that is larger than the maximum allowable integer defined by the hardware, which will result in irregular behavior. This does not have any impact on the time it takes for the threads to complete counting, so it has no impact on the experiment results. If it bothers you, however, you can upgrade to a better virtual machine or set a maximum number of loops and/or threads.

## Definition of semaphores.
A semaphore is an integer value used for signaling among processes. There are three atomic operations that may be performed on a semaphore: initialize, increment, and decrement. Decrement is used to block a process, and increment is used to unblock a process.

## Definitions of "real time", "kernel time", "user time".
- Real time is the amount of cumulative time that passes between when a process begins running and when it terminates.
- Kernel time is the amount of real time that is spent in kernel mode.
- User time is the amount of real time that is spent in user mode.

## Include plots from the results.
![2-Core Plot](imgs/experiment2022110604.csv_rt.png)
![4-Core Plot](imgs/experiment2022110612.csv_rt.png)
![8-Core Plot](imgs/experiment2022110613.csv_rt.png)
![16-Core Plot](imgs/experiment2022110625.csv_rt.png)

## Descriptions of trends of the plots.
For each experiment, it is clear from the above graphs that as the number of loops increases, the time to complete the process also increases in a linear fashion, regardless of the number of threads. Additionally, as the number of threads increases, the time it takes to complete the process also increases. Interestingly, the time decreased between the experiments with 2 cores and 4 cores, but increased between 4 cores and 8 cores, before decreasing again between 8 cores and 16 cores. Taken altogether, there seems to be an average negative trend between time and the number of cores, but there also seems to be some amount of noise/randomness to the results for an individual experiment.

## Explanation of the trend results.
The time increases linearly with the number of loops because, assuming each iteration in a loop takes the same amount of time, when you double the number of loops, you double the time. This result is independent of the number of threads. When the number of threads is increased, the time is also increased because the threads have to coordinate with each other using semaphores to ensure protection of shared memory, which takes additional time and scales linearly. By increasing the number of cores, we should expect to see that the time it takes to complete the process is decreased because we can divide the threads across the new cores. This is the general trend from the experiments, but there is some fluctuation. This could be the result of many things. One possibility is the cores were also busy executing other threads not from the experiment. It could also be that some of the cores were better/faster than others. Further testing is needed to develop an average time across experiments of the same number of cores. 
