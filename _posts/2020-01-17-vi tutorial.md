---
title: "vi tutorial"
categories: 
  - 교육
last_modified_at: 2020-01-15T13:00:00+09:00

toc: true
---
2020.01.17

2020.01.17 김기남 책임님의 vi tutorial 교육을 토대로 작성하였습니다.

주로 로그 분석하기, 시스템 분석하기에 쓸 듯
vi 들어가면 처음에 command mode

<br>
* 라인 번호 출력
    * : set nu
* 한글 깨짐
    * :set encoding=utf-8
* 커서 이동
    * 방향키
    * H, J, K, L 로도 커서 이동 가능
    * w : 앞 단어 맨 앞글자
    * b : 이전 단어 맨 앞글자
    * e : 단어 끝 글자
    * \- : 이전 줄 맨 앞 글자로 이동
    * **0 : 문장 맨 앞 글자로 이동**
    * **$ : 문장 맨 끝 글자로 이동**
    * ctrl-d : 페이지 중간단위로 앞으로 이동
    * ctrl-u : 페이지 중간단위로 뒤로 이동
    * **ctrl-f : 페이지 전체단위로 앞으로 이동**
    * **ctrl-b : 페이지 전체단위로 뒤로 이동**
    * M : 페이지 안에서 중간 줄로 이동
    * H : 페이지 안에서 앞 줄로 이동
    * L : 페이지 안에서 끝 줄로 이동
    * **gg : 파일의 맨 처음으로 이동**
    * **G : 파일의 맨 끝 줄로 이동**
* vi - 편집
    * **i : 현재 커서위치에서 쓰기모드**
    * I : 현재 커서위치 맨 앞글자에서 쓰기모드
    * a :
    * A :
    * o :
    * O :
    * cc : 현재 커서위치에 있는 줄 삭제하고 쓰기모드
    * cw : 현재 커서위치부터 단어 끝까지 삭제하고 쓰기모드
    * C : 현재 커서위치부터 줄 끝까지 삭제하고 쓰기모드
    * r :
    * dd : 행 전체 삭제 (계속 command mode)
    * dw : 현재 커서위치에서 단어 단위로 삭제(계속 command mode)
    * D :
    * x : 현재 커서위치에서 한 글자 삭제(계속 command mode)
    * u : undo
    * ctrl-r : redo
    * dG : 현재 커서위치부터 파일 끝까지 삭제 ( d + G )
    * 5x : 5byte 삭제 ( 5 + x )
* vi - search/replace
    * **/string : 'string' 검색**
    * **n : 검색된 단어 다음 찾기**
    * **N : 검색된 단어 이전 찾기**
    * :nohl : 검색된 단어 하이라이트 삭제
    * :%s/pattern/replace[option] : pattern 문자열을 replace 문자열로 전부 치환
* Ex mode
    * :wq : 저장 후 나가기
    * : q! : 저장 안하고 나가기
    * :edit :
    * :e!
* Shell
    * :r !cat test.txt : 파일을 뒤에 붙여넣기
    * :vs : 수평 창 분할
    * :sp : 수직 창 분학
    * ctrl-w, w : 창 이동
    * :qa : 전체 창 닫기
* Mark
    * ma, mb, mc : a,b,c로 마킹
    * `a,`b, \`c : 마킹된 a,b,c로 이동
    * d\`a : 마킹된 a 내용 삭제
* copy/paste
    * yy : 한 줄 복사
    * p : 붙여넣기
    * ma & move & y\`a : a부터 현재 커서위치까지 복사
* Customization
    * :set : 설정한 값 볼 수 있음
    * :set ai : 자동 들여쓰기 (auto indent)
    * :set si : smart indent
    * :set cindent :
    * :set paste : 붙여넣기 했을 때 indent 없이 계단식으로 붙여질 때 해결방법
    * :set nu : line number
    * `no`를 입력하여 옵션 해지가능 (ex. :set nonu)
    * :set ts=4 : \t 문자의 폭
    * :set sts=4 : TAB키의 문자 폭
    * :set sw=4 : 자동 줄 맞추기 폭
    * :set expandtab : TAB 대신에 space로 치환
    * :syntax on
    * :set showmatch : 괄호 매칭된 것 보여줌
    * :set ignorecase : 대소문자 무시하고 검색해라
    * :set smartcase : 대소문자 구분해서 검색해라
    * root 디렉토리에서 .vimrc에서 설정을 기입하면 반영됨
