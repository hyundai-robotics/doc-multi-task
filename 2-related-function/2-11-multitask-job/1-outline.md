# 2.11.1 Outline

Applying the multitasking feature allows you to create an independent program that runs the program controlling the additional axis in a subtask.

As shown in the figure below, you can drive additional axes independently in each subtask by specifying a set of mechanisms that do not overlap each other.
![Mechanism set config.](<../../_assets/image_13.png>)

The main task can control both the robot and the assigned additional axes, but the subtask can only control the assigned additional axes.

To use this feature, you need a mechanism set (mechset), mechanism setting, and the axisctrl command. A brief definition is below.
- Mechanism setting : It is composed of a set of axes (robot axes, additional axes), and one mechanism can be selected with a jog to operate it as a mechanism unit. "[${cont_model}  Robot Controller Operation Manual - Mechanism Setting](https://hrbook-hrc.web.app/#/view/doc-hi6-operation/en-${cont_model}-tp630/7-system/6-initialization/6-mechannism-set?cont_model=${cont_model}) 
- Mechanism set : The difference between the optional combination of mechanisms and the mechanism is that the steps in the work program are recorded when they are recorded.  "[${cont_model} Robot Controller Operation Manual - Recording Condition](https://hrbook-hrc.web.app/#/view/doc-hi6-operation/en-${cont_model}-tp630/3-programming/2-prog-edit/2-statement-input/3-rec-cond?cont_model=${cont_model})"
- axisctrl : This is a command regarding the control settings of the additional axis. "[2.1.6 axisctrl](../../2-related-function/2-1-command-sentence/6-axisctrl.md?cont_model=${cont_model})"
