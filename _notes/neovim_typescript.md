---
title: 'Neovim Typescript 환경 설정'
author: 'vlwkaos'
published: true
---

VSCode를 쓰다가 뭔가 편집에 갑갑함을 느껴 [[Vim]]을 에뮬레이팅하는 확장을 깔아서 사용했었다. 이것저것 나한테 맞게 커스터마이징 하다보니 점점 진짜 Vim에서만 제공하는 기능이 쓰고 싶어져서 (사실 Vim 확장은 매우 느리고 자주쓰는 기능에 버그가 있었다) 아예 갈아 탈까 하는 생각을 했다. 

하지만 내가 원하는 기능을 장착한 Vim을 쓰기 위한 장벽은 생각보다 높았다. VSCode에서 제공하는 여러 Language Feature나, 리팩토링 기능 등등 많지는 않지만 애용하는 기능을 단순 편집기인 Vim에 설정하려면 일일히 다 찾아야하고 이는 꽤나 시간이 많이 소요되는 일이기 때문이다. 그래서 찾아낸 차선책이 VSCode Neo Vim 이라는 Neovim 확장이었다. 

Neovim은 Vim의 포크중 하난데 백엔드로 사용이 가능하도록 만들어져서 다른 프로그램과 연결이 가능하다. VSCode의 각종 설정을 Neovim의 설정 파일인 `init.vim`로 옮겼다. 확실히 Vim 확장보다 속도면에서 빨라졌다. 다만 오픈소스이고 거의 1인 개발이라 개발 속도가 더디고 불안정하고 버그가 많다. 예를 들어 Neovim과 VSCode간의 버퍼 desync가 빈번히 일어난다. 거기까지는 봐줄 수 있지만 `--INSERT--` 모드를 나갈 때 `j,k`등의 입력을 하면 VSCode 버퍼에 입력되는 사태가 발생하는데 굉장히 짜증난다. 아무튼 나는 Vim은 아니지만 Neovim을 쓰기로 마음먹고 시간날 때마다 조금씩 조금씩 필요한 기능을 Neovim에 설정했다. 비슷한 처지에 있는 사람이 이 글을 읽고 도움을 받을 수 있으면 좋을 거 같다.

---

## 어떤 기능이 필요하지?

내게 무엇이 필요한지 정리를 해보았다.

- 키 매핑
  - 패널 간 이동
  - 참조 등
- Typescript Language Feature / Syntax Highlight
  - 심볼, 함수 참조 찾기 등
  - 자동완성, 파일 이름 변경시 import 참조 변경
- 파일 탐색 
  - 파일 이름 변경 등
  - 파일 검색
  - 전체 파일에서 텍스트 검색
  - 전체 파일에서 텍스트 교체
- 프로젝트 별 설정
- Git 관련
  - Git Blame
  - 변경점 확인
  - 병합 도구
- jest 테스트 실행

## 설정 How-To

Powershell의 경우 설정하다가 터미널의 문제인지 뭔지 제대로 GUI가 그려지지 않거나 버그가 많았다. 그래서 리눅스 환경 (2020년 기준 맥OS / WSL2를 쓰고있다) 기준으로 작성하였다.

