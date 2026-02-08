# 1.1 About the multi-tasking feature

The ${cont_model} controller can run up to 8 programs (JOB files) simultaneously and independently. This independent operation mode is referred to as the **multi-tasking feature**.

With multi-tasking, you can execute a robot control program while simultaneously running programs that control other devices. Robot control and other device control can operate independently, and when needed they can work in a synchronized state to cooperate. This enables performing complex and sophisticated application tasks.

Figure 1-1 below shows a single-tasking structure. In this case only one task exists, so it is not possible to independently run two or more programs at the same time. Compared to the multi-tasking structure described later, you can think of this as having only the main task and no sub tasks.

![Figure 1-1 Single-tasking structure](<../_assets/image_1.png>)

Figure 1-2 below shows a multi-tasking structure. Because up to 8 tasks can run concurrently, one program (JOB file) can be assigned per task, allowing up to 8 programs (JOB files) to run independently and simultaneously. Running 8 tasks concurrently allows independent control of multiple devices. However, robot control is limited to one robot per main task. To perform synchronized tasks using multiple robots simultaneously, please use our cooperative control system.

![Figure 1-2 Multi-tasking structure](<../_assets/image_2.png>)

The names of the eight tasks that execute programs are as follows:

* Main task
* Sub task 1 ~ 7

The main task is always created and present by default to execute JOB programs. Sub tasks can be created and destroyed as needed. Figure 1-3 below shows the sub task creation structure. Sub tasks are created automatically when the program executes a `task start` statement. Sub tasks are destroyed automatically when a `task reset` statement is executed or when an `end` statement is executed in each sub task program.

![Figure 1-3 Sub task creation](<../_assets/image_3.png>)
