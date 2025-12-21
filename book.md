# ${cont_model} Robot Controller Manual - Multi-tasking

{% hint style="warning" %}
The information contained in this product manual is the property of HD Hyundai Robotics.

No part of this manual may be reproduced, redistributed, or provided to any third party, nor used for any purpose, without the prior written consent of HD Hyundai Robotics.

This manual may be changed without prior notice.

**Copyright ⓒ 2023 by HD Hyundai Robotics**
{% endhint %}
# 1. Overview
# 1.1 About the multi-tasking feature

The ${cont_model} controller can run up to 8 programs (JOB files) simultaneously and independently. This independent operation mode is referred to as the **multi-tasking feature**.

With multi-tasking, you can execute a robot control program while simultaneously running programs that control other devices. Robot control and other device control can operate independently, and when needed they can work in a synchronized state to cooperate. This enables performing complex and sophisticated application tasks.

Figure 1-1 below shows a single-tasking structure. In this case only one task exists, so it is not possible to independently run two or more programs at the same time. Compared to the multi-tasking structure described later, you can think of this as having only the main task and no sub tasks.

![Figure 1-1 Single-tasking structure](<../_assets/image_1.png>)

Figure 1‑2 below shows a multi-tasking structure. Because up to 8 tasks can run concurrently, one program (JOB file) can be assigned per task, allowing up to 8 programs (JOB files) to run independently and simultaneously. Running 8 tasks concurrently allows independent control of multiple devices.

![Figure 1‑2 Multi-tasking structure](<../_assets/image_2.png>)

The names of the eight tasks that execute programs are as follows:

* Main task
* Sub task 1 ~ 7

The main task is always created and present by default to execute JOB programs. Sub tasks can be created and destroyed as needed. Figure 1‑3 below shows the sub task creation structure. Sub tasks are created automatically when the program executes a `task start` statement. Sub tasks are destroyed automatically when a `task reset` statement is executed or when an `end` statement is executed in each sub task program.

![Figure 1‑3 Sub task creation](<../_assets/image_3.png>)
# 1.2 Terminology

The terms used in this manual are defined in the table below.

<mark style="color:green;">Table 1‑1 Multitask terminology</mark>

| Term | Description |
| --- | --- |
| Program (job file) | - A job program stored in the controller's non-volatile memory (e.g., 0001.job, 0002.job, 1001.job, etc.). |
| Main task (Main task) / Sub task 1 ~ 7 (Sub task 1 ~ 7) | - The robot controller's program executor that can load and run job programs.
- There are 8 tasks in total; each task can load and run only one program at a time. |
| Main task program / Sub task program | - The specific job program assigned to a task.
- (Example: If the main task loads 0001.job, the main task program is 0001.job.)
- A program can be executed only when it is assigned to either the main task or a sub task. |
# 2. Related functions
# 2.1 Command statements
# 2.1.1 task start

The `task start` statement creates a sub task, assigns a specific job program to it, and starts the sub task program.

The `task start` statement can be entered from **Command Input** → **Other** → **Task**.

