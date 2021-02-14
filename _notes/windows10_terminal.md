---
title: '윈도우10 터미널 환경설정'
author: 'vlwkaos'
published: true
---

작금의 마이크로소프트(이하 마소)는 개발자와 친해지기 위해 꽤 많이 노력 중인 것 같다. VSCode, Fast, TypeScript 등등 개발에 도움되는 여러 오픈 소스 프로젝트를 운영하고 있기도 하고,  특히 리눅스 환경을 윈도우10에 가져오려는 야심찬 계획을 차근차근 실천해나가고 있기도 하다. 최근에는 [리눅스 GUI 프로그램을 WSL에서 실행](https://twitter.com/i/status/1308452901266751488)할 수 있도록 만드려는 것 같다.

정말이지 WSL는 윈도우10에서 개발하는 많은 개발자의 등을 간지러운 곳을 제대로 긁어주고 있다 벅벅. 

하지만 윈도우 환경에서 계속 개발할 수 밖에 없는 경우가 있다. 윈도우에서 개발이 불편했던 이유 중 하나는 쾌적한 터미널 환경이나 패키지 매니저가 없었던 점이 컸다고 생각한다. 이제는 그것도 옛말이 되었다. 또 하나의 마소의 오픈소스 프로젝트. 윈도우즈 터미널을 만나보자.

---

## Windows Terminal 설치

![](/assets/win10_t1.jpg)

스크린샷에서 알 수 있다시피 윈도우즈 터미널을 이용하면 여러가지 커스터마이징이 가능하다! 이게 뭐가 중요하냐고 할 수도 있지만 게임을 할 때도 스킨이 있을 때 더 재미있다. >윈도우즈 터미널< 당장 설치하자. 정말이지 글로벌 대기업의 광고영상을 보고있자면… 가슴이 웅장해진다.

<div class='embed-wrapper'>
<iframe width="560" height="315" src="https://www.youtube.com/embed/8gw0rXPMMPE" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
</div>

이 글에서는 간단하게 Git 표시를 해주는 Powerline 테마까지만 설정해 볼 것이다.

---

## Chocolatey 설치

참고! 진행하는 도중 뭔가 환경변수가 바뀌는 경우에, 혹은 잘 안될 때 refreshenv 를 한번씩 입력해주면 대부분 해결된다. 그래도 안된다면 재부팅...

Chocolately는 이제 꽤나 성숙했다 할 수 있는 윈도우10 패키지 매니저이다. 기본적으로 탑재되어있는 Powershell을 열고 다음 커맨드를 입력해 설치하자. powershell을 빨리 여는 방법은? `win + x a`

```
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString(‘https://chocolatey.org/install.ps1'))`
```

만약 Policy관련해서 오류가 뜬다면 다음 커맨드를 입력한다.

```
Set-ExecutionPolicy -Scope Process -ExecutionPolicy Bypas
```

다 했다면 `choco` 를 입력해서 설치가 잘 됐는지 확인한다.

1. (옵션) `choco install git` 깃…터미널에서 한글이 깨지는 경우가 있을 검색으로 해결하자!
1. (옵션)필요하다면 `choco install vim` 으로 vim도 설치할 수 있다.
1. `choco install gsudo` gsudo는 리눅스의 sudo랑 비슷하다. 관리자 권한으로 elevation을 제공한다.
1. (옵션) `choco install nvm` node 환경이 필요한 개발자라면 node의 버전을 nvm으로 관리할 수 있다. 만약 이미 바이너리 버전의 노드가 설치되어 있고 그게 더 편하다면 안해도된다. `nvm list available`을 입력하면 사용 가능한 node 버전이 나열된다. `nvm install <버전>` 버전을 정확히 입력하면 설치가 된다. 그리고 `nvm use <설치된 버전>` 한번더 정확히 입력해 준다. 재부팅을 해야할 지도 모른다.
1. (옵션) `npm install -g windows-build-tools` 로 추가 개발용 도구를 설치한다.

## 터미널 테마 설정

파워라인 테마를 위해 `oh-my-posh`를 설치한다.

```
Install-Module posh-git -Scope CurrentUser
Install-Module oh-my-posh -Scope CurrentUser
```

폰트는 `nerd-font` 사이트의 폰트를 추천한다. powerline 심볼을 지원하는 아무 폰트나 사용하면 된다. 예를 들어 `Ubuntu Mono derivative Powerline`, `Literation Mono NF` 등등이 있다.

그리고 윈도우즈 터미널을 실행한다. 아래 이미지 처럼 클릭하여 설정 파일을 연다.

![](/assets/win10_t2.png)

아래로 내리다보면 `"profiles"`: 옵션 값이 보일 것이다. 터미널 프로필을 설정할 수 있는 부분인다. 아마 기본적으로 Powershell에 대한 기본 프로필이 있을 것이다. 프로필마다 `"guid"` `"name"` 등 여러 값이 지정되어 있는데 그 아래 `"fontFace"` 라는 속성을 추가하고 아까 받았던 폰트 이름을 정확히 값으로 지정해준다. 추가적으로 `"startingDirectory"`라는 속성을 추가해서 `"."` 라는 값을 준다. 이렇게 하면 탐색기의 주소창에 `wt` 를 입력했을 때 현재 디렉토리에서 윈도우즈 터미널을 열 수 있다.

이제 Powershell에서 `Set-Prompt` 를 입력하여 `oh-my-posh`의 기본 프롬프트 설정을 열면 파워라인 테마가 보일 것이다. 추가적으로 `Set-Theme Agnoster` 로 테마를 바꿀 수도 있다. 더 자세한 사항은 [repo](https://github.com/JanDeDobbeleer/oh-my-posh)를 확인.

## 자동으로 테마 시작

리눅스 bash의 `.bashrc` 파일 처럼 shell이 시작할 때 몇가지 커맨드를 실행하고 싶은 경우 Powershell에서는 어떻게 해야할까?

```
New-Item -Type file -Path $Profile -Force
Set-ExecutionPolicy RemoteSigned
```

를 입력하면 `.bashrc` 역할을 하는 파워쉘용 프로필 파일을 만들어 준다.
아래 줄은 로컬 파워쉘 스크립트를 실행하기 위해 권한을 주는 커맨드이다.

`notepad $profile` 을 입력하면 메모장으로 프로필 파일을 열 수 있다. 그 안에 다음을 입력한다.

```
Set-Prompt
clear
```
이제 윈도우즈 터미널을 열 때 자동으로 `oh-my-posh`로 열린다.

나머지 배경, 테마 등의 설정은 [윈도우즈 터미널repo](https://github.com/microsoft/terminal)와 ColorTool를 참조!

### 미세팁

추가적으로 윈도우즈 터미널의 설정파일을 열어보면 알겠지만 아래로 가보면
```
    // Add custom keybindings to this array.
    // To unbind a key combination from your defaults.json, set the command to "unbound".
    // To learn more about keybindings, visit https://aka.ms/terminal-keybindings
    "keybindings":
    [
        // Copy and paste are bound to Ctrl+Shift+C and Ctrl+Shift+V in your defaults.json.
        // These two lines additionally bind them to Ctrl+C and Ctrl+V.
        // To learn more about selection, visit https://aka.ms/terminal-selection
        { "command": {"action": "copy", "singleLine": false }, "keys": "ctrl+c" },
        { "command": "paste", "keys": "ctrl+v" },

        // Press Ctrl+Shift+F to open the search box
        { "command": "find", "keys": "ctrl+shift+f" },

        // Press Alt+Shift+D to open a new pane.
        // - "split": "auto" makes this pane open in the direction that provides the most surface area.
        // - "splitMode": "duplicate" makes the new pane use the focused pane's profile.
        // To learn more about panes, visit https://aka.ms/terminal-panes
        { "command": { "action": "splitPane", "split": "auto", "splitMode": "duplicate" }, "keys": "alt+shift+d" },
		// Unbind keys first from any other actions so that we can use
		{ "command": "unbound", "keys": "ctrl+shift+ageup" },
		{ "command": "unbound", "keys": "ctrl+shift+pagedown" },
		{ "command": "unbound", "keys": "ctrl+shift+up" },
		{ "command": "unbound", "keys": "ctrl+shift+down" },

		{ "command": "scrollUpPage", "keys": "ctrl+shift+pageup" },
		{ "command": "scrollDownPage", "keys": "ctrl+shift+pagedown" },
		{ "command": "scrollUp", "keys": "ctrl+shift+up" },
		{ "command": "scrollDown", "keys": "ctrl+shift+down" },
	]
```

이런 내용이 있는데 (없으면 집어넣자) 아래 단축키다.

- `ctrl + shift + ↑ / ↓` 터미널 스크롤
- `shift + alt + - / =` 가로 분할 / 세로 분할
- `alt + 방향키` 분할 화면 포커스 이동
- `ctrl + shift + w` 분할 화면 닫기

필요에 따라 더 편한 키로 바꾸면 된다.