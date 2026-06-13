
[__SOURCE](README.md)
# ${cont_model} 控制器手册 - 多任务处理
[__SOURCE](0-about-this-manual/README.md)
# 关于手册

[__SOURCE](0-about-this-manual/precautions.md)
# 注意事项

{% include file="zh/precautions.md" %}
[__SOURCE](0-about-this-manual/safety-notice.md)
# 安全注意事项

{% include file="zh/safety-notice.md" %}

[__SOURCE](1-overview/README.md)
# 1. 概述
[__SOURCE](1-overview/1-1-about-multi-task.md)
# 1.1 关于多任务功能

${cont_model} 控制器可以同时独立运行多达 8 个程序 (JOB 文件)。这种独立操作模式称为 **多任务功能**。

通过多任务，您可以在同时运行控制其他设备的程序时执行机器人控制程序。机器人控制和其他设备控制可以独立运行，并在需要时协同工作。这使得执行复杂和精密的应用任务成为可能。

下图 1-1 显示了单任务结构。在这种情况下，仅存在一个任务，因此无法独立同时运行两个或多个程序。与后面描述的多任务结构相比，您可以将其视为只有主任务而没有子任务。

![图 1-1 单任务结构](<../_assets/image_1.png>)

下图 1-2 显示了多任务结构。因为最多可以同时运行 8 个任务，所以每个任务可以分配一个程序 (JOB 文件)，允许多达 8 个程序 (JOB 文件) 独立且同时运行。并行运行 8 个任务允许对多个设备进行独立控制。然而，机器人控制每个主任务仅限于一个机器人。要同时使用多个机器人执行同步任务，请使用我们的协同控制系统。

![图 1-2 多任务结构](<../_assets/image_2.png>)

执行程序的八个任务名称如下：

* 主任务
* 子任务 1 ~ 7

主任务始终默认创建并存在以执行 JOB 程序。子任务可以根据需要创建和销毁。下图 1-3 显示了子任务创建结构。当程序执行 `task start` 语句时，子任务会自动创建。当执行 `task reset` 语句或在每个子任务程序中执行 `end` 语句时，子任务会自动销毁。

![图 1-3 子任务创建](<../_assets/image_3.png>)
[__SOURCE](1-overview/1-2-term-explan.md)
# 1.2 术语

本手册中使用的术语在下表中定义。

<mark style="color:green;">表 1-1 多任务术语</mark>

| 术语 | 描述 |
| --- | --- |
| 程序 (工作文件) | - 存储在控制器非易失性内存中的作业程序（例如，0001.job、0002.job、1001.job等）。 |
| 主任务 (Main task) / 子任务 1 ~ 7 (Sub task 1 ~ 7) | - 可以加载和运行作业程序的机器人控制器程序执行器。
- 总共有 8 个任务；每个任务一次只能加载和运行一个程序。 |
| 主任务程序 / 子任务程序 | - 分配给任务的特定作业程序。
- （示例：如果主任务加载 0001.job，则主任务程序为 0001.job。）
- 仅当程序被分配给主任务或子任务时，才可以执行。 |
[__SOURCE](2-related-function/README.md)
# 2. 相关功能
[__SOURCE](2-related-function/2-1-command-sentence/README.md)
# 2.1 命令语句
[__SOURCE](2-related-function/2-1-command-sentence/1-task-start.md)
# 2.1.1 `task start`

`task start` 语句创建一个子任务，分配一个特定的作业程序给它，并启动子任务程序。

`task start` 语句可以从 `[Command Input] - [Other] - [Task]` 输入。

