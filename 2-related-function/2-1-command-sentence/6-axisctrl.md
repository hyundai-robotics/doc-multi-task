# 2.1.6 axisctrl

axisctrl 명령문은 move 명령문 실행에 의해 각축의 위치를 이동할 때, 해당 부가축에 대해서 로봇과 함께 목표위치로 이동할 지 여부를 지정하는 역할을 수행합니다.  

```
axisctrl <on/off>,a=<부가축 번호>
axisctrl <on/off>,a=[부가축 번호,부가축 번호,...] : 복수지정 가능(최대 4개)
```

|    **항목**    | 　　　　　　　　　　**내용**                                  |
| :----------: | ------------------------------------------------- |
| **on/off** | on=축제어 유효, off=축제어 무효                       |
| **부가축 번호** | 축제어 상태 변경을 위한 부가축 번호(배열에 의한 복수 지정 가능)                       |
|    **사용 예시**   | <p># 메인태스크 프로그램</p><p>print ”maintask”</p><p>move P,spd=30%,accu=3,tool=1  #서보건 동시 이동</p><p><mark style="background-color:green;">axisctrl off,a=2 </mark># 서보건을 메인태스크에서 제어하지 않음</p><p>task start,sub=1,job=11</p><p>move P,spd=30%,accu=3,tool=1  #서보건 이동 X</p><p>move P,spd=30%,accu=3,tool=1  #서보건 이동 X</p><p>delay 1</p><p>…</p><p>move P,spd=30%,accu=3,tool=1  #서보건 이동 X</p><p>task wait,sub=1 #서브태스크 1 종료 대기</p><p><mark style="background-color:green;">axisctrl on,a=2</mark># 서보건을 메인태스크에서 동기 제어함</p><p>move P,spd=30%,accu=3,tool=1  #서보건 동시 이동</p><p>move P,spd=30%,accu=3,tool=1  #서보건 동시 이동</p><p>…</p> |
|                | <p># 서브 태스크 1 프로그램</p><p>print ”서보건 이동/팁드레싱/건서치 동작”</p><p>move P,spd=30%,accu=3,tool=1  #서보건 이동</p><p>spot gun=1,cnd=255,seq=64 #서보건 팁드레싱</p><p>move P,spd=30%,accu=3,tool=1  #서보건 이동</p><p>gunsea gun=1,sea=1,pre=100,spd=20  #서보건 마모량 측정</p><p>move P,spd=30%,accu=3,tool=1  #서보건 이동</p><p>end</p>

### 참고사항

```python
   로봇축에 대한 축제어 기능은 지원하지 않습니다.
   불연속 명령문으로 처리되어 해당 스텝은 코너링을 하지 않습니다.
```
