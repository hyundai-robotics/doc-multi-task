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