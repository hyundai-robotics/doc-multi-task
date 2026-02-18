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