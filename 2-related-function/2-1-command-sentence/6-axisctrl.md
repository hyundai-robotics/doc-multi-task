# 2.1.6 `axisctrl`

`axisctrl` 语句指定在执行 `移动 (move)` 语句时，辅助轴是否应与机器人一起移动到目标位置。

```
axisctrl <on/off>,a=<aux_axis_number>
axisctrl <on/off>,a=[aux_axis_number,aux_axis_number,...]  # 可以指定多个（最多 4 个）
```

| **Item** | **Description** |
| :------: | --------------- |
| **on/off** | on = 启用轴控制，off = 禁用轴控制 |
| **aux_axis_number** | 要更改轴控制状态的辅助轴编号（多个编号可以作为数组指定） |
| **Usage example** | <p># 主任务程序</p><p>print "maintask"</p><p>move P,spd=30%,accu=3,tool=1  # 伺服枪并发移动</p><p><mark style="background-color:green;">axisctrl off,a=2 </mark># 不从主任务控制伺服枪</p><p>task start,sub=1,job=11</p><p>move P,spd=30%,accu=3,tool=1  # 伺服枪运动被忽略</p><p>move P,spd=30%,accu=3,tool=1  # 伺服枪运动被忽略</p><p>delay 1</p><p>...</p><p>move P,spd=30%,accu=3,tool=1  # 伺服枪运动被忽略</p><p>task wait,sub=1 # 等待子任务 1 完成</p><p><mark style="background-color:green;">axisctrl on,a=2</mark># 现在主任务同步控制伺服枪</p><p>move P,spd=30%,accu=3,tool=1  # 伺服枪并发移动</p><p>move P,spd=30%,accu=3,tool=1  # 伺服枪并发移动</p><p>...</p> |
| | <p># 子任务 1 程序</p><p>print "伺服枪移动 / 尖端修整 / 枪搜索"</p><p>move P,spd=30%,accu=3,tool=1  # 伺服枪移动</p><p>spot gun=1,cnd=255,seq=64 # 伺服枪尖端修整</p><p>move P,spd=30%,accu=3,tool=1  # 伺服枪移动</p><p>gunsea gun=1,sea=1,pre=100,spd=20  # 伺服枪磨损测量</p><p>move P,spd=30%,accu=3,tool=1  # 伺服枪移动</p><p>end</p>

### Notes

```
不支持机器人轴的机器人轴控制。
这被视为不连续语句，因此该步骤不执行转角。
```