
[__SOURCE](README.md)
# ${cont_model} Controller Manual - Multi-tasking

[__SOURCE](1-overview/README.md)
# 1. Overview

[__SOURCE](1-overview/1-1-about-multi-task.md)
# 1.1 About the multi-tasking feature

The ${cont_model} controller can run up to 8 programs (JOB files) simultaneously and independently. This independent operation mode is referred to as the **multi-tasking feature**.

With multi-tasking, you can execute a robot control program while simultaneously running programs that control other devices. Robot control and other device control can operate independently, and when needed they can work in a synchronized state to cooperate. This enables performing complex and sophisticated application tasks.

Figure 1-1 below shows a single-tasking structure. In this case only one task exists, so it is not possible to independently run two or more programs at the same time. Compared to the multi-tasking structure described later, you can think of this as having only the main task and no sub tasks.

![Figure 1-1 Single-tasking structure](<../_assets/image_1.png>)

Figure 1‑2 below shows a multi-tasking structure. Because up to 8 tasks can run concurrently, one program (JOB file) can be assigned per task, allowing up to 8 programs (JOB files) to run independently and simultaneously. Running 8 tasks concurrently allows independent control of multiple devices. However, robot control is limited to one robot per main task. To perform synchronized tasks using multiple robots simultaneously, please use our cooperative control system.

![Figure 1‑2 Multi-tasking structure](<../_assets/image_2.png>)

The names of the eight tasks that execute programs are as follows:

* Main task
* Sub task 1 ~ 7

The main task is always created and present by default to execute JOB programs. Sub tasks can be created and destroyed as needed. Figure 1‑3 below shows the sub task creation structure. Sub tasks are created automatically when the program executes a `task start` statement. Sub tasks are destroyed automatically when a `task reset` statement is executed or when an `end` statement is executed in each sub task program.

![Figure 1‑3 Sub task creation](<../_assets/image_3.png>)

[__SOURCE](1-overview/1-2-term-explan.md)
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

[__SOURCE](2-related-function/README.md)
# 2. Related functions

[__SOURCE](2-related-function/2-1-command-sentence/README.md)
# 2.1 Command statements