```
task start,sub=<sub_task_number>,job=<program_number>
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
      <td style="text-align:left">指定要创建的子任务编号 (0 ~ 7)。<br>(如果设为 0，则会自动选择一个未使用的任务并使用。)</td>
    </tr>
    <tr>
      <td style="text-align:left">程序编号</td>
      <td style="text-align:left">指定在创建的子任务中运行的程序 (1 ~ 9999)。</td>
    </tr>
    <tr>
      <td style="text-align:left">使用示例</td>
      <td style="text-align:left">task start,sub=1,job=11 (在子任务 1 中分配并运行 0011.job)<br>task start,sub=0,job=11 (自动选择一个子任务并运行 0011.job)</td>
    </tr>
  </tbody>
</table>

![Figure 2-1 使用 task start 的示例](<../../_assets/image_5.png>)

![Figure 2-2 子任务创建和等待终止的示例](<../../_assets/image_7.png>)

注意：要创建的子任务编号必须与调用任务的自身编号不同。此外，`task start` 不能在以下描述的某些错误条件下应用。

如果尝试在已经创建并运行的子任务上使用 `task start` 创建子任务，并分配另一个程序给该子任务，将导致错误。请参见下面的示例。

* <mark style="color:green;">`在运行的子任务上分配和启动另一个程序时出错`</mark>

```
task start,sub=1,job=11 # 子任务 1 已启动
task start,sub=1,job=12
...
```
[__SOURCE](2-related-function/2-1-command-sentence/2-task-wait.md)
# 2.1.2 `task wait`

`task wait` 语句等待子任务被销毁。通常，在执行该子任务程序中的 `end` 语句时，子任务会被自动销毁。当您希望在继续工作之前等待另一个子任务完成时，请使用此语句。

```
task wait,sub=<sub_task_number>,job=<program_number>
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
      <td style="text-align:left">指定要等待的子任务编号 (0 ~ 7)。<br>(如果设置为 0，控制器会查找运行指定程序编号的任务，并等待该任务被销毁。)</td>
    </tr>
    <tr>
      <td style="text-align:left">程序编号</td>
      <td style="text-align:left">当子任务编号指定为 0 时使用。指定正在运行的程序 (1 ~ 9999)。</td>
    </tr>
    <tr>
      <td style="text-align:left">用法示例</td>
      <td style="text-align:left">task wait,sub=1 (等待子任务 1 被销毁)<br>task wait,sub=0,job=11 (等待运行程序 11 的任务被销毁)</td>
    </tr>
  </tbody>
</table>
[__SOURCE](2-related-function/2-1-command-sentence/3-task-sync.md)
# 2.1.3 `task sync`

`task sync` 语句用于同步任务。当两个或多个机器人必须合作时，同步是必不可少的；`task sync` 在任务之间对齐起点时非常有用。当主任务和子任务需要进行工作并在特定点同时启动一个步骤时，它非常方便。

```
task sync,id=<identifier>,no=<number_of_tasks_to_sync>
```

| **项目** | **描述** |
| :------: | --------------- |
| **标识符** | 指定一个从 1 到 32 的标识符。可以在程序中设置多个同步点，标识符用于区分它们。 |
| **需要同步的任务数量** | 指定要同步的任务数量（2 ~ 8）。发出任务会等待执行 `task sync` 的任务数量与此值匹配。 |
| **用法示例** | <p># 主任务程序</p><p>print "maintask"</p><p>task start,sub=1,job=11</p><p>...</p><p><mark style="background-color:green;">task sync,id=1,no=2</mark></p><p>print "sync with subtask 1"</p><p>(id=1, 需要同步的任务数量 = 2)</p> |
| | <p># 子任务 1 程序</p><p>print "subtask 1"</p><p>...</p><p><mark style="background-color:green;">task sync,id=1,no=2</mark></p><p>print "sync with maintask"</p><p>(id=1, 需要同步的任务数量 = 2)</p> |
[__SOURCE](2-related-function/2-1-command-sentence/4-task-stop.md)
# 2.1.4 `task stop`

`task stop` 语句强制停止子任务的执行。

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
      <td style="text-align:left">指定要停止的子任务编号（0 ~ 7）。<br>(如果设置为0，控制器会找到运行指定程序编号的任务并停止该任务。)</td>
    </tr>
    <tr>
      <td style="text-align:left">程序编号</td>
      <td style="text-align:left">当子任务编号被指定为0时使用。指定正在运行的程序（1 ~ 9999）。</td>
    </tr>
    <tr>
      <td style="text-align:left">用法示例</td>
      <td style="text-align:left">task stop,sub=1 (停止执行子任务1)<br>task stop,sub=0,job=11 (停止执行运行程序11的任务)</td>
    </tr>
  </tbody>
</table>
[__SOURCE](2-related-function/2-1-command-sentence/5-task-reset.md)
# 2.1.5 `task reset`

`task reset`语句强制销毁一个子任务。通常，当子任务程序中执行`end`语句时，子任务会自动被销毁。

