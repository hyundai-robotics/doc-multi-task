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

