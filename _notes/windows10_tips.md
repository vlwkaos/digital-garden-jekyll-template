---
title: '윈도우10 팁 및 유용한 앱'
author: 'vlwkaos'
published: true
---

컴퓨터를 효율적으로 잘 다루기 위해서는 컴퓨터 내의 각종 기능을 잘 다루기만 해서는 안된다. 자신에게 필요한 기능이 뭔지 파악하고 탐색하거나 그것을 어떻게 충족 시킬 것인지 항상 고민하는 자세가 필요하다. 이 글에서는 누구에게나 도움 될 법한 윈도우10 팁과 앱을 소개할 것이다.

---

## 가상 데스크톱 기능

윈도우 10은 여러개의 가상데스크톱 기능을 지원한다. 이 기능은 윈도우 7을 쓰던 시절 맥OS의 가장 탐났던 기능 중 하나이다. 윈도우 키 + TAB을 누르면 마치 맥OS의 미션 컨트롤과 비슷한 화면이 나오면서 열려있는 모든 윈도우의 썸네일이 보여진다.

<p align='center'>
<img src='/assets/wind10tip1.png'/>
<br><span>가상 데스크톱 타임라인</span></p>

여기서 상단에 ‘새 데스크톱’을 누르면 데스크톱이 추가된다. 맥OS와 다른 점은 모든 모니터에 하나의 가상 데스크톱을 사용한다는 점이다. 맥OS는 이 부분을 설정에서 변경할 수 있다.

마이크로소프트가 공식 인증하는 Precision-Touchpad가 탑재된 랩톱을 이용하면 맥OS처럼 제스쳐로 이용할 수도 있다. 일반적인 데스크톱 상황에서는 꽤나 귀찮은 단축키 조합(CTRL + 방향키 좌우)을 사용해야 하는데 여간 불편한게 아니다. 그렇기 때문에 이를 해결할 방법이 필요하다.

### AutoHotkey로 나만의 단축키를 만들자

