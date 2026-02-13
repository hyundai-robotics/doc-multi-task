
[__SOURCE](README.md)
# ${cont_model} 제어기 기능설명서 - 멀티태스킹

[__SOURCE](0-about-this-manual/precautions.md)
# 사전 주의사항

{% include url="https://hrcontentsrelay-bmgae5hdbzapc4bc.koreacentral-01.azurewebsites.net/api/proxy?path=doc-common-pages/ko/precautions.md" %}

[__SOURCE](1-overview/README.md)
# 1. 개요


[__SOURCE](1-overview/1-1-about-multi-task.md)
# 1.1 멀티태스킹 기능에 대하여

${cont_model} 제어기는 총 8개의 프로그램(JOB 파일)을 동시에 독립적으로 실행할 수 있으며, 이러한 독립된 동작방식에 의해 수행되는 멀티태스킹 제어를 "**멀티태스킹 기능**"이라 합니다.

멀티태스크 기능을 이용하면 로봇 제어 프로그램을 실행하면서 동시에 다른 디바이스를 제어하는 프로그램을 실행할 수 있습니다. 이때 로봇 제어와 다른 디바이스 제어를 서로 독립적으로 수행할 수 있고 필요한 경우에는 로봇과 다른 디바이스가 서로 동기된 상태로 협업작업을 할 수도 있어 복잡하고 어려운 응용작업을 수행할 수 있는 이점이 있습니다.

아래의 그림 1-1은 싱글태스킹 구조입니다. 여기서는 1개의 태스크만 존재하여 2개 이상의 프로그램을 동시에 독립적으로 실행 할 수 없습니다. 이어서 설명할 멀티태스킹 구조와 비교해 보면 메인태스크만 존재하고 서브태스크는 없다고 생각하면 됩니다.

![그림 1-1 싱글태스킹 구조](<../_assets/image_1.png>)

아래의 그림 1-2은 멀티태스킹 구조입니다. 최대 8개의 태스크가 동시 실행 가능하기 때문에 각 태스크 당 1개의 프로그램 (JOB 파일)을 할당하여 최대 8개의 프로그램(JOB 파일)을 독립적으로 동시에 실행할 수 있습니다. 8개의 태스크를 동시에 수행함으로써 다수의 디바이스 제어를 독립적으로 수행할 수 있습니다. 그러나 로봇 제어는 메인태스크에서 1 대의 로봇만 가능합니다. 동시에 여러 대의 로봇을 이용해서 동기된 작업을 하시려면 당사의 협조제어를 이용하시기 바랍니다. 

![그림 1-2 멀티태스킹 구조](<../_assets/image_2.png>)

프로그램을 실행하는 8개의 태스크들의 명칭은 아래와 같습니다.

* 메인태스크
* 서브태스크 1 \~ 7

메인태스크는 JOB 프로그램을 수행하기 위해서 항상 기본으로 생성되고 존재하며, 서브태스크는 필요에 따라 생성과 소멸이 가능합니다. 아래의 그림 1-3은 서브태스크 생성 구조입니다. 서브태스크의 생성은 프로그램에서 task start 명령문을 실행할 때 자동으로 이루어집니다. 서브태스크의 소멸은 task reset 명령문이 실행될 때나 각각의 서브태스크 프로그램에서 end 명령문이 실행될 때 자동으로 이루어집니다.

![그림 1-3 서브태스크 생성
](<../_assets/image_3.png>)

[__SOURCE](1-overview/1-2-term-explan.md)
# 1.2 용어 설명

본 설명서에서 사용하는 용어에 대한 설명은 아래의 표와 같습니다.

<mark style="color:green;">표 1-1 멀티태스크 용어 설명</mark>

