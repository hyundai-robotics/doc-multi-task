# 2.1.2 `task wait`

The `task wait`语句等待一个子任务被销毁。通常，当执行该子任务程序中的`end`语句时，子任务会自动被销毁。当您想等待另一个子任务完成后再继续工作时，请使用此命令。

```
task wait,sub=<sub_task_number>,job=<program_number>
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
      <td style="text-align:left">指定要等待的子任务编号 (0 ~ 7)。<br>(如果设置为 0，控制器会找到运行指定程序编号的任务并等待该任务被销毁。)</td>
    </tr>
    <tr>
      <td style="text-align:left">程序编号</td>
      <td style="text-align:left">当子任务编号指定为 0 时使用。指定正在运行的程序 (1 ~ 9999)。</td>
    </tr>
    <tr>
      <td style="text-align:left">用法示例</td>
      <td style="text-align:left">task wait,sub=1 (等待子任务 1 被销毁)<br>task wait,sub=0,job=11 (等待运行程序 11 的任务被销毁)</td>
    </tr>
  </tbody>
</table>