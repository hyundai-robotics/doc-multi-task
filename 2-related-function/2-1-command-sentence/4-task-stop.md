# 2.1.4 task stop

The `task stop` statement forcibly stops execution of a sub task.

```
task stop,sub=<sub_task_number>,job=<program_number>
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
      <td style="text-align:left">Specify the sub task number to stop (0 ~ 7).<br>(If set to 0, the controller finds the task running the specified program number and stops that task.)</td>
    </tr>
    <tr>
      <td style="text-align:left">Program number</td>
      <td style="text-align:left">Used when the sub task number is specified as 0. Specifies the running program (1 ~ 9999).</td>
    </tr>
    <tr>
      <td style="text-align:left">Usage examples</td>
      <td style="text-align:left">task stop,sub=1 (stop execution of sub task 1)<br>task stop,sub=0,job=11 (stop execution of the task running program 11)</td>
    </tr>
  </tbody>
</table>