|         용어                                                              |           설명                                                                                                                                     |
| ----------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------ |
| 프로그램 (job 파일)                                                           | <p>- 제어기의 저장메모리에 저장되어 있는 작업 프로그램<br>(예시: 0001.job, 0002.job, 1001.job 등 제어기에 저장된 job 파일을 지칭합니다.)</p>                                             |
| <p>메인태스크</p><p>(Main task)</p><p>서브태스크 1 ~ 7</p><p>(Sub task 1 ~ 7)</p> | <p>- 작업 프로그램을 로드해서 실행할 수 있는 로봇제어기의 작업프로그램 실행기</p><p>- 총 8개의 태스크가 있고 각 태스크는 한번에 1개의 프로그램만 로드와 실행을 할 수 있습니다.</p>                                   |
| <p>메인태스크 프로그램</p><p>서브태스크 프로그램</p>                                      | <p>- 태스크에 할당된 특정 작업 프로그램을 지칭합니다.<br>(예시: 메인태스크에서 0001.job을 로드한 경우 메인태스크 프로그램은 0001.job 입니다.)</p><p>- 프로그램은 메인태스크 또는 서브태스크로 할당되어야만 실행이 가능합니다.</p> |

[__SOURCE](2-related-function/README.md)
# 2. 관련 기능


[__SOURCE](2-related-function/2-1-command-sentence/README.md)
# 2.1 명령문


[__SOURCE](2-related-function/2-1-command-sentence/1-task-start.md)
# 2.1.1 task start

task start 명령문은 서브태스크를 생성, 서브태스크에 특정 job 프로그램을 할당, 서브태스크의 프로그램을 기동하는 역할을 수행합니다.

task start 명령어는 `[명령입력]`→`[기타]`→`[task]` 순서대로 선택해서 입력을 할 수 있습니다.

```
task start,sub=<서브태스크 번호>,job=<프로그램 번호>
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">항목</th>
      <th style="text-align:left">내용</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        서브태스크 번호
      </td>
      <td style="text-align:left">
        생성할 서브태스크 번호를 지정(0 ~ 7) <br>
        (0으로 지정하면 미사용중인 태스크중 하나를 자동으로 선정하고 이 태스크를 사용)  
      </td>
    </tr>
    <tr>
      <td style="text-align:left"> 
        프로그램 번호
      </td>
      <td style="text-align:left">
        생성된 서브태스크에서 실행할 프로그램을 지정(1 ~ 9999)
      </td>
    </tr>
    <tr>
      <td style="text-align:left"> 
        사용 예시
      </td>
      <td style="text-align:left">
        task start,sub=1,job=11 (서브 태스크 1에 0011.job 를 할당하여 실행) <br>
        task start,sub=0,job=11 (서브 태스크를 자동으로 선정하여 0011.job 를 실행)
      </td>
    </tr>
  </tbody>
</table>


![그림 2 1 task start 명령어 사용 예시](<../../_assets/image_5.png>)

![그림 2 2 서브태스크 생성과 종료대기 예시](<../../_assets/image_7.png>)

주의할 점으로 생성하고자 하는 서브태스크 번호는 자기 자신의 번호가 아닌 다른 서브태스크 번호이어야 합니다. 이외에도 task start 명령은 아래에서 설명하는 경우에는 오류 상황으로 적용이 불가능하니 주의가 필요합니다.

task start를 이용하여 생성하고자 하는 서브태스크가 이미 생성되어 실행중인 경우에 다른 프로그램을 해당 서브태스크에서 생성하면 오류가 발생합니다. 아래의 예시를 참고하시기 바랍니다.

*   <mark style="color:green;">**서브태스크 실행 중 다른 프로그램 할당과 실행 오류**</mark>

    ```
    task start,sub=1,job=11 # subtask 1 was started
    task start,sub=1,job=12
    ...
    ```

[__SOURCE](2-related-function/2-1-command-sentence/2-task-wait.md)
# 2.1.2 task wait

task wait 명령문은 서브태스크의 소멸을 대기하는 역할을 수행합니다. 일반적으로 서브태스크 소멸은 해당 서브태스크 프로그램의 end 명령문 실행에 의해 자동으로 처리됩니다. 작업을 수행 중에 다른 서브태스크의 완료를 대기하였다가 다음 동작을 수행할 때 이용합니다.

```
task wait,sub=<서브태스크 번호>,job=<프로그램 번호>
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">항목</th>
      <th style="text-align:left">내용</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        서브태스크 번호
      </td>
      <td style="text-align:left">
        소멸을 대기하는 서브태스크 번호를 지정(0 ~ 7) <br>
        (0으로 지정하면 기동중인 프로그램 번호에 해당하는  태스크를 찾아 그 태스크의 소멸을 대기)  
      </td>
    </tr>
    <tr>
      <td style="text-align:left"> 
        프로그램 번호
      </td>
      <td style="text-align:left">
        서브태스크에서 번호가 0으로 지정된 경우에 사용. 실행중인 프로그램을 지정(1 ~ 9999)
      </td>
    </tr>
    <tr>
      <td style="text-align:left"> 
        사용 예시
      </td>
      <td style="text-align:left">
        task wait,sub=1 (서브태스크 1의 소멸을 대기) <br>
        task wait,sub=0,job=11 (프로그램 11이 기동중인 태스크의 소멸을 대기)
      </td>
    </tr>
  </tbody>
