# 2.1.1 task start

task start 명령문은 서브태스크를 생성, 서브태스크에 특정 job 프로그램을 할당, 서브태스크의 프로그램을 기동하는 역할을 수행합니다.

task start 명령어는 『**명령입력**』→『**기타**』→『**task**』 순서대로 선택해서 입력을 할 수 있습니다.

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