[AutoHotkey](https://www.autohotkey.com/)(이하 오토핫키)는 사실 윈도우를 웬만큼 다룰 줄 아는 사람이라면 들어 봤을 프로그램(요즘에는 앱이라는 단어가 더 적절하지만…)이다. 오토핫키를 이용하면 단순히 단축키 변경 뿐만 아니라 스크립팅(코딩과 비슷하다)을 통해 어지간한 기능은 모두 구현이 가능하다. 설치후에 `.ahk` 파일을 만들어 스크립트를 실행할 수 있다.

예를 들어 다음은 작업표시줄 위에서 마우스 휠을 조작할 경우 볼륨을 조절할 수 있는 스크립트이다

<script src="https://gist.github.com/vlwkaos/176534b2585da9fc5d4e43ffc66f98e4.js"></script>

윈도우10의 가상 데스크톱 기능을 최대한 활용하는 방법은 sdias라는 github유저가 만든 [Virtual Desktop Enhancer](https://github.com/vlwkaos/win10-virtual-desktop-enhancer) 를 이용하는 것이다. 링크된 버전은 내가 살짝 수정한 상태인데 다음이 설정되어있다.

- Capslock + 1,2,3,4,5 로 데스크톱 1,2,3,4,5 번으로 이동이 가능하다. 참고로 Capslock기능은 비활성화 되어있다.
- Capslock + F1 로 해당 윈도우를 가상 데스크톱 전체에 보여지도록 할 수 있다. (토글)
- Capslock + F2로 윈도우를 가장 상위에 뜨게 할 수 있다.(토글)

오토핫키를 설치하고 vdemain.ahk를 더블클릭으로 실행하면 된다. 만약 단축키를 바꾸고 싶다면 [이 부분](https://github.com/vlwkaos/win10-virtual-desktop-enhancer/blob/bbd3e84b3a2b6424cb5860cffb740b5b651a6e39/vdemain.ahk#L13) 을 바꿔주면 된다. 다양한 핫키의 사용법은 [공식 문서를 참조](https://ahkscript.github.io/ko/docs/Hotkeys.htm)하자.

마우스로 윈도우를 드래그 하는 동안 Capslock + 1,2,3,4,5 를 누르면 해당 가상 데스크톱으로 이동 시킬 수 있다.

---

## 윈도우10을 슈퍼차징 해주는 도구 모음

오토핫키가 어렵다면 부족한 윈도우10 기능을 채워주는 [파워토이](https://github.com/microsoft/PowerToys)를 이용하면 좀 더 친숙한 환경에서 여러가지 기능을 추가할 수 있다. 깃허브 우측의 Releases 항목에서 다운로드가 가능하다. 포함된 프로그램은 다음과 같다.

- 컬러 픽커 Color Picker
- 윈도우 레이아웃 쉽게 설정 해주는 FancyZones
- 윈도우 탐색기에서 Svg 등 이미지 미리보기 File Explorer Add-ons
- 이미지크기 조절을 단번에 할 수 있는 Image Resizer
- 여러 단축키 설정을 GUI 로 Keyboard Manager
- 여러 파일 이름을 한꺼번에 바꿔주는 PowerRename
- 맥OS의Spotlight같은 PowerToys Run, 어디서든 모든 앱을 실행하자

개인적으로 파워토이를 사용하지는 않고 더 나은 버전이라고 생각되는 다른 프로그램을 이용한다.

### FancyZones 대체제 WindowGrid

[WindowGrid](http://windowgrid.net/) 는 정해진 열과 행개수에 맞춰 창을 띄울 수 있게 해주는 프로그램이다. 아래 공식 사이트의 GIF를 보면 이해가 갈 것이다.

<p align='center'>
<img src='https://miro.medium.com/max/480/0*8OxnAfavR7Xpt0bu.gif' />
<br><span>WindowGrid 웹사이트에서 가져온 시연 이미지</span></p>

기본적인 사용법은 마우스 좌클릭(유지)으로 창을 원하는 곳에 위치하고 우클릭 한 번 하고나서 마우스를 이동하여 원하는 크기로 조절한다. 마지막으로 좌클릭 버튼을 뗄 때 창 크기가 설정된다.

### PowerToys Run 보다 좋은 Listary

[Listary 6 Beta](https://discussion.listary.com/t/listary-6-beta/4615) 는 지금 무료로 이용할 수 있다. Listary를 이용하면 어디서든 검색을 통해 앱이나 파일을 열 수 있을 뿐만 아니라 특히 윈도우 탐색기에서 바로 검색이 가능해진다는 아주 큰 장점이 있다.

<p align='center'>
<img src='/assets/wind10tip2.png' />
<br><span>탐색기에서 Listary</span></p>

탐색기를 열고 키보드를 입력하면 자동으로 우측 하단에 Listary검색이 열려서 원하는 문서나 폴더에 훨신 빠르게 접근이 가능하다.

---

## 키보드 마우스를 여러 컴퓨터에 사용하기

[barrier](https://github.com/debauchee/barrier)(이하 베리어) 는 가상 KVM이다. KVM은 마우스와 키보드를 여러 클라이언트에 연결하여버튼 하나로 연결 위치를 바꿔주는 장치이다. 그러니까 베리어를 사용하면 같은 네트워크 상에서 KVM 없이 마우스와 키보드를 여러대의 컴퓨터를 오가며 사용할 수 있다. 하지만 설정이 조금 까다롭다.

우선 실제 마우스와 키보드가 연결된 컴퓨터를 실행 후 서버로 설정하고 Start를 누르면 된다.아래 이미지의 빨간 박스에 표시된 버튼을 통해 각 스크린의 위치등 설정을 할 수 있는데 세세한 설정은 불가능하다. 그리고 다른 컴퓨터에서는 아래 Client 에 체크 한 뒤 서버 컴퓨터의 IP를 입력하고 Start를 누르면 된다. Auto config 옵션은 내가 했을 때는 잘 안됐다.

<p align='center'>
<img src='/assets/win10tip3.png' />
<br><span>barrier GUI</span></p>

그런데 베리어에는 한 가지 결함이 있다. 다중 모니터를 사용할 때, 예를 들어 모니터 두개를 가로, 세로로 연결했다면 가로 세로의 최대 너비와 높이가 되는 직사각형 기준으로 마우스 가장자리 인식이 된다. 그러니까 가로, 세로 모니터라면 가로 모니터 아래나 위쪽으로 가져가도 상단 하단으로 인식하지 않는다. 아래 이미지를 참조하면 이해가 갈 것이다.

<p align='center'>
<img src='/assets/wind10tip4.png' />
<br><span>X친 곳은 가장자리 취급이 되지 않는다</span></p>

하지만 이것 역시 조금 복잡하지만 오토핫키로 해결이 가능하다.

일단 세부 설정을 위해 베리어 역시 설정 파일을 이용한 설정을 해야한다. 베리어 설정은 `*.sgc` 형식의 파일을 이용한다. ‘Use existing configuration’ 옵션을 선택하고 경로를 지정하면 된다. 아래는 `barrierrc.sgc` 의 예시이다.

<script src="https://gist.github.com/vlwkaos/dd47393e518f189a54ad2688fa0fba58.js"></script>

여기서 주의할 부분은 맨 아래 F23키를 toggleScreen 기능에 할당하는 부분이다. 그러니까 오토핫키에서 마우스가 특정 영역에 들어갔을 때 F23키를 발동시키는 오토핫키 스크립트를 짤 것이다.

<script src="https://gist.github.com/vlwkaos/41e99dbab82378a5d44fa1a7b1dd29f1.js"></script>

이 스크립트를 사용하면 `LAlt + 0` 을 눌러서 가장자리 감지 기능을 토글할 수 있다. 1080p 모니터를 세로, 가로로 사용하고 있는 내 설정을 기반으로 하고 있다. 23번 줄의 주석을 없애면 마우스 위치가 툴팁으로 표시된다. 이를 이용해서 원하는 너비와 높이를 결정해 xpos, ypos 조건부를 수정하면 된다.

---

## 윈도우 캡쳐 도구말고 ShareX

[ShareX](https://getsharex.com/) 는 정말 없는 기능이 없는 이미지 캡쳐 툴이다. GIF/영상부터 이미지 annotation까지 모두 포함되어 있고 캡쳐 후 클립보드에 저장, imgur등 이미지 호스팅 서비스에 업로드, 워터마크 지정, 커맨드 실행, 인쇄 등등 컴퓨터의 거의 모든 기능을 후처리로 사용할 수 있다.

<p align='center'>
<img src='/assets/wind10tip5.png' />
<br><span>ShareX의 수많은 기능</span></p>

---

## 윈도우 스토어 앱까지 삭제 가능한 통합 언인스톨러

[Geek Uninstaller](https://geekuninstaller.com/) 포터블로 쓸 수 있고 쉽게 사용이 가능한 삭제 프로그램이다. 더 설명할게 딱히 없다.

<p align='center'>
<img src='/assets/wind10tip6.png' />
<br><span>Geek Uninstaller 메인 화면</span></p>

---

## 마무리

적용하기 쉬운 것도 있고 어려운 것도 있는데 이 글을 읽는 사람들에게 도움이 되었으면 좋겠다. 사실 엄청 세세하게 적은게 아니라서 프로그램 설치만 하면 되는 부분을 제외하고는 적용하기 어려울 지도 모르겠다.