</table>
[__SOURCE](2-related-function/2-1-command-sentence/3-task-sync.md)
# 2.1.3 task sync

task sync 명령문은 태스크들 사이의 동기를 맞추는 역할을 수행합니다. 일반적으로 2개 이상의 로봇이 협조 작업을 위해서는 동기가 필수적인데, 이 경우 태스크간 동기 시작 시점을 맞출 때 편리하게 사용할 수 있습니다. 메인태스크와 서브태스크가 작업을 수행하다가 특정 지점에서 동시에 작업을 시작하고자 할 때 유용하게 이용할 수 있습니다.

```
task sync,id=<식별자>,no=<동일 id의 실행 갯수>
```

|     **항목**     |           **내용**                                                                                                                                                                                                                   |
| :------------: | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|     **식별자**    | <p>1~32 로 식별자를 지정</p><p>동기할 지점은 프로그램 내에서 여러 개 설정할 수 있으며 이를 구별하기 위해서 사용합니다.</p>                                                                                                                                                     |
| **동기할 태스크 개수** | <p>2~8로 동기할 태스크의 개수를 지정</p><p>task sync 명령어가 실행된 태스크 개수와 일치할 때까지 대기합니다.</p>                                                                                                                                                         |
|    **사용 예시**   | <p># 메인태스크 프로그램</p><p>print "maintask"</p><p>task start,sub=1,job=11</p><p>...</p><p><mark style="background-color:green;">task sync,id=1,no=2</mark></p><p>print "sync with subtask 1"</p><p> </p><p>(태스크 id는 1, 동기할 태스크 개수는 2개)</p> |
|                | <p># 서브 태스크 1 프로그램</p><p>print "subtask 1"</p><p>...</p><p><mark style="background-color:green;">task sync,id=1,no=2</mark></p><p>print "sync with maintask"</p><p> </p><p>(태스크 id는 1, 동기할 태스크 개수는 2개)</p>                            |

[__SOURCE](2-related-function/2-1-command-sentence/4-task-stop.md)
# 2.1.4 task stop

task stop 명령문은 서브태스크의 실행을 강제로 정지하는 역할을 수행합니다. 

