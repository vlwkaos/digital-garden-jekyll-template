---
title: 'Vim'
author: 'vlwkaos'
excerpt: 'Vim 사용에 관한 모든 것'
published: true
---

## Vim 설정 이것저것

- `:s` 문자열 교체에 변수를 사용하는 방법

  `%s/regex/\=var/g` 
  다른 플래그를 알고싶다면 [다음을 참조](https://nikodoko.com/posts/vim-substitute-tricks/)
  
- 아래줄에 붙여넣기

  아래줄에 붙여넣는 방법은 두 가지가 있다. 
  1. 줄 선택 모드(VISUAL LINE)에서 줄바꿈 글자까지 복사를 해서 붙여 넣는 방법. 
  2. `o`로 개행 입력 모드로 진입 후 `ESC`로 기본 모드에서 `p`로 붙여넣기.

  두번째 방법의 경우 indent가 유지가 되지 않기 때문에 indent를 유지하기 위해 `o`를 입력했을 때 임의로 글자를 삽입하고 삭제한 뒤 `ESC`를 누르도록 매핑하는 방법이 있다.