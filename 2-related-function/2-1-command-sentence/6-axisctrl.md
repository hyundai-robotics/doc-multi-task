# 2.1.6 `axisctrl`

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
