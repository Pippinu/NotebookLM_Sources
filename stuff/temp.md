### LAS Numerical Examples

To better understand how the Least Attained Service (LAS) policy works in practice, we can look at two complementary examples: a step-by-step trace and a graph of its general performance.

#### Example 1: A Step-by-Step Trace (Slide 179)

This example traces the execution of three jobs with different service times arriving at different moments, showing how LAS dynamically changes which job is being served.

  * **Scenario:**
      * Job 3 (service time = 2) arrives at time 12.
      * Job 2 (service time = 8) arrives at time 24.
      * Job 1 (service time = 4) arrives at time 48.
  * **Execution Trace:**
    1.  At time 12, Job 3 arrives and starts running.
    2.  At time 24, Job 2 arrives. Since Job 2 has received **zero** service so far, it has the "least attained service," and the server preempts Job 3 to start working on Job 2.
    3.  At time 48, Job 1 arrives. It also has zero attained service, so it has priority. The server preempts Job 2 and starts working on Job 1.
    4.  The server will continue to work on whichever active job has the least accumulated service time, switching between them until they are all complete.
  * **Result vs. FCFS:**
      * **With LAS:** The completion times are $\large S_1=14$, $\large S_2=8$, $\large S_3=2$. The policy manages to finish the two smaller jobs (2 and 3) very quickly.
      * **With FCFS:** The completion times would have been $\large S_1=8$, $\large S_2=10$, $\large S_3=10$. The jobs would be served strictly in their arrival order.

This trace clearly shows how LAS reorders execution to prioritize jobs that are presumed to be short, even if they arrive later.

#### Example 2: General Performance Characteristics (Slide 180)

This graph illustrates the general performance of LAS by plotting the conditional response time as a function of the job's size, for different system loads ($\large \rho$).

  * **X-axis:** The normalized service demand ($\large x/E[X]$), representing how large a job is compared to the average job size.
  * **Y-axis:** The conditional response time ($\large R(x)/E[X]$), representing how long a job of a certain size takes to complete, normalized by the average service time.
  * **Interpretation:**
      * **Favors Short Jobs:** For small job sizes (left side of the graph), the response time is very low. LAS is extremely efficient for short tasks.
      * **Penalizes Long Jobs:** For large job sizes (right side of the graph), the response time grows dramatically. LAS will constantly preempt long jobs to service newly arriving short jobs, so large jobs can take a very long time to complete.
      * **Impact of Load:** This effect becomes much more extreme as the system load ($\large \rho$) increases. At high loads (e.g., $\large \rho=0.8$), the penalty for long jobs is severe.

**Conclusion from both examples:** Together, these examples demonstrate both the *mechanism* of LAS (always prioritizing the job with the least completed work) and its *performance consequence* (excellent response times for short jobs at the expense of very long delays for large ones).