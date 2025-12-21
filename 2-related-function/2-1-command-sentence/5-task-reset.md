# 2.1.5 task reset

The `task reset` statement forcibly destroys a sub task. Normally a sub task is destroyed automatically when an `end` statement in that sub task program is executed.

```
task reset,sub=<sub_task_number>,job=<program_number>
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
      <td style="text-align:left">Specify the sub task number to destroy (0 ~ 7).<br>(If set to 0, the controller finds the task running the specified program number and destroys that task.)</td>
    </tr>
    <tr>
      <td style="text-align:left">Program number</td>
      <td style="text-align:left">Used when the sub task number is specified as 0. Specifies the running program (1 ~ 9999).</td>
    </tr>
    <tr>
      <td style="text-align:left">Usage examples</td>
      <td style="text-align:left">task reset,sub=1 (destroy sub task 1)<br>task reset,sub=0,job=11 (destroy the task running program 11)</td>
    </tr>
  </tbody>
</table>
