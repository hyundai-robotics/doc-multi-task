# 2.11.3 Warnings

When writing move statements in a program to be run as a subtask, note the following: 

- Only axes that have performed axisctrl off in the move statement attribute should be designated with that mechanism.
- Move statements must be executed exclusively to avoid overlapping mechanisms. 
- It should be recorded as move P. When executed with L and C, the additional axis can move at full speed.
- The speed unit must be recorded in % or sec. If recorded in mm/s, the recorded additional axis can operate at maximum speed.
