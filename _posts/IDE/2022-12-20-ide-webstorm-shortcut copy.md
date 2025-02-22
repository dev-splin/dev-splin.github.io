---
title: "[IDE] : WebStorm 유용한 단축키"
excerpt_separator: <!--more-->
categories:
  - IDE
  - Jetbrains
tags:
  - IDE
  - Jetbrains
  - WebStorm
  - Setting
toc: true
toc_sticky: true
toc_label: 목차
image: /assets/img/thumbnail/jetbrains-webstorm-logo.png
---

# Intellij 유용한 단축키

이 글은 intellij로 개발 시 유용한 단축키를 모아놓은 글입니다.

추후에 알게되는 단축키가 있다면 추가하겠습니다 :smile:



## 창 열기

| WINDOW                 | MAC               | 간단한 설명                       | 비고 |
| ---------------------- | ----------------- | --------------------------------- | ---- |
| Ctrl + Alt + S         | ⌘ + ,(컴마)       | Setting 창 열기                   | -    |
| Ctrl + Alt + Shift + S | ⌘ + ;(세미콜론)   | Project Structure 창 열기         | -    |
| Alt + 1                | ⌘ + 1             | 도구 창 열기                      | -    |
| Esc(도구 창 에서)      | Esc(도구 창 에서) | 도구 창 에서 에디터로 포커스 이동 | -    |





## 이동 및 검색

| WINDOW         | MAC       | 간단한 설명                               | 비고 |
| -------------- | --------- | ----------------------------------------- | ---- |
| Shift+Ctrl+A   | ⇧ + ⌘ + A | 모든 단축키 검색                          | -    |
| F2             | F2        | 다음 오류,경고,제안으로 이동              | -    |
| Ctrl + E       | ⌘ + E     | 최근 실행했던 파일 확인                   | -    |
| Ctrl + B       | ⌘ + B     | 커서에 해당하는 필드/메서드/클래스로 이동 | -    |
| Ctrl + Alt + B | ⌥ + ⌘ + B | 커서에 해당하는 메서드의 구현체로 이동    | -    |
| Alt + F7       | ⌘ + F7    | 커서에 해당하는 항목이 사용된 위치 검색   | -    |
| Shift + Shift  | ⇧ + ⇧     | 전체 검색                                 | -    |
| F3             | ⌘ + G     | 같은 단어 찾기                            | -    |
| Ctrl + G       | ⌘ + L     | 특정 라인으로 이동                        | -    |



## 수정/제안/실행

| WINDOW                              | MAC                    | 간단한 설명                                           | 비고                                  |
| ----------------------------------- | ---------------------- | ----------------------------------------------------- | ------------------------------------- |
| Alt + Enter                         | ⌘ + Enter              | 해당 라인/괄호 범위 내의 액션(수정,오류,경고 등) 노출 | -                                     |
| Ctrl + Ctrl                         | ⌃ + ⌃                  | 어떤 항목이든 실행                                    | 기본적으로 최근 실행 구성 목록 표시   |
| Shift + F6                          | ⇧ + F6                 | 같은 변수명 전체 변경                                 | -                                     |
| Ctrl + P                            | ⌘ + P                  | 커서가 위치한 메서드의 매개변수 조회                  | -                                     |
| Alt + Shift + Insert / Alt + 드래그 | ⌘ + ⇧ + 8 / ⌥ + 드래그 | 세로로 편집(종료 시에도 같은 키)                      | Shift(⇧) + 화살표 키로 커서 선택 가능 |
| Shift + Ctrl + U                    | ⌘ + ⇧ + U              | 대문자 -> 소문자, 소문자 -> 대문자로 변경             |                                       |



## 선택

| WINDOW                 | MAC       | 간단한 설명                                   | 비고 |
| ---------------------- | --------- | --------------------------------------------- | ---- |
| Ctrl + W               | ⌥ + ↑     | 선택 영역 확대                                | -    |
| Shift + Ctrl + W       | ⌥ + ↓     | 선택 영역 축소                                | -    |
| Alt + J                | ⌃ + G     | 한 번 누를 때 마다 다음 같은 단어 하나씩 선택 | -    |
| Ctrl + Alt + Shift + J | ⌃ + ⌘ + G | 같은 단어 모두 선택                           | -    |



## 자동 완성

| WINDOW               | MAC        | 간단한 설명                                                | 비고                                                                                        |
| -------------------- | ---------- | ---------------------------------------------------------- | ------------------------------------------------------------------------------------------- |
| Ctrl + Shift + Enter | ⇧ + ⌘ + ⏎  | 현재 구문 자동 완성                                        | for, if 문 등도 자동 완성 가능                                                              |
| Ctrl + Alt + L       | ⌥ + ⌘ + L  | 현재 파일의 서식 지정                                      | -                                                                                           |
| Alt + insert         | ⌘ + N      | 생성자, getter(), setter(), toString() 메서드 등 자동 생성 | -                                                                                           |
| Ctrl + Alt + V       | ⌥ + ⌘ + V  | 해당 메서드에 대한 반환 타입, 변수를 자동 작성             | new Member() 라고 입력 후 해당 단축키를 누르면 자동으로 변수를 만들어줌                     |
| Ctrl + Alt + T       | ⌥ + ⌘ + T  | 선택한 영역을 if/else, try/catch 등으로 감쌀 수 있음       | -                                                                                           |
| Ctrl + Alt + M       | ⌥ + ⌘ + M  | 입력한 메서드 명으로 메서드 생성                           | 어느 정도 메서드명 가이드도 잡아줌                                                          |
| Ctrl + Shift + O     | ^ + ⌥ + O  | 안쓰는 import 제거                                         | -                                                                                           |
| Ctrl + Shift + T     | ⇧ + ⌘ +  T | 테스트 케이스 생성                                         |                                                                                             |
| Alt + Enter          | ⌥  + ⏎     | 자동 완성 Help                                             | 커서에 위치하는 목록에 대해서 자동으로 완성해줌 (import, 메서드 생성, 리팩토링 추천 등)     |
| Ctrl + Alt + P       | ⌘ + ⌥ + P  | 커서에 위치하는 값 메서드 파라미터로 만듦                  |                                                                                             |
| Ctrl + Alt + N       | ⌥ + ⌘ + N  | 자동으로 inline으로 바꿔줌                                 | Ex) int를 반환하는 메서드에서<br />int a = 6 + 7; return a; -> return 6+7;<br />로 변경해줌 |





**참고 링크**

----

- https://mimah.tistory.com/entry/IntelliJ-IDEA-Window-%EB%8B%A8%EC%B6%95%ED%82%A4-%EC%A0%95%EB%A6%AC
- https://blog.jetbrains.com/ko/2020/03/11/top-15-intellij-idea-shortcuts_ko/
- https://velog.io/@jummi10/intellij-new-shortcut
- https://treasurebear.tistory.com/67
