# 2.11.1 大纲

应用多任务功能允许您创建一个独立的程序，在子任务中运行控制附加轴的程序。

如下面图所示，您可以通过指定一组不重叠的机制独立驱动每个子任务中的附加轴。
![Mechanism set config.](<../../_assets/image_13.png>)

主任务可以控制机器人和指定的附加轴，但子任务只能控制指定的附加轴。

要使用此功能，您需要一个机制集（mechset）、机制设置和 axisctrl 命令。简要定义如下：
- 机制设置：由一组轴（机器人轴、附加轴）组成，可以选择一个机制进行操作，作为机制单元。 "[${cont_model}  Robot Controller Operation Manual - Mechanism Setting](https://hrbook-hrc.web.app/#/view/doc-hi6-operation/zh-tp630/7-system/6-initialization/6-mechannism-set?cont_model=${cont_model}) 
- 机制集：可选机制组合和机制之间的区别在于，工作程序中的步骤在记录时进行记录。 "[${cont_model} Robot Controller Operation Manual - Recording Condition](https://hrbook-hrc.web.app/#/view/doc-hi6-operation/zh-tp630/3-programming/2-prog-edit/2-statement-input/3-rec-cond?cont_model=${cont_model})"
- axisctrl：这是与附加轴的控制设置相关的命令。 "[2.1.6 axisctrl](../../2-related-function/2-1-command-sentence/6-axisctrl.md?cont_model=${cont_model})"