```
task stop,sub=<서브태스크 번호>,job=<프로그램 번호>
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">항목</th>
      <th style="text-align:left">내용</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        서브태스크 번호
      </td>
      <td style="text-align:left">
        정지를 원하는 서브태스크 번호를 지정(0 ~ 7) <br>
        (0으로 지정하면 기동중인 프로그램 번호에 해당하는  태스크를 찾아 그 태스크를 정지)  
      </td>
    </tr>
    <tr>
      <td style="text-align:left"> 
        프로그램 번호
      </td>
      <td style="text-align:left">
        서브태스크에서 번호가 0으로 지정된 경우에 사용. 실행중인 프로그램을 지정(1 ~ 9999)
      </td>
    </tr>
    <tr>
      <td style="text-align:left"> 
        사용 예시
      </td>
      <td style="text-align:left">
        task stop,sub=1 (서브태스크 1의 실행을 정지) <br>
        task stop,sub=0,job=11 (프로그램 11이 기동중인 태스크의 실행을 정지)
      </td>
    </tr>
  </tbody>
</table>
[__SOURCE](2-related-function/2-1-command-sentence/5-task-reset.md)
# 2.1.5 task reset

task reset 명령문은 서브태스크를 강제로 소멸하는 역할을 수행합니다. 일반적으로 서브태스크 소멸은 해당 서브태스크 프로그램의 end 명령문 실행에 의해 자동으로 처리됩니다. 

```
task reset,sub=<서브태스크 번호>,job=<프로그램 번호>
```

<table>
  <thead>
    <tr>
      <th style="text-align:left">항목</th>
      <th style="text-align:left">내용</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        서브태스크 번호
      </td>
      <td style="text-align:left">
        소멸을 원하는 서브태스크 번호를 지정(0 ~ 7) <br>
        (0으로 지정하면 기동중인 프로그램 번호에 해당하는  태스크를 찾아 그 태스크를 소멸)  
      </td>
    </tr>
    <tr>
      <td style="text-align:left"> 
        프로그램 번호
      </td>
      <td style="text-align:left">
        서브태스크에서 번호가 0으로 지정된 경우에 사용. 실행중인 프로그램을 지정(1 ~ 9999)
      </td>
    </tr>
    <tr>
      <td style="text-align:left"> 
        사용 예시
      </td>
      <td style="text-align:left">
        task reset,sub=1 (서브태스크 1을 소멸) <br>
        task reset,sub=0,job=11 (프로그램 11이 기동중인 태스크를 소멸)
      </td>
    </tr>
  </tbody>
</table>

[__SOURCE](2-related-function/2-1-command-sentence/6-axisctrl.md)
# 2.1.6 axisctrl

axisctrl 명령문은 move 명령문 실행에 의해 각축의 위치를 이동할 때, 해당 부가축에 대해서 로봇과 함께 목표위치로 이동할 지 여부를 지정하는 역할을 수행합니다.  

```
axisctrl <on/off>,a=<부가축 번호>
axisctrl <on/off>,a=[부가축 번호,부가축 번호,...] : 복수지정 가능(최대 4개)
```

|    **항목**    |           **내용**                                  |
| :----------: | ------------------------------------------------- |
| **on/off** | on=축제어 유효, off=축제어 무효                       |
| **부가축 번호** | 축제어 상태 변경을 위한 부가축 번호(배열에 의한 복수 지정 가능)                       |
|    **사용 예시**   | <p># 메인태스크 프로그램</p><p>print "maintask"</p><p>move P,spd=30%,accu=3,tool=1  #서보건 동시 이동</p><p><mark style="background-color:green;">axisctrl off,a=2 </mark># 서보건을 메인태스크에서 제어하지 않음</p><p>task start,sub=1,job=11</p><p>move P,spd=30%,accu=3,tool=1  #서보건 이동 X</p><p>move P,spd=30%,accu=3,tool=1  #서보건 이동 X</p><p>delay 1</p><p>...</p><p>move P,spd=30%,accu=3,tool=1  #서보건 이동 X</p><p>task wait,sub=1 #서브태스크 1 종료 대기</p><p><mark style="background-color:green;">axisctrl on,a=2</mark># 서보건을 메인태스크에서 동기 제어함</p><p>move P,spd=30%,accu=3,tool=1  #서보건 동시 이동</p><p>move P,spd=30%,accu=3,tool=1  #서보건 동시 이동</p><p>...</p> |
|                | <p># 서브 태스크 1 프로그램</p><p>print "서보건 이동/팁드레싱/건서치 동작"</p><p>move P,spd=30%,accu=3,tool=1  #서보건 이동</p><p>spot gun=1,cnd=255,seq=64 #서보건 팁드레싱</p><p>move P,spd=30%,accu=3,tool=1  #서보건 이동</p><p>gunsea gun=1,sea=1,pre=100,spd=20  #서보건 마모량 측정</p><p>move P,spd=30%,accu=3,tool=1  #서보건 이동</p><p>end</p>

### 참고사항

```python
   로봇축에 대한 축제어 기능은 지원하지 않습니다.
   불연속 명령문으로 처리되어 해당 스텝은 코너링을 하지 않습니다.
