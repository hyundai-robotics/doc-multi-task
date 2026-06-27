# 2.1.4 `task stop`

The `task stop` statement forcibly stops execution of a sub task.

```
task stop,sub=<sub_task_number>,job=<program_number>
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
      <td style="text-align:left">指定要停止的子任务编号 (0 ~ 7).<br>(如果设置为 0，控制器会找到运行指定程序编号的任务并停止该任务.)</td>
    </tr>
    <tr>
      <td style="text-align:left">程序编号</td>
      <td style="text-align:left">当子任务编号指定为 0 时使用。指定正在运行的程序 (1 ~ 9999).</td>
    </tr>
    <tr>
      <td style="text-align:left">用法示例</td>
      <td style="text-align:left">task stop,sub=1 (停止执行子任务 1)<br>task stop,sub=0,job=11 (停止执行运行程序 11 的任务)</td>
    </tr>
  </tbody>
</table>