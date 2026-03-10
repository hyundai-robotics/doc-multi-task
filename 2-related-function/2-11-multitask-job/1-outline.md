# 2.11.1 개요

멀티태스킹 기능을 응용하면 부가축을 제어하는 프로그램을 서브태스크에서 실행하는 독립적
인 프로그램을 구성할 수 있습니다. 

아래의 그림과 같이 메커니즘 세트를 서로 겹치지 않게 지정하여 각 서브태스크에서 독립적으로 부가축을 구동할 수 있습니다. 
![메커니즘 세트 구성](<../../_assets/image_13.png>)

메인태스크는 로봇과 할당된 부가축의 제어가 모두 가능하지만 서브태스크에서는 할당된 부가축에 대한 제어만 가능합니다. 

본 기능을 사용하기 위해서는 메커니즘 세트(mechset), 매커니즘의 설정, axisctrl 명령의 사용이 
필요합니다. 간략한 정의는 아래와 같습니다. 
- 매커니즘 : 축의 조합(로봇축, 부가축)을 세트로 구성한 것으로 조그로 하나의 매커니즘을 선택해서 매커니즘 단위로 조작이 가능합니다. "[${cont_model} 제어기 조작설명서 - 메커니즘 설정](https://hrbook-hrc.web.app/#/view/doc-hi6-operation/ko-tp630/7-system/6-initialization/6-mechannism-set?cont_model=${cont_model})
- 메커니즘 세트 : 매커니즘의 선택적 조합으로 매커니즘과의 차이는 작업 프로그램에서 스텝을 기
록할 때 기록이 되는 점입니다.  "[${cont_model} 제어기 조작설명서 - 기록 조건](https://hrbook-hrc.web.app/#/view/doc-hi6-operation/ko-tp630/3-programming/2-prog-edit/2-statement-input/3-rec-cond?cont_model=${cont_model})"
- axisctrl : 부가축의 제어 설정에 관한 명령문입니다. "[2.1.6 axisctrl](../../2-related-function/2-1-command-sentence/6-axisctrl.md)" 