[__SOURCE](2-related-function/2-1-command-sentence/1-task-start.md)
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
...
```

[__SOURCE](2-related-function/2-1-command-sentence/2-task-wait.md)
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

[__SOURCE](2-related-function/2-1-command-sentence/3-task-sync.md)
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

[__SOURCE](2-related-function/2-1-command-sentence/4-task-stop.md)
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

[__SOURCE](2-related-function/2-1-command-sentence/5-task-reset.md)
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

[__SOURCE](2-related-function/2-1-command-sentence/6-axisctrl.md)
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
| **Usage example** | <p># Main task program</p><p>print "maintask"</p><p>move P,spd=30%,accu=3,tool=1  # Servo-gun concurrent move</p><p><mark style="background-color:green;">axisctrl off,a=2 </mark># Do not control the servo-gun from the main task</p><p>task start,sub=1,job=11</p><p>move P,spd=30%,accu=3,tool=1  # Servo-gun movement ignored</p><p>move P,spd=30%,accu=3,tool=1  # Servo-gun movement ignored</p><p>delay 1</p><p>...</p><p>move P,spd=30%,accu=3,tool=1  # Servo-gun movement ignored</p><p>task wait,sub=1 # Wait for sub task 1 to finish</p><p><mark style="background-color:green;">axisctrl on,a=2</mark># Main task now synchronously controls the servo-gun</p><p>move P,spd=30%,accu=3,tool=1  # Servo-gun concurrent move</p><p>move P,spd=30%,accu=3,tool=1  # Servo-gun concurrent move</p><p>...</p> |
| | <p># Sub task 1 program</p><p>print "Servo-gun move / tip dressing / gun search"</p><p>move P,spd=30%,accu=3,tool=1  # Servo-gun move</p><p>spot gun=1,cnd=255,seq=64 # Servo-gun tip dressing</p><p>move P,spd=30%,accu=3,tool=1  # Servo-gun move</p><p>gunsea gun=1,sea=1,pre=100,spd=20  # Servo-gun wear measurement</p><p>move P,spd=30%,accu=3,tool=1  # Servo-gun move</p><p>end</p>

### Notes

```
Robot axis control for robot axes is not supported.
This is treated as a discontinuous statement so the step does not perform cornering.
```

[__SOURCE](2-related-function/2-2-monitoring/README.md)
# 2.2 Monitoring

[__SOURCE](2-related-function/2-2-monitoring/1-monitoring-pane.md)
# 2.2.1 Monitoring pane

From **Window Select** → **Multitasking**, you can view various statuses including the program number assigned to each task.

![Figure 2‑4 Multitasking monitoring pane](<../../_assets/image_6.png>)


[__SOURCE](2-related-function/2-2-monitoring/2-title-frame.md)
# 2.2.2 Title frame

The execution status of the subtask and the selection status of the current task can be checked in the title frame. <br>
Task switching can be done with the [CTRL]+[->] key or the [CTRL]+[<-] key, and you can change the currently selected task. For more details, see "[2.5 Task conversion](../2-5-task-conversion/1-robot-prog.md)". 

![](<../../_assets/image_8.png>)

[__SOURCE](2-related-function/2-3-subtask-create/README.md)
# 2.3 Subtask creation

[__SOURCE](2-related-function/2-3-subtask-create/1-auto-create.md)
# 2.3.1 Automatic creation

When a `task start` statement is executed within a maintask program or a subtask program, the specified program is assigned to the desired subtask and the subtask is created automatically.

[__SOURCE](2-related-function/2-3-subtask-create/2-manual-create.md)
# 2.3.2 Manual creation

This method allows the user to assign and start a program on a desired subtask via teach pendant (TP) operations. The manual creation procedure is as follows:

From **Window Select** → **Multitasking**, move the cursor to the desired subtask and choose **Edit** to select a program.

![Figure 2‑5 Manual subtask creation](<../../_assets/image_4.png>)

[__SOURCE](2-related-function/2-4-subtask-delete/README.md)
# 2.4 Subtask destruction

[__SOURCE](2-related-function/2-4-subtask-delete/1-auto-delete.md)
# 2.4.1 Automatic destruction

When an `end` statement is executed in a subtask program, the subtask is automatically destroyed.

[__SOURCE](2-related-function/2-4-subtask-delete/2-manual-delete.md)
# 2.4.2 Manual destruction

You can manually destroy and clear a subtask by selecting the program number `0` in the monitoring window. Procedure:

From **Window Select** → **Multitasking**, move the cursor to the desired subtask and choose **Edit**, then set the program number to `0`.

![Figure 2‑6 Manual subtask destruction](../../_assets/image.png)

Additionally, executing `task reset` will destroy the associated subtask.

The following operations will destroy all sub tasks:
- Re-selecting the main taskprogram in MANUAL mode
- Executing `*R0 : Task Reset*` in MANUAL mode

[__SOURCE](2-related-function/2-5-task-conversion/README.md)
# 2.5 Task conversion


[__SOURCE](2-related-function/2-5-task-conversion/1-robot-prog.md)
# 2.5.1 Robot program

Task switching is possible using key operations, as shown in the table below. Task switching is only possible between created tasks. 


|            **Operation**            | 　　　**Description**         |
| :---------------------------------: | ---------------------------- |
|       \`CTRL`+\`->`key      | Switch to next task          |
|       \`CTRL`+\`<-`key      | Switch to previous task      |

<br>
[Maintask]

![](<../../_assets/image_9.png>)

<br>

[Subtask 1]

![](<../../_assets/image_10.png>)

[__SOURCE](2-related-function/2-5-task-conversion/2-os-assign.md)
# 2.5.2 Output signal assign

Output signal assignments for each task can be made using the following menu selection procedure. This can be done even if no subtasks are currently assigned. 
 
You can set output signals for each task by using the [Previous task]/[Next task] buttons on the 『F2: System』-> 『2: Control parameter』-> 『2: Input/Output signal setting』-> 『4: Output signal assign』 screen.

<br>
[Main task]

![](<../../_assets/image_11.png>)

<br>

[Subtask 1]

![](<../../_assets/image_12.png>)

[__SOURCE](2-related-function/2-6-job-select/README.md)
# 2.6 Program select


[__SOURCE](2-related-function/2-6-job-select/1-maintask-sel.md)
# 2.6.1 Select from maintask

When you select a program in the main task, all created subtasks are stopped and destroyed. 


|     **Task types**    | 　　 　　**Action content**                                 |
| :-------------------: | ---------------------------------------------------------- |
|       Maintask        | Change program number <br> Clear step and function number  |
|       Subtask         | Clear program number <br> Clear step and function number   |

[__SOURCE](2-related-function/2-6-job-select/2-subtask-sel.md)
# 2.6.2 Select from subtask

When selecting a program in a subtask, only the program in that subtask is newly selected.


|     **Task types**    | 　　 　　**Action content**                                |
| :-------------------: | --------------------------------------------------------- |
|       Maintask        | No change                                                 |
|       Subtask         | Change program number <br> Clear step and function number |

[__SOURCE](2-related-function/2-7-step-goback.md)
# 2.7 Step forward/backward

To step forward or backward all created tasks simultaneously, or only the currently selected task, use the keys summarized in the table below. The step behavior for the main task and subtasks is as follows.

