# 2.1.1 task start

The `task start` statement creates a sub task, assigns a specific job program to it, and starts the sub task program.

The `task start` statement can be entered from **Command Input** → **Other** → **Task**.

```
task start,sub=<sub_task_number>,job=<program_number>
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">Item</th>
      <th style="text-align:left">Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">Sub task number</td>
      <td style="text-align:left">Specify the sub task number to create (0 ~ 7).<br>(If set to 0, an unused task is selected automatically and used.)</td>
    </tr>
    <tr>
      <td style="text-align:left">Program number</td>
      <td style="text-align:left">Specify the program to run in the created sub task (1 ~ 9999).</td>
    </tr>
    <tr>
      <td style="text-align:left">Usage examples</td>
      <td style="text-align:left">task start,sub=1,job=11 (assign and run 0011.job on sub task 1)<br>task start,sub=0,job=11 (automatically select a sub task and run 0011.job)</td>
    </tr>
  </tbody>
</table>

![Figure 2‑1 Example of using task start](<../../_assets/image_5.png>)

![Figure 2‑2 Example of sub task creation and wait for termination](<../../_assets/image_7.png>)

Note: The sub task number to be created must be a different sub task number than the calling task's own number. Also, `task start` cannot be applied in certain error conditions described below.

If you attempt to create a sub task with `task start` when that sub task is already created and running, assigning another program to that sub task will result in an error. See the example below.

* <mark style="color:green;">**Error when assigning and starting another program on a running sub task**</mark>

```
task start,sub=1,job=11 # subtask 1 was started
task start,sub=1,job=12
…
```
