# 2.1.1 `task start`

`task start` 语句创建一个子任务，分配一个特定的作业程序给它，并启动子任务程序。

`task start` 语句可以从 `[Command Input] - [Other] - [Task]` 输入。

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
      <td style="text-align:left">指定要创建的子任务编号 (0 ~ 7)。<br>(如果设为 0，则会自动选择一个未使用的任务并使用。)</td>
    </tr>
    <tr>
      <td style="text-align:left">程序编号</td>
      <td style="text-align:left">指定在创建的子任务中运行的程序 (1 ~ 9999)。</td>
    </tr>
    <tr>
      <td style="text-align:left">使用示例</td>
      <td style="text-align:left">task start,sub=1,job=11 (在子任务 1 中分配并运行 0011.job)<br>task start,sub=0,job=11 (自动选择一个子任务并运行 0011.job)</td>
    </tr>
  </tbody>
</table>

![Figure 2-1 使用 task start 的示例](<../../_assets/image_5.png>)

![Figure 2-2 子任务创建和等待终止的示例](<../../_assets/image_7.png>)

注意：要创建的子任务编号必须与调用任务的自身编号不同。此外，`task start` 不能在以下描述的某些错误条件下应用。

如果尝试在已经创建并运行的子任务上使用 `task start` 创建子任务，并分配另一个程序给该子任务，将导致错误。请参见下面的示例。

* <mark style="color:green;">`在运行的子任务上分配和启动另一个程序时出错`</mark>

```
task start,sub=1,job=11 # 子任务 1 已启动
task start,sub=1,job=12
...
```