```
task reset,sub=<sub_task_number>,job=<program_number>
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
      <td style="text-align:left">指定要销毁的子任务编号 (0 ~ 7)。<br>(如果设置为0，控制器将找到运行指定程序编号的任务并销毁该任务。)</td>
    </tr>
    <tr>
      <td style="text-align:left">程序编号</td>
      <td style="text-align:left">当子任务编号指定为0时使用。指定正在运行的程序 (1 ~ 9999)。</td>
    </tr>
    <tr>
      <td style="text-align:left">用法示例</td>
      <td style="text-align:left">task reset,sub=1 (销毁子任务 1)<br>task reset,sub=0,job=11 (销毁运行程序 11 的任务)</td>
    </tr>
  </tbody>
</table>
[__SOURCE](2-related-function/2-1-command-sentence/6-axisctrl.md)
# 2.1.6 `axisctrl`

`axisctrl` 语句指定在执行 `移动 (move)` 语句时，辅助轴是否应与机器人一起移动到目标位置。

```
axisctrl <on/off>,a=<aux_axis_number>
axisctrl <on/off>,a=[aux_axis_number,aux_axis_number,...]  # 可以指定多个（最多 4 个）
```

| **项目** | **描述** |
| :------: | --------------- |
| **on/off** | on = 启用轴控制, off = 禁用轴控制 |
| **aux_axis_number** | 轴控制状态被更改的辅助轴编号（可以指定多个编号作为数组） |
| **使用示例** | <p># 主任务程序</p><p>print "maintask"</p><p>move P,spd=30%,accu=3,tool=1  # 伺服枪并行移动</p><p><mark style="background-color:green;">axisctrl off,a=2 </mark># 主任务不控制伺服枪</p><p>task start,sub=1,job=11</p><p>move P,spd=30%,accu=3,tool=1  # 伺服枪运动被忽略</p><p>move P,spd=30%,accu=3,tool=1  # 伺服枪运动被忽略</p><p>delay 1</p><p>...</p><p>move P,spd=30%,accu=3,tool=1  # 伺服枪运动被忽略</p><p>task wait,sub=1 # 等待子任务 1 完成</p><p><mark style="background-color:green;">axisctrl on,a=2</mark># 主任务现在同步控制伺服枪</p><p>move P,spd=30%,accu=3,tool=1  # 伺服枪并行移动</p><p>move P,spd=30%,accu=3,tool=1  # 伺服枪并行移动</p><p>...</p> |
| | <p># 子任务 1 程序</p><p>print "伺服枪移动 / 尖端打理 / 枪搜索"</p><p>move P,spd=30%,accu=3,tool=1  # 伺服枪移动</p><p>spot gun=1,cnd=255,seq=64 # 伺服枪尖端打理</p><p>move P,spd=30%,accu=3,tool=1  # 伺服枪移动</p><p>gunsea gun=1,sea=1,pre=100,spd=20  # 伺服枪磨损测量</p><p>move P,spd=30%,accu=3,tool=1  # 伺服枪移动</p><p>end</p>

### 注意事项

```
不支持机器人轴的机器人轴控制。
这被视为一个不连续语句，因此该步骤不会进行转角。
```
[__SOURCE](2-related-function/2-2-monitoring/README.md)
# 2.2 监控
[__SOURCE](2-related-function/2-2-monitoring/1-monitoring-pane.md)
# 2.2.1 监控面板

从 **窗口选择** → **多任务处理**，您可以查看包括分配给每个任务的程序编号在内的各种状态。

![图 2-4 多任务监控面板](<../../_assets/image_6.png>)
[__SOURCE](2-related-function/2-2-monitoring/2-title-frame.md)
# 2.2.2 标题框架

子任务的执行状态和当前任务的选择状态可以在标题框架中检查。 <br>
任务切换可以通过 [CTRL]+[->] 键或 [CTRL]+[<-] 键完成，您可以更改当前选择的任务。有关更多详细信息，请参阅 "[2.5 任务转换](../2-5-task-conversion/1-robot-prog.md)"。

![](<../../_assets/image_8.png>)
[__SOURCE](2-related-function/2-3-subtask-create/README.md)
# 2.3 子任务创建
[__SOURCE](2-related-function/2-3-subtask-create/1-auto-create.md)
# 2.3.1 自动创建

当在 maintask 程序或子任务程序中执行 `task start` 语句时，指定的程序将分配给所需的子任务，并且子任务会自动创建。
[__SOURCE](2-related-function/2-3-subtask-create/2-manual-create.md)
# 2.3.2 手动创建

此方法允许用户通过教学挂件（TP）操作在所需的子任务上分配和启动程序。手动创建程序的步骤如下：

从 **窗口选择** → **多任务处理**，将光标移动到所需的子任务并选择 **编辑** 以选择程序。

![图 2-5 手动子任务创建](<../../_assets/image_4.png>)
[__SOURCE](2-related-function/2-4-subtask-delete/README.md)
# 2.4 子任务破坏
[__SOURCE](2-related-function/2-4-subtask-delete/1-auto-delete.md)
# 2.4.1 自动销毁

当在子任务程序中执行 `end` 语句时，子任务将被自动销毁。
[__SOURCE](2-related-function/2-4-subtask-delete/2-manual-delete.md)
# 2.4.2 手动销毁

您可以通过在监视窗口中选择程序编号 `0` 来手动销毁和清除子任务。程序步骤：

从 **窗口选择** → **多任务处理**，将光标移动到所需的子任务，然后选择 **编辑**，接着将程序编号设置为 `0`。

![图 2-6 手动子任务销毁](../../_assets/image.png)

此外，执行 `task reset` 将销毁相关的子任务。

以下操作将销毁所有子任务：
- 在手动模式下重新选择主任务程序
- 在手动模式下执行 `*R0 : 任务重置*`
[__SOURCE](2-related-function/2-5-task-conversion/README.md)
# 2.5 任务转换
[__SOURCE](2-related-function/2-5-task-conversion/1-robot-prog.md)
# 2.5.1 机器人程序

可以使用下面表中所示的键操作进行任务切换。任务切换仅在创建的任务之间可能。

|            **操作**            |    **描述**         |
| :----------------------------: | ------------------ |
|       `[CTRL]+[->]` 键      | 切换到下一个任务      |
|       `[CTRL]+[<-]` 键      | 切换到上一个任务      |

<br>
[主任务]

![](<../../_assets/image_9.png>)

<br>

[子任务 1]

![](<../../_assets/image_10.png>)
[__SOURCE](2-related-function/2-5-task-conversion/2-os-assign.md)
# 2.5.2 输出信号分配

可以使用以下菜单选择程序为每个任务进行输出信号分配。即使当前没有分配任何子任务，也可以进行此操作。

您可以通过在 `[F2: 系统] - 2: 控制参数 - 2: 2：输入/输出信号设置 - 4: 输出信号分配 ([F2: System] - 2: Control parameter - 2: Input/Output signal setting - 4: Output signal assign)` 屏幕上使用 [上一个任务]/[下一个任务] 按钮来设置每个任务的输出信号。

<br>
[主任务]

![](<../../_assets/image_11.png>)

<br>

[子任务 1]

![](<../../_assets/image_12.png>)
[__SOURCE](2-related-function/2-6-job-select/README.md)
# 2.6 程序选择
[__SOURCE](2-related-function/2-6-job-select/1-maintask-sel.md)
# 2.6.1 从主要任务中选择

当您在主任务中选择程序时，所有创建的子任务都会停止并被销毁。


|     **任务类型**    |      **操作内容**                                 |
| :-------------------: | ---------------------------------------------------------- |
|       主要任务        | 更改程序编号 <br> 清除步骤和功能编号  |
|       子任务         | 清除程序编号 <br> 清除步骤和功能编号   |
[__SOURCE](2-related-function/2-6-job-select/2-subtask-sel.md)
# 2.6.2 从子任务中选择

在子任务中选择程序时，仅会新选择该子任务中的程序。

|     **任务类型**     |      **操作内容**                                |
| :-------------------: | --------------------------------------------------------- |
|       主任务         | 无变化                                                 |
|       子任务         | 更改程序编号 <br> 清除步骤和功能编号 |
[__SOURCE](2-related-function/2-7-step-goback.md)
# 2.7 向前/向后迈步

要同时向前或向后迈步所有创建的任务，或者仅向当前选定的任务，使用下面表格中总结的按键。主任务和子任务的步骤行为如下。

| **操作** | **描述** |
| :--------: | --------------- |
| `[FWD]`/`[BWD]` 键 | 同时向前/向后迈步所有创建的任务 |
| `[CTRL]`+`[FWD]`/`[BWD]` 键 | 仅向当前选定的任务迈步 |
[__SOURCE](2-related-function/2-8-start.md)
# 2.8 开始

要开始任务，在自动模式下启用 `MOTOR ON` 和 `START`，或在手动模式下启用 `MOTOR ON` 并按 `[FWD]` 键。或者，可以通过独立执行语句或将外部信号与脚本命令结合使用来启动子任务。
[__SOURCE](2-related-function/2-9-stop.md)
# 2.9 停止

如果在多任务操作期间按下教学操作面板上的 `停止 (stop)` 按钮或输入外部停止信号，则所有任务将停止。

此外，执行 `task stop` 语句将停止相应的子任务。
[__SOURCE](2-related-function/2-10-start-stop-lamp.md)
# 2.10 启动/停止灯

教师挂件上的操作状态灯（启动/停止灯）指示任务执行的状态，如下表所示。

| **指示灯** | **含义** |
| :------------: | ------------ |
| 启动灯亮 / 停止灯灭 | 至少有一个任务正在运行 |
| 启动灯灭 / 停止灯亮 | 所有任务已停止 |
[__SOURCE](2-related-function/2-11-multitask-job/README.md)
# 2.11 多任务程序
[__SOURCE](2-related-function/2-11-multitask-job/1-outline.md)
# 2.11.1 概述

应用多任务特性允许您创建一个独立程序，在子任务中运行控制附加轴的程序。

如下图所示，您可以通过指定一组不重叠的机制，在每个子任务中独立驱动附加轴。
![机制集配置。](<../../_assets/image_13.png>)

主任务可以控制机器人和指定的附加轴，但子任务只能控制指定的附加轴。

要使用此功能，您需要一个机制集（mechset）、机制设置和 axisctrl 命令。以下是简要定义。
- 机制设置：由一组轴（机器人轴，附加轴）组成，可以通过 jog 选择一个机制作为机制单元进行操作。 "[${cont_model} 机器人控制器操作手册 - 机制设置](https://hrbook-hrc.web.app/#/view/doc-hi6-operation/en-${cont_model}-tp630/7-system/6-initialization/6-mechannism-set?cont_model=${cont_model}) 
- 机制集：可选机制组合与机制的区别在于，当录制工作程序时步骤会被记录。 "[${cont_model} 机器人控制器操作手册 - 录制条件](https://hrbook-hrc.web.app/#/view/doc-hi6-operation/en-${cont_model}-tp630/3-programming/2-prog-edit/2-statement-input/3-rec-cond?cont_model=${cont_model})"
- axisctrl：这是关于附加轴控制设置的命令。 "[2.1.6 axisctrl](../../2-related-function/2-1-command-sentence/6-axisctrl.md?cont_model=${cont_model})"
[__SOURCE](2-related-function/2-11-multitask-job/2-example.md)
# 2.11.2 示例

让我们用一个简单的例子来说明一个由机器人和固定枪组成的系统执行的多任务任务， 如下图所示。 <br>

![](<../../_assets/image_14.png>)
<br>

在这里，机器人可以分为两种情况：一种是机器人用固定枪进行点焊的任务，另一种是固定枪执行尖端修整和枪搜索（测量尖端磨损），与机器人的运动分开。组合工作程序的结构如下图所示。 <br>

![](<../../_assets/image_15.png>)

- 在主任务中运行的0001.job可以通过选择机制集0来编程驱动所有轴，当程序运行时，它依次执行移动和点焊以进行点焊。
- 之后，如果在主程序中执行axisctrl off命令，则指定的轴号将被设置为由子任务控制。这样，主任务就不会移动到记录有固定枪的目标位置。
- 使用任务开始命令来指定要在子任务中执行的程序。分配给子任务的程序必须配置为单独的机制集。任务开始命令执行0064.job，以便独立于主任务中的0001.job执行。
- 当主任务程序遇到任务等待命令时，它将等待指定的子任务完成并执行结束。
- 然后，主程序执行axisctrl on命令来控制附加轴。之后，您可以通过选择机制集0来编程驱动所有轴。
[__SOURCE](2-related-function/2-11-multitask-job/3-warning.md)
# 2.11.3 警告

在编写将作为子任务运行的程序中的移动语句时，请注意以下几点：

- 仅能使用在移动语句属性中执行了 axisctrl off 的轴进行指定。
- 移动语句必须独占执行，以避免重叠机制。
- 应记录为 move P。当与 L 和 C 一起执行时，额外的轴可以以全速移动。
- 速度单位必须记录为 % 或 sec。如果记录为 mm/s，则记录的额外轴可以以最大速度运行。