| **Action** | **Description** |
| :--------: | --------------- |
| [**FWD**]/[**BWD**] key | Step forward/backward for all created tasks simultaneously |
| [**CTRL**]+[**FWD**]/[**BWD**] key | Step forward/backward for the currently selected task only |

[__SOURCE](2-related-function/2-8-start.md)
# 2.8 Start

To start a task, in AUTO mode enable **MOTOR ON** and **START**, or in MANUAL mode enable **MOTOR ON** and press the [**FWD**] key. Alternatively, subtasks can be started by executing statements independently or by using external signals in conjunction with script commands.

[__SOURCE](2-related-function/2-9-stop.md)
# 2.9 Stop

If the stop button on the teach pendant is pressed or an external stop signal is input during multi-task operation, all tasks will stop.

In addition, executing the `task stop` statement stops the corresponding subtask.

[__SOURCE](2-related-function/2-10-start-stop-lamp.md)
# 2.10 Start/Stop lamp

The operation status lamp (Start/Stop lamp) on the teach pendant indicates the state of task execution as shown in the table below.

| **Indicator** | **Meaning** |
| :------------: | ------------ |
| Start lamp ON / Stop lamp OFF | At least one task is running |
| Start lamp OFF / Stop lamp ON | All tasks are stopped |

[__SOURCE](2-related-function/2-11-multitask-job/README.md)
# 2.11 Multitask program


[__SOURCE](2-related-function/2-11-multitask-job/1-outline.md)
# 2.11.1 Outline

Applying the multitasking feature allows you to create an independent program that runs the program controlling the additional axis in a subtask.

As shown in the figure below, you can drive additional axes independently in each subtask by specifying a set of mechanisms that do not overlap each other.
![Mechanism set config.](<../../_assets/image_13.png>)

The main task can control both the robot and the assigned additional axes, but the subtask can only control the assigned additional axes.

To use this feature, you need a mechanism set (mechset), mechanism setting, and the axisctrl command. A brief definition is below.
- Mechanism setting : It is composed of a set of axes (robot axes, additional axes), and one mechanism can be selected with a jog to operate it as a mechanism unit. "[${cont_model}  Robot Controller Operation Manual - Mechanism Setting](https://hrbook-hrc.web.app/#/view/doc-hi6-operation/en-${cont_model}-tp630/7-system/6-initialization/6-mechannism-set?cont_model=${cont_model}) 
- Mechanism set : The difference between the optional combination of mechanisms and the mechanism is that the steps in the work program are recorded when they are recorded.  "[${cont_model} Robot Controller Operation Manual - Recording Condition](https://hrbook-hrc.web.app/#/view/doc-hi6-operation/en-${cont_model}-tp630/3-programming/2-prog-edit/2-statement-input/3-rec-cond?cont_model=${cont_model})"
- axisctrl : This is a command regarding the control settings of the additional axis. "[2.1.6 axisctrl](../../2-related-function/2-1-command-sentence/6-axisctrl.md?cont_model=${cont_model})"

[__SOURCE](2-related-function/2-11-multitask-job/2-example.md)
# 2.11.2 Example

Let's illustrate a simple example of a multitasking task performed by a system consisting of a robot and a stationary gun, as shown in the figure below. <br>

![](<../../_assets/image_14.png>)
<br>

Here, the robot can be divided into two cases: a task in which the robot performs spot welding with a stationary gun, and a task in which the stationary gun performs tip dressing and gun search (measuring tip wear) separately from the robot's movements. The structure of the composed work program is as shown in the figure below. <br>

![](<../../_assets/image_15.png>)

- 0001.job running in the main task can be programmed to drive all axes by selecting mechanism set 0, and when the program is run, it sequentially executes move and spot to perform spot welding. 
- After that, if you execute the axisctrl off command in the main program, the specified axis number will be set to be controlled by the subtask. This way, the main task will not move to the target where the stationary gun is recorded.
- Use the task start command to specify a program to be executed in a subtask. The program assigned to a subtask must be configured as a separate mechanism set. The task start command executes 0064.job to be executed independently from 0001.job in the main task.
- When the main task program encounters the task wait command, it waits until the specified subtask is completed and end is executed.
- The main program then executes the axisctrl on command to take control of the additional axes. Afterwards, you can program all axes to be driven by selecting mechanism set 0.


[__SOURCE](2-related-function/2-11-multitask-job/3-warning.md)
# 2.11.3 Warnings

When writing move statements in a program to be run as a subtask, note the following: 

- Only axes that have performed axisctrl off in the move statement attribute should be designated with that mechanism.
- Move statements must be executed exclusively to avoid overlapping mechanisms. 
- It should be recorded as move P. When executed with L and C, the additional axis can move at full speed.
- The speed unit must be recorded in % or sec. If recorded in mm/s, the recorded additional axis can operate at maximum speed.