0. `nodejs`를 설치한다.
1. [`neovim`을 설치한다](https://github.com/neovim/neovim/wiki/Installing-Neovim)
2. [`init.vim`](https://github.com/vlwkaos/dotfiles/blob/main/nvim/init.vim) 파일을 `~/.config/nvim/` 폴더에 넣는다. 
3. [`vim-plug`를 설치한다](https://github.com/junegunn/vim-plug)
4. 터미널에서 `nvim`를 입력하여 Neovim을 실행한다. 아마 에러가 몇개 뜰 텐데 `:PlugInstall` 을 입력하고 플러그인을 설치한다. `:q` 로 Neovim을 종료한다.
5. 다시 Neovim을 실행한다.`:CocInstall`을 입력하여 `coc.nvim`의 플러그인을 설치한다. Typescript Language Feature 기능을 위해 필요하다.
6. [Nerd Font를 설치한다](https://github.com/ryanoasis/nerd-fonts). 파일 아이콘 등을 위해 필요하다. 사용할 폰트 하나만 정해서 다운받아도 된다.
7. [`ripgrep`을 설치한다](https://github.com/BurntSushi/ripgrep) 검색시 사용한다. 터미널에서 쓰는건데 Vim의 `fzf`라는 fuzzy 검색 플러그인에서도 사용할 수 있다.
8. [Watchman을 설치한다](https://facebook.github.io/watchman/) `coc.nvim`의 파일이름 변경시 import 경로 수정 기능을 위해 필요하다.
9. [기타 문제 해결](#기타-문제-해결)

## 사용법과 단축키 설정 [2020-11 기준]

단축키는 분명 사람마다 다 다를 것이라 나열은 해놓지만 보고 수정해서 써야한다. 그리고 플러그인의 모든 기능을 다 쓰고있는 게 아니기 때문에 플러그인 페이지에 가서 문서를 읽어보는 것도 좋다.

- `<Leader>` 키는 스페이스바로 설정되어 있다
- `d`키로 삭제시 클립보드로 가지 않고 바로 삭제. 잘라내기는 `<Leader>d`, `<Leader>D`로 설정해놓았다.
- `*`키로 커서 아래 검색 (커서 아래부터 시작)
- 선택모드에서 `//` 입력시 선택한 항목 파일내 검색
- 선택모드에서 `<Leader>r` 키로 선택한 항목 파일내 replace
- `Tab`, `Shift + Tab`으로 버퍼(탭)간 이동
- `Alt + h,j,k,l`로 나뉜 버퍼(패널)간 포커스를 이동, 맥OS는 Option키가 바로 안먹으므로 [기타 문제 해결](#기타-문제-해결)참조
- `Alt + H,J,K,L`로 창크기 조절
- `<Leader>h,j,k,l` 버퍼 위치를 이동
- `X` 키로 버퍼 닫기 (창을 종료하지 않음)
- `ZZ` 키로 파일 닫기

### 파일 탐색

- `Ctrl + b`로 파일 탐색기 열고 닫기. 탐색기는 `NERDTree`라는 플러그인을 사용한다.
- `<Leader>i` 현재 파일 위치 탐색기에 표시
- `Ctrl + p`로 파일명 검색 `fzf`를 이용한다.
- `<Leader>/`로 파일 전체 텍스트 검색
- 전체 교체를 편하게 하는 방법은 뭘까?

### Typescript Language Feature / Syntax Highlight

`coc.nvim` 플러그인은 마소의 LSP를 사용해서 VS에서 쓰이는 기능을 거의 사용할 수 있게 해준다. 그래서 VSCode의 typescript language feature를 Neovim에서 고대로 쓸 수 있다.

- `gh` VSCode 마우스 호버와 같은 동작
- `gd` go to definition
- `gr` view references
- `gi` go to implementation
- `gy` go to type definition
- `[e, ]e` 다음 에러/ 이전 에러
- `<Leader>.` code action (quick-fix, auto import 등등)
- `F2` 심볼등 이름 변경 (텍스트만 바꾸는게 아니고 모두 바꿈)
- `<Leader>F2` 파일 이름 변경, Watchman이 설치되어 있어야 파일 이름이 바뀔 때 import 경로도 수정해준다.
- `<Leader>ff` 포맷 (따로 정의되어있음...)
- 폴더 이름 변경은 아직...

VSCode에 있는 Typescript 관련 기능 설정은 `:CocConfig`을 입력하면 열리는 `.json` 파일에 설정할 수 있다.
예를 들어 아래처럼 세팅하면 import 경로를 모두 상대경로로 지정한다.

```json
{
  "typescript.preferences.importModuleSpecifier": "relative"
}
```

### Git 기능 사용

`vim-gitgutter` 플러그인을 사용해서 로컬 변경점(hunk)을 바로 확인하고 stage, revert할 수 있다.

<p align='center'>
<img src='/assets/neovim_gitgutter.png'/>
<br><span>로컬 변경점이 + / - 로 표시된다.</span></p>

- `]c`, `[c` 로 이전/ 다음 변경점을 탐색할 수 있다.
- `<Leader>hp` Preview hunk 팝업으로 변경점을 보여줌
- `<Leader>hs` Stage hunk
- `<Leader>hu` Revert hunk

`vim-fugitive`는 깃 커맨드를 Neovim 내부에서 사용할 수 있도록 해준다. 로그는 스트림으로 보여주기 때문에 부적절하고 간단한 커맨드나 아래 키맵으로 정의한 기능이 유용하다.

- `<Leader>gs` Git status를 우측에 연다
- `<Leader>gc` Git mergetool (충돌 열기)
- `<Leader>gd` 충돌 파일에 사용하면 3-way 병합 모드 실행.
- `<Leader>dgl` 병합 모드에서 가운데 놓고 사용. 오른쪽 변경점을 가져옴 (incoming)
- `<Leader>dgh` 병합 모드에서 가운데 놓고 사용. 왼쪽 변경점을 가져옴 (local index)

`vim-fugitive`로 Git status를 열면 거기서 커서 이동이 가능한데 파일 이름 위에서 키 입력을 통해 여러가지를 할 수 있다.

- `-`나 `a`를 입력하여 stage, unstage를 할 수 있다.
- `=`를 입력하여 변경점을 볼 수 있다.
- `P`를 입력하여 git add --patch 기능을 사용할 수 있다.
- 엔터키로 파일을 열 수 있다.

### jest 테스트 실행

두가지 방법이 있다. Neovim 밖에서 `` npx jest `fzf` ``를 입력해서 플러그인을 설치하면서 같이 설치된 `fzf`를 이용해서 테스트 파일명을 검색하거나,
Neovim 내에서 테스트 파일을 열고 `:TestFile` 실행.

### 기타 문제 해결

- Neovim의 버퍼에 쓰기가 되어도 웹팩 watch에서 알아채지 못하는 경우가 있다. 

  [웹팩 공식 문서](https://webpack.js.org/configuration/watch/)를 보면 Vim에서 `backupcopy=auto`라는 설정이 되어있는 경우가 있는데 이 경우 웹팩이 알아채지 못할 수 있다고 한다. 설정에 `set backupcopy=yes`를 추가하면 해결된다.

- Git 브랜치를 변경하면 현재 파일의 내용이 바뀌는데 Neovim에서는 작업하던 파일은 따로 버퍼에 올려두기 때문에 바로 바뀌지 않는다. 

  이를 해결하려면 설정에 `set autoread`를 추가하면 된다.

- 맥OS의 Option키는 자체 기호 입력에 쓰이기 때문에 터미널에서 제대로 입력되지 않는다. 
  
  `iterm2`를 사용한다면 키설정에서 Option키를 `+ESC`를 반환하도록 할 수가 있는데, 이렇게 하고나서 터미널에서 `Ctrl + v`를 입력후 키조합을 입력해보면 어떤식으로 입력이 일어나는지 알 수 있다. 예를 들자면 `Ctrl + v` 하고나서 `Option + l`을 입력하면 아마 `^[l`이렇게 입력될 것이다. 이걸 Vim파일 키 매핑시에 사용하면 잘 작동한다. 참고로 `^[`는 `ESC`와 동일하다 (아까 설정한대로)


---

### 후기 

Neovim을 몇 번 사용해본 결과 플러그인을 많이 설치하지 않았음에도 커서 이동에 버벅임이 있어 결국 사용하기를 포기했다. Vscode에 Neovim 확장을 사용하는 것이 그나마 제일 쾌적하다고 결론을 지었다. 내 VScode용 Neovim 설정 파일이 다른 사람에게 필요한 경우가 있을지 모르겠지만 [여기](https://github.com/vlwkaos/dotfiles/blob/main/vscode/init.vim)서 볼 수 있다. 대부분 VScode 기능을 vim 키로 매핑한 것이다.


