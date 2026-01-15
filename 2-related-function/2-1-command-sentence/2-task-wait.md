# 2.1.2 task wait

The `task wait` statement waits for a sub task to be destroyed. Normally a sub task is destroyed automatically when an `end` statement in that sub task program is executed. Use this when you want to wait for another sub task to finish before continuing work.

```
task wait,sub=<sub_task_number>,job=<program_number>
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
      <td style="text-align:left">Specify the sub task number to wait for (0 ~ 7).<br>(If set to 0, the controller finds the task running the specified program number and waits for that task to be destroyed.)</td>
    </tr>
    <tr>
      <td style="text-align:left">Program number</td>
      <td style="text-align:left">Used when the sub task number is specified as 0. Specifies the running program (1 ~ 9999).</td>
    </tr>
    <tr>
      <td style="text-align:left">Usage examples</td>
      <td style="text-align:left">task wait,sub=1 (wait for sub task 1 to be destroyed)<br>task wait,sub=0,job=11 (wait for the task running program 11 to be destroyed)</td>
    </tr>
  </tbody>
</table>