```

[__SOURCE](2-related-function/2-2-monitoring/README.md)
# 2.2 모니터링


[__SOURCE](2-related-function/2-2-monitoring/1-monitoring-pane.md)
# 2.2.1 모니터링 창

`[창선택]` → `[멀티태스킹]`에서 각각의 태스크에 할당된 프로그램 번호를 포함하여 각종 상태를 확인할 수 있습니다.

![ 그림 2-4 멀티태스킹 모니터링 창](<../../_assets/image_6.png>)

[__SOURCE](2-related-function/2-2-monitoring/2-title-frame.md)
# 2.2.2 제목 프레임

서브태스크의 실행 상태와 현재 태스크의 선택 상태는 제목프레임 확인할 수 있습니다. <br>
태스크 전환은 [CTRL]+[->]키 또는 [CTRL]+[<-]키로 실행이 가능하며 현재 선택된 태스크를 변경할 수 있습니다. 자세한 내용은 "[2.5 태스크 전환](../2-5-task-conversion/1-robot-prog.md)"을 참고하십시오. 

![](<../../_assets/image_8.png>)

[__SOURCE](2-related-function/2-3-subtask-create/README.md)
# 2.3 서브태스크 생성


[__SOURCE](2-related-function/2-3-subtask-create/1-auto-create.md)
# 2.3.1 자동 생성

메인태스크 프로그램 또는 서브태스크 프로그램 내에서 task start 명령문 실행에 의해 원하는 서브태스크에 프로그램이 할당되어 서브태스크가 자동 생성됩니다.

[__SOURCE](2-related-function/2-3-subtask-create/2-manual-create.md)
# 2.3.2 수동 생성

사용자가 TP 조작을 통해서 원하는 서브태스크에 프로그램을 할당하고 실행시키는 방법입니다. 수동 생성 절차는 다음과 같습니다.

`[창선택]` → `[멀티태스킹]`창에서 원하는 서브태스크로 커서를 이동하여 `[편집]` 버튼으로  프로그램 선택

![그림 2-5 서브태스크 수동생성](<../../_assets/image_4.png>)

[__SOURCE](2-related-function/2-4-subtask-delete/README.md)
# 2.4 서브태스크 소멸


[__SOURCE](2-related-function/2-4-subtask-delete/1-auto-delete.md)
# 2.4.1 자동 소멸

서브태스크의 프로그램에서 end 명령문이 실행되면 서브태스크는 자동 소멸됩니다. 
[__SOURCE](2-related-function/2-4-subtask-delete/2-manual-delete.md)
# 2.4.2 수동 소멸

서브태스크를 수동으로 소멸시키고 클리어하는 것은 모니터링 창에서 프로그램 번호를 0으로 선택하는 방식으로 할 수 있습니다. 절차는 다음과 같습니다.

`[창선택]` → `[멀티태스크]`에서 원하는 서브태스크로 커서를 이동하여 `[편집]` 버튼으로 프로그램 번호를 '**0**'선택

![그림 2 6 멀티태스크 수동 소멸](../../_assets/image.png)

이외에도 task reset을 실행하면 해댱 서브테스크가 소멸됩니다. 
<br/>
<br/>
하기의 조작시에는 모든 서브태스크가 소멸됩니다.
- 수동모드에서 메인태스크의 프로그램을 다시 선택할 때
- 수동모드에서 '*R0 : 태스크 리셋*'을 실행할 때



[__SOURCE](2-related-function/2-5-task-conversion/README.md)
# 2.5 태스크 전환


[__SOURCE](2-related-function/2-5-task-conversion/1-robot-prog.md)
# 2.5.1 로봇 프로그램

태스크 전환 방법은 아래의 표와 같이 키 조작으로 가능합니다. 태스크 전환은 생성된 태스크 사이에서만 전환이 가능합니다. 


|               **동작**             |      **내용**         |
| :--------------------------------: | ------------------------ |
|       `CTRL`+`->`키      | 다음 태스크로 전환         |
|       `CTRL`+`<-`키      | 이전 태스크로 전환         |

<br>
[메인 태스크]

![](<../../_assets/image_9.png>)

<br>

[서브태스크 1]

![](<../../_assets/image_10.png>)

[__SOURCE](2-related-function/2-5-task-conversion/2-os-assign.md)
# 2.5.2 출력 신호 할당

태스크 별로 출력 신호 할당은 다음의 메뉴 선택 절차로 가능합니다. 현재 서브태스크가 할당되어 있지 않은 상태에서도 설정이 가능합니다. 
 
`[F2: 시스템] - 2: 제어 파라미터 - 2:입출력 신호 설정 - 4:출력 신호 할당` 화면에서 `[이전태스크]`/`[다음 태스크]` 버튼에 의해 태스크별 출력 신호를 설정할 수 있습니다.

<br>
[메인 태스크]

![](<../../_assets/image_11.png>)

<br>

[서브태스크 1]

![](<../../_assets/image_12.png>)

[__SOURCE](2-related-function/2-6-job-select/README.md)
# 2.6 프로그램 선택


[__SOURCE](2-related-function/2-6-job-select/1-maintask-sel.md)
# 2.6.1 메인태스크에서 선택

메인태스크에서 프로그램을 선택하면 생성된 모든 서브태스크의 동작이 중단되고 소멸됩니다. 


|     **태스크 종류**    |      **동작 내용**                                |
| :--------------------: | ---------------------------------------------------- |
|       메인태스크        | 프로그램 변경 <br> 스텝번호, 펑션번호 클리어            |
|       서브태스크        | 프로그램 번호 클리어 <br> 스텝번호, 펑션번호 클리어      |

[__SOURCE](2-related-function/2-6-job-select/2-subtask-sel.md)
# 2.6.2 서브태스크에서 선택

서브태스크에서 프로그램 선택 시에는 해당 서브태스크의 프로그램만 새롭게 선택됩니다. 


|     **태스크 종류**    |      **동작 내용**                                |
| :--------------------: | ---------------------------------------------------- |
|       메인태스크        | 변동 사항 없음                                        |
|       서브태스크        | 프로그램 번호 변경 <br> 스텝번호, 펑션번호 클리어       |

[__SOURCE](2-related-function/2-7-step-goback.md)
# 2.7 스텝 전/후진

생성된 모든 태스크를 동시에 스텝 전/후진을 하거나 현재 선택된 태스크만 스텝 전/후진을 하고자 할 때에는 아래 표에 정리된 키를 이용하면 됩니다. 스텝 전/후진 키를 선택할 때 메인태스크와 서브태스크의 스텝 동작은 아래 표와 같습니다.

|               **동작**               |           **내용**       |
| :--------------------------------: | ---------------------- |
|       `FWD`/`BWD`키       | 생성된 모든 태스크 동시에 전/후진 실행 |
| `CTRL`+`FWD`/`BWD`키 | 현재 선택된 태스크만 전/후진 실행    |

[__SOURCE](2-related-function/2-8-start.md)
# 2.8 기동 처리

태스크를 실행하려면 자동모드에서 '**MOTOR ON**', '**START**'를 활성화하거나 수동모드에서 '**MOTOR ON**'을 활성화 시키고 	`FWD` 키를 선택하면 됩니다. 이외에 명령문 독립실행을 통해서 외부 신호와 연계해 서브태스크를 실행할 수도 있습니다.

[__SOURCE](2-related-function/2-9-stop.md)
# 2.9 정지 처리

멀티 태스크 동작 중에 T/P의 정지 버튼을 누르거나 외부의 정지 신호가 입력되면 모든 태스크는 정지됩니다.
<br/>
이외에도 task stop 명령문을 실행하면 해당 서브태스크가 정지합니다.

[__SOURCE](2-related-function/2-10-start-stop-lamp.md)
# 2.10 기동/정지 램프

티치펜던트의 기동/정지 램프는 태스크의 동작 상태를 표시하며 그 상태는 아래의 표와 같습니다.

|        **동작**       |           **내용**  |
| :-----------------: | ----------------- |
| 기동램프 ON  / 정지램프 OFF | 하나의 태스크라도 기동중인 경우 |
|  기동램프 OFF / 정지램프 ON | 모든 태스크가 정지된 경우    |

[__SOURCE](2-related-function/2-11-multitask-job/README.md)
# 2.11 멀티태스킹 프로그램


[__SOURCE](2-related-function/2-11-multitask-job/1-outline.md)
# 2.11.1 개요

멀티태스킹 기능을 응용하면 부가축을 제어하는 프로그램을 서브태스크에서 실행하는 독립적
인 프로그램을 구성할 수 있습니다. 

아래의 그림과 같이 메커니즘 세트를 서로 겹치지 않게 지정하여 각 서브태스크에서 독립적으로 부가축을 구동할 수 있습니다. 
![메커니즘 세트 구성](<../../_assets/image_13.png>)

메인태스크는 로봇과 할당된 부가축의 제어가 모두 가능하지만 서브태스크에서는 할당된 부가축에 대한 제어만 가능합니다. 

본 기능을 사용하기 위해서는 메커니즘 세트(mechset), 매커니즘의 설정, axisctrl 명령의 사용이 
필요합니다. 간략한 정의는 아래와 같습니다. 
- 매커니즘 : 축의 조합(로봇축, 부가축)을 세트로 구성한 것으로 조그로 하나의 매커니즘을 선택해서 매커니즘 단위로 조작이 가능합니다. "[${cont_model} 제어기 조작설명서 - 메커니즘 설정](https://hrbook-hrc.web.app/#/view/doc-hi6-operation/ko-${cont_model}-tp630/7-system/6-initialization/6-mechannism-set?cont_model=${cont_model}) 
- 메커니즘 세트 : 매커니즘의 선택적 조합으로 매커니즘과의 차이는 작업 프로그램에서 스텝을 기
록할 때 기록이 되는 점입니다.  "[${cont_model} 제어기 조작설명서 - 기록 조건](https://hrbook-hrc.web.app/#/view/doc-hi6-operation/ko-${cont_model}-tp630/3-programming/2-prog-edit/2-statement-input/3-rec-cond?cont_model=${cont_model})"
- axisctrl : 부가축의 제어 설정에 관한 명령문입니다. "[2.1.6 axisctrl](../../2-related-function/2-1-command-sentence/6-axisctrl.md)" 



[__SOURCE](2-related-function/2-11-multitask-job/2-example.md)
# 2.11.2 사용 예

아래의 그림과 같이 로봇과 정치서보건으로 구성된 시스템으로 멀티태스킹 작업을 수행하는 간단한 예를 설명하겠습니다. <br>

![](<../../_assets/image_14.png>)
<br>

여기에서는 로봇이 정치서보건으로 스폿용접을 수행하는 작업과 정치서보건이 로봇의 동작과는 별도로 팁드레싱하고 건서치(팁의 마모량 계측)를 수행하는 작업의 2가지의 경우로 나누어 생각할 수 있으며 구성된 작업 프로그램의 형태는 하기의 그림과 같습니다. <br>

![](<../../_assets/image_15.png>)

- 메인태스크에서 구동하는 0001.job은 메커니즘 세트 0을 선택하여 모든 축이 구동되도록 프로그램 할 수 있으며 프로그램을 실행하면 순차적으로 move와 spot을 실행하며 스폿용접 작업을 수행합니다. 
- 그 후에 메인 프로그램에서 axisctrl off 명령을 수행하면 지정된 축 번호는 서브태스크에서 제어되도록 설정됩니다. 이렇게 되면 메인태스크에서는 정치서보건이 기록된 위치로 이동하지 않습니다.
- task start 명령을 이용하여 서브태스크에서 수행할 프로그램을 지정합니다. 서브태스크에 할당된 프로그램은 반드시 독립된 메커니즘 세트으로 설정되어야 합니다. task start 명령에 의해서 0064.job이 메인태스크의 0001.job과 독립적으로 실행됩니다.
- 메인태스크 프로그램에서 task wait 명령을 만나면 지정된 서브태스크가 완료되어 end 를 실행할
때까지 대기합니다.
- 다시 메인 프로그램이 부가축의 제어권을 가지고 오기 위해서 axisctrl on 명령을 실행합니다. 이후에는 메커니즘 세트 0을 선택하여 모든 축이 구동되도록 프로그램 할 수 있습니다.


[__SOURCE](2-related-function/2-11-multitask-job/3-warning.md)
# 2.11.3 티칭시 주의사항

서브태스크로 실행할 프로그램에 move문을 기록할 때 하기의 주의사항을 참고하십시오. 

- move문 속성에서 axisctrl off를 수행한 축에 한하여 해당 메커니즘으로 지정되어야 합니다. 
- move문은 하나의 매커니즘이 서로 겹치지 않도록 배타적으로 실행되어야 합니다. 
- move P로 기록해야 합니다. L과 C로 실행될 경우엔 부가축이 최고속으로 동작할 수 있습니다.
- 속도 단위를 % 혹은 sec로 기록해야 합니다. mm/s로 기록할 경우 기록된 부가축은 최고속으로 동작할 수 있습니다.
