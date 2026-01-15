# 2.1.3 task sync

The `task sync` statement synchronizes tasks. When two or more robots must cooperate, synchronization is essential; `task sync` is useful to align the start points among tasks. It is handy when the main task and sub tasks need to perform work and then start a step simultaneously at a specific point.

```
task sync,id=<identifier>,no=<number_of_tasks_to_sync>
```

| **Item** | **Description** |
| :------: | --------------- |
| **Identifier** | Specify an identifier from 1 to 32. Multiple sync points can be set in a program and the identifier is used to distinguish them. |
| **Number of tasks to sync** | Specify the number of tasks to synchronize (2 ~ 8). The issuing task waits until the number of tasks that executed `task sync` with the same identifier matches this value. |
| **Usage example** | <p># Main task program</p><p>print "maintask"</p><p>task start,sub=1,job=11</p><p>...</p><p><mark style="background-color:green;">task sync,id=1,no=2</mark></p><p>print "sync with subtask 1"</p><p>(id=1, number of tasks to sync = 2)</p> |
| | <p># Sub task 1 program</p><p>print "subtask 1"</p><p>...</p><p><mark style="background-color:green;">task sync,id=1,no=2</mark></p><p>print "sync with maintask"</p><p>(id=1, number of tasks to sync = 2)</p> |
