---
title: "[IDE] : VSCODE 개인 설정"
excerpt_separator: <!--more-->
categories:
  - IDE
  - VSCODE
tags:
  - IDE
  - VSCODE
  - Setting
toc: true
toc_sticky: true
toc_label: 목차
image: /assets/img/thumbnail/visual-studio-code-logo.png
---

회사에서는 WebStorm을 쓰고 있었는데, 유료인 Jetbrains를 사용하지 못 하는 환경에 대비하여 VSCode에 익숙해지고 앞으로는 VSCode를 사용해보려고 합니다. 더 유용한 설정들이나 확장프로그램이 있다면 추천해주시면 감사하겠습니다.👍



## VSCode 기본 설정
`지속적으로 업데이트 중...`

## 확장프로그램
VSCode는 다양한 확장프로그램으로 마치 유료 IDE처럼 사용할 수 있기 때문에, 알맞은 확장프로그램을 사용하는 것이 생산성 향산에 많은 도움이 될 수 있습니다.

### 편의성 관련
1. `Image Preview`: 이미지 URL에 마우스를 올리면 이미지 미리보기
2. `Live Server`: 개발 시 실시간으로 확인할 수 있는 서버
3. `Path Intellisense`: 파일명 및 경로 자동 완성
4. `Svg Preview`: svg 파일 편집/미리보기 등
5. `Auto Import`: 파일명 입력 시 자동 import
6. `Code Runner`: 특정 라인을 바로 실행하여 확인 가능. typescript 실행을 위해서는  `ts-node` 설치 해야함
7. `bookMarks`: 특정 라인 북마크
8.  `Bracket Select`: `alt + a`로 괄호 안 문자 선택
9.  `Surround`: `ctrl + shift + T`를 이용해 try/catch로 특정 코드를 감쌀 때 유용 (for문 등도 생성 가능)
10. `Doxygen Documentation Generator`: 주석 설명 포멧 자동 완성
11. `Polacode`: 코드를 스타일 입힌 후 스크린샷
12. `Explorer Exclude`: 폴더 트리 메뉴에서 특정 폴더 숨김 처리
13. `Random Everything`: 다양한 랜덤 값 간편하게 생성
14. `Thunder Client`: Postman을 대신하여 VSCode에서 간편하게 사용 가능
15. `project-tree`: 프로젝트 폴더 구조를 마크다운 텍스트로 저장
16. `diff`: 두 파일 간 다른 점 비교
17. `Partial Diff`: 파일 내 선택한 영역 간단 비교
18. `Font Switcher`: 에디터 종료없이 폰트 실시간 변경
19. `Font Awesome Auto-complete & Preview`: Font Awesome 사용 시 아이콘 클래스 미리보기
20. `Font Awesome Gallery`: Font Awesome 내 아이콘 검색
21. `npm Intellisense`: 설치된 module 자동 완성
22. `Import Cost`: import할 때 패키지 용량 노출
23. `node-readme`: npm모듈 readme.md 바로 노출
24. `dotenv-autocomplete`: .env파일 내 환경변수 자동 완성
25. `vscode goto node_modules`: node_modules 내 경로 바로 탐색

### 에러 감지 관련
1. `Prettier`: 자동 정렬 및 코드 포멧팅
2. `Code Spell Checker`: 오타 방지
3. `Eslint`: 소스 코드 에러 감지
4. `Error Lens`: 코드에 에러가 있을 경우 바로 노출
5. `HTMLHint`: html 작성 오류 표시
6. `Stylelint`: css 구문 오류 감지
7. `scss-lint`: scss 문법 검사

### 테마 및 가독성 관련
1. `Bracket Pair Colorizer DLW`: 괄호 색 변경(`설정 -> 텍스트 편집기`에서 VSCode 내장 기능 사용 가능)
2. `Color Highlight`: 색깔 코드 배경색으로 노출
3. `Material Theme`: VSCode 테마 변경
4. `Material Icon Theme`: 파일/폴더 Icon Theme
5. `Active File In StatusBar`: 하단에 작업중인 파일의 path 표시
6. `Bracket Peek`: 해당 태그나 중괄호가 어느 코드에 포함되어 있는지 표시
7. `Output Colorizer`: Output 텍스트 색상을 입혀 가독성 높아짐
8. `Log File Highlighter`: 로그 파일에 색상을 입혀 가독성 높아짐
9. `Colorful Comments`: 주석색상 자유롭게 변경
10. `DotENV`: 환경변수 파일 하이라이팅

### Git 관련
1. `GitHub Repositories`: Git Hub에 올라간 프로젝트 바로 불러오기
2. `Git History`: git log 및 파일 히스토리, 브랜치/커밋 비교
3. `Git Lens`: 코드가 어떤 사람에 의해 작성된 내역인지 표시
4. `Diff Viewer`: diff 파일 가독성 있게 변환

### HTML/CSS 관련
1. `Auto Rename Tag`: HTML 태그 동시 수정
2. `Auto Close Tag`: HTML 태그 자동 닫기
3. `Highlight Matching Tag`: 특정 태그 쌍을 표시
4. `CSS Variable Autocomplete`: css 변수 자동 완성 기능
5. `SCSS IntelliSense`: scss 자동 완성
6. `CSS Peek`: ctrl + id/class 선택 시 css 탐색
7. `html tag wrapper`: html tag로 감싸는 기능 
8. `HTML CSS Support`: html 작성 시 존재하는 css class 자동 완성. css 프레임 워크 사용 시 유용
9. `HTML to CSS autocompletion`: 위와 반대로 css 작성 시 html에서 작성된 class 자동 완성
10. `HTML End Tag Labels`: html 마지막 닫는 태그에 클래스명 표시
11. `Html Auto Completion`: 자주 쓰이는 태그 자동 완성
12. `CSS Initial Value`: 마우스 오버 시 CSS 속성 기본값 노출
13. `SCSS Scope Helper`: scss에서 해당 중괄가 어느 선택자인지 표시

### Javascript/Typescript/Node/React 관련
1. `JavaScript (ES6) code snippets`: 다양한 자바스크립트 템플릿 사용 가능
2. `ES7+ React/Redux/React-Native snippets` : React 관련된 코드를 템플릿화하여 쉽게 사용
3. `Javascript Auto Backticks`: 문자열에서 `${}`를 감지하여 백틱( ` )으로 변경
4. `es6-string-html`: javascript 내에서 html 태그를 문자열로 사용 시 `/*html*/`을 붙이면 문자열을 하이라이팅
5. `JS Quick Console`: 변수를 드래그하고 `ctrl + shift + l`을 누르면 `console.log` 자동 생성
6. `Dot Log`: 변수.log 로 console.log 변수 매핑 `e.g. variable.log => console.log("variable", variable)`
7. `TypeScript Hero`: import 구문 정렬 및 사용 되지 않는 모듈 제거
8. `TypeScript Toolbox`: 타입스크립트 최적화 또는 자동 가져오기 등 많은 기능 제공
9. `Move TS`: typescript 프로젝트 이동 시 import 경로 자동 업데이트
10. `JSON to TS`: 복사된 JSON 코드를 Typescript 인터페이스로 간단 변환