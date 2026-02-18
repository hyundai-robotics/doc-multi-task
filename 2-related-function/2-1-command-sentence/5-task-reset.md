# 2.1.5 `task reset`

`task reset`语句强制销毁一个子任务。通常，当子任务程序中执行`end`语句时，子任务会自动被销毁。

```
task reset,sub=<sub_task_number>,job=<program_number>
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
      <td style="text-align:left">指定要销毁的子任务编号 (0 ~ 7)。<br>(如果设置为0，控制器将找到运行指定程序编号的任务并销毁该任务。)</td>
    </tr>
    <tr>
      <td style="text-align:left">程序编号</td>
      <td style="text-align:left">当子任务编号指定为0时使用。指定正在运行的程序 (1 ~ 9999)。</td>
    </tr>
    <tr>
      <td style="text-align:left">用法示例</td>
      <td style="text-align:left">task reset,sub=1 (销毁子任务 1)<br>task reset,sub=0,job=11 (销毁运行程序 11 的任务)</td>
    </tr>
  </tbody>
</table>