```
task start,sub=<sub_task_number>,job=<program_number>
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
      <td style="text-align:left">Specify the sub task number to create (0 ~ 7).<br>(If set to 0, an unused task is selected automatically and used.)</td>
    </tr>
    <tr>
      <td style="text-align:left">Program number</td>
      <td style="text-align:left">Specify the program to run in the created sub task (1 ~ 9999).</td>
    </tr>
    <tr>
      <td style="text-align:left">Usage examples</td>
      <td style="text-align:left">task start,sub=1,job=11 (assign and run 0011.job on sub task 1)<br>task start,sub=0,job=11 (automatically select a sub task and run 0011.job)</td>
    </tr>
  </tbody>
</table>

![Figure 2‑1 Example of using task start](<../../_assets/image_5.png>)

![Figure 2‑2 Example of sub task creation and wait for termination](<../../_assets/image_7.png>)

Note: The sub task number to be created must be a different sub task number than the calling task's own number. Also, `task start` cannot be applied in certain error conditions described below.

If you attempt to create a sub task with `task start` when that sub task is already created and running, assigning another program to that sub task will result in an error. See the example below.

* <mark style="color:green;">**Error when assigning and starting another program on a running sub task**</mark>

```
task start,sub=1,job=11 # subtask 1 was started
task start,sub=1,job=12
…
```
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
# 2.1.3 task sync

The `task sync` statement synchronizes tasks. When two or more robots must cooperate, synchronization is essential; `task sync` is useful to align the start points among tasks. It is handy when the main task and sub tasks need to perform work and then start a step simultaneously at a specific point.

```
task sync,id=<identifier>,no=<number_of_tasks_to_sync>
```

| **Item** | **Description** |
| :------: | --------------- |
| **Identifier** | Specify an identifier from 1 to 32. Multiple sync points can be set in a program and the identifier is used to distinguish them. |
| **Number of tasks to sync** | Specify the number of tasks to synchronize (2 ~ 8). The issuing task waits until the number of tasks that executed `task sync` with the same identifier matches this value. |
| **Usage example** | <p># Main task program</p><p>print "maintask"</p><p>task start,sub=1,job=11</p><p>…</p><p><mark style="background-color:green;">task sync,id=1,no=2</mark></p><p>print "sync with subtask 1"</p><p>(id=1, number of tasks to sync = 2)</p> |
| | <p># Sub task 1 program</p><p>print "subtask 1"</p><p>…</p><p><mark style="background-color:green;">task sync,id=1,no=2</mark></p><p>print "sync with maintask"</p><p>(id=1, number of tasks to sync = 2)</p> |
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
# 2.1.6 axisctrl

The `axisctrl` statement specifies whether auxiliary axes should move together with the robot to the target position when a `move` statement is executed.

```
axisctrl <on/off>,a=<aux_axis_number>
axisctrl <on/off>,a=[aux_axis_number,aux_axis_number,...]  # multiple can be specified (up to 4)
```

| **Item** | **Description** |
| :------: | --------------- |
| **on/off** | on = axis control enabled, off = axis control disabled |
| **aux_axis_number** | The auxiliary axis number for which axis control state is changed (multiple numbers can be specified as an array) |
| **Usage example** | <p># Main task program</p><p>print "maintask"</p><p>move P,spd=30%,accu=3,tool=1  # Servo-gun concurrent move</p><p><mark style="background-color:green;">axisctrl off,a=2 </mark># Do not control the servo-gun from the main task</p><p>task start,sub=1,job=11</p><p>move P,spd=30%,accu=3,tool=1  # Servo-gun movement ignored</p><p>move P,spd=30%,accu=3,tool=1  # Servo-gun movement ignored</p><p>delay 1</p><p>…</p><p>move P,spd=30%,accu=3,tool=1  # Servo-gun movement ignored</p><p>task wait,sub=1 # Wait for sub task 1 to finish</p><p><mark style="background-color:green;">axisctrl on,a=2</mark># Main task now synchronously controls the servo-gun</p><p>move P,spd=30%,accu=3,tool=1  # Servo-gun concurrent move</p><p>move P,spd=30%,accu=3,tool=1  # Servo-gun concurrent move</p><p>…</p> |
| | <p># Sub task 1 program</p><p>print "Servo-gun move / tip dressing / gun search"</p><p>move P,spd=30%,accu=3,tool=1  # Servo-gun move</p><p>spot gun=1,cnd=255,seq=64 # Servo-gun tip dressing</p><p>move P,spd=30%,accu=3,tool=1  # Servo-gun move</p><p>gunsea gun=1,sea=1,pre=100,spd=20  # Servo-gun wear measurement</p><p>move P,spd=30%,accu=3,tool=1  # Servo-gun move</p><p>end</p>

### Notes

```
Robot axis control for robot axes is not supported.
This is treated as a discontinuous statement so the step does not perform cornering.
```
# 2.2 Monitoring
# 2.2.1 Multitask state

From **Window Select** → **Multitasking**, you can view various statuses including the program number assigned to each task.

![Figure 2‑4 Multitasking monitoring window](<../../_assets/image_6.png>)
# 2.3 Sub-task creation
# 2.3.1 Automatic creation

When a `task start` statement is executed within a main task program or a sub task program, the specified program is assigned to the desired sub task and the sub task is created automatically.
# 2.3.2 Manual creation

This method allows the user to assign and start a program on a desired sub task via teach pendant (TP) operations. The manual creation procedure is as follows:

From **Window Select** → **Multitasking**, move the cursor to the desired sub task and choose **Edit** to select a program.

![Figure 2‑5 Manual sub task creation](<../../_assets/image_4.png>)
# 2.4 Sub-task destruction
# 2.4.1 Automatic destruction

When an `end` statement is executed in a sub task program, the sub task is automatically destroyed.
# 2.4.2 Manual destruction

You can manually destroy and clear a sub task by selecting the program number `0` in the monitoring window. Procedure:

From **Window Select** → **Multitasking**, move the cursor to the desired sub task and choose **Edit**, then set the program number to `0`.

![Figure 2‑6 Manual sub task destruction](../../_assets/image.png)

Additionally, executing `task reset` will destroy the associated sub task.

The following operations will destroy all sub tasks:
- Re-selecting the main task program in MANUAL mode
- Executing `*R0 : Task Reset*` in MANUAL mode
# 2.5 Step forward/backward

To step forward or backward all created tasks simultaneously, or only the currently selected task, use the keys summarized in the table below. The step behavior for the main task and sub tasks is as follows.

| **Action** | **Description** |
| :--------: | --------------- |
| [**FWD**]/[**BWD**] key | Step forward/backward for all created tasks simultaneously |
| [**CTRL**]+[**FWD**]/[**BWD**] key | Step forward/backward for the currently selected task only |
# 2.6 Motor ON handling

To start a task, in AUTO mode enable **MOTOR ON** and **START**, or in MANUAL mode enable **MOTOR ON** and press the [**FWD**] key. Alternatively, sub tasks can be started by executing statements independently or by using external signals in conjunction with script commands.
# 2.7 Stop handling

If the stop button on the teach pendant is pressed or an external stop signal is input during multi-task operation, all tasks will stop.

In addition, executing the `task stop` statement stops the corresponding sub task.
# 2.8 Motor ON/STOP lamp

The operation status lamp (Motor ON / Stop lamp) on the teach pendant indicates the state of task execution as shown in the table below.

| **Indicator** | **Meaning** |
| :------------: | ------------ |
| Motor ON lamp ON / Stop lamp OFF | At least one task is running |
| Motor ON lamp OFF / Stop lamp ON | All tasks are stopped |
