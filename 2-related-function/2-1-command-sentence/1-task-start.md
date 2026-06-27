# 2.1.1 `task start`

The `task start` statement creates a sub task, assigns a specific job program to it, and starts the sub task program.

The `task start` statement can be entered from `[Command Input] - [Other] - [Task]`.

```
task start,sub=<sub_task_number>,job=<program_number>
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">项目</th>
      <th style="text-align:left">描述</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">子任务编号</td>
      <td style="text-align:left">指定要创建的子任务编号 (0 ~ 7)。<br>(如果设置为 0，将自动选择未使用的任务并使用。)</td>
    </tr>
    <tr>
      <td style="text-align:left">程序编号</td>
      <td style="text-align:left">指定在创建的子任务中运行的程序 (1 ~ 9999)。</td>
    </tr>
    <tr>
      <td style="text-align:left">用法示例</td>
      <td style="text-align:left">task start,sub=1,job=11 (在子任务 1 上分配并运行 0011.job)<br>task start,sub=0,job=11 (自动选择一个子任务并运行 0011.job)</td>
    </tr>
  </tbody>
</table>

![Figure 2-1 Example of using task start](<../../_assets/image_5.png>)

![Figure 2-2 Example of sub task creation and wait for termination](<../../_assets/image_7.png>)

Note: The sub task number to be created must be a different sub task number than the calling task's own number. Also, `task start` cannot be applied in certain error conditions described below.

If you attempt to create a sub task with `task start` when that sub task is already created and running, assigning another program to that sub task will result in an error. See the example below.

* <mark style="color:green;">`在运行中的子任务上分配和启动另一个程序时出错`</mark>

```
task start,sub=1,job=11 # subtask 1 was started
task start,sub=1,job=12
...
```