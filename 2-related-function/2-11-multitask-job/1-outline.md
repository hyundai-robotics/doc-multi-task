# 2.11.1 概述

应用多任务特性允许您创建一个独立程序，在子任务中运行控制附加轴的程序。

如下图所示，您可以通过指定一组不重叠的机制，在每个子任务中独立驱动附加轴。
![机制集配置。](<../../_assets/image_13.png>)

主任务可以控制机器人和指定的附加轴，但子任务只能控制指定的附加轴。

要使用此功能，您需要一个机制集（mechset）、机制设置和 axisctrl 命令。以下是简要定义。
- 机制设置：由一组轴（机器人轴，附加轴）组成，可以通过 jog 选择一个机制作为机制单元进行操作。 "[${cont_model} 机器人控制器操作手册 - 机制设置](https://hrbook-hrc.web.app/#/view/doc-hi6-operation/en-${cont_model}-tp630/7-system/6-initialization/6-mechannism-set?cont_model=${cont_model}) 
- 机制集：可选机制组合与机制的区别在于，当录制工作程序时步骤会被记录。 "[${cont_model} 机器人控制器操作手册 - 录制条件](https://hrbook-hrc.web.app/#/view/doc-hi6-operation/en-${cont_model}-tp630/3-programming/2-prog-edit/2-statement-input/3-rec-cond?cont_model=${cont_model})"
- axisctrl：这是关于附加轴控制设置的命令。 "[2.1.6 axisctrl](../../2-related-function/2-1-command-sentence/6-axisctrl.md?cont_model=${cont_model})"