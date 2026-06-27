# 2.1.3 `task sync`

`task sync`语句用于同步任务。当两个或多个机器人必须合作时，同步是必不可少的；`task sync`有助于在任务之间对齐起始点。当主任务和子任务需要在特定点同时执行工作并开始一个步骤时，它非常方便。

```
task sync,id=<identifier>,no=<number_of_tasks_to_sync>
```

| **项目** | **描述** |
| :------: | --------------- |
| **标识符** | 指定一个从1到32的标识符。可以在程序中设置多个同步点，标识符用于区分它们。 |
| **同步的任务数量** | 指定要同步的任务数量（2 ~ 8）。发起任务会等待具有相同标识符的执行了`task sync`的任务数量匹配该值。 |
| **使用示例** | <p># 主任务程序</p><p>print "maintask"</p><p>task start,sub=1,job=11</p><p>...</p><p><mark style="background-color:green;">task sync,id=1,no=2</mark></p><p>print "与子任务1同步"</p><p>(id=1, 同步的任务数量 = 2)</p> |
| | <p># 子任务1程序</p><p>print "subtask 1"</p><p>...</p><p><mark style="background-color:green;">task sync,id=1,no=2</mark></p><p>print "与主任务同步"</p><p>(id=1, 同步的任务数量 = 2)</p> |