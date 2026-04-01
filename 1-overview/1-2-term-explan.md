# 1.2 Terminology

The terms used in this manual are defined in the table below.

<mark style="color:green;">Table 1-1 Multitask terminology</mark>

| Term | Description |
| --- | --- |
| Program (job file) | - A job program stored in the controller's non-volatile memory (e.g., 0001.job, 0002.job, 1001.job, etc.). |
| Main task<br>Sub task 1 ~ 7 | -The robot controller's program executor that can load and run job programs.<br>-There are 8 tasks in total; each task can load and run only one program at a time. |
| Main task program<br>Sub task program | -The specific job program assigned to a task.<br>(Example: If the main task loads 0001.job, the main task program is 0001.job.)<br>-A program can be executed only when it is assigned to either the main task or a sub task. |
