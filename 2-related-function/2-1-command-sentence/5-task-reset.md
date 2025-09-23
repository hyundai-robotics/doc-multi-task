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
