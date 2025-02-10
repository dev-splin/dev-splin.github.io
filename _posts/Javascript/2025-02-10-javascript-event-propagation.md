---
title: "[Javascript] : 이벤트 전파란?"
excerpt_separator: <!--more-->
categories:
  - Javascript
  - "Event Propagation"
tags:
  - Javascript
  - "Event Propagation"
# 이미지 url (썸네일 필요한 경우 추가)
image: /assets/img/thumbnail/javascript-logo.png
# 기본 비노출 상태, 노출하고 싶은 경우 아래 옵션 제거
# published: false
---

이벤트 전파는 DOM에서 이벤트가 발생했을 때, 그 이벤트가 어떤 방식으로 전파되는지를 설명하는 개념입니다. 

이벤트 전파는 크게 세 단계로 나뉘는데, `캡처링(Capturing)`, `타겟(Target)`, 그리고 `버블링(Bubbling)` 입니다. <span class="highlighting-underline">각 단계를 거치면서 이벤트 리스너가 있으면 실행</span>되는데, 순서가 있는 캡처링/버블링 같은 경우에는 순서대로 실행되게 됩니다.

1. `캡처링`: 이벤트가 DOM 트리의 최상위 요소(document)에서 시작하여, 이벤트가 발생한 요소(타깃 요소)로 향해 내려가는 단계
2. `타겟`: 이벤트가 실제로 발생한 타겟 요소에 도달하는 단계
3. `버블링`: 타겟 요소에서 이벤트가 발생한 후, 다시 DOM 트리의 상위 요소들로 이벤트가 전파되어 올라가는 단계

기본적으로 대부분의 이벤트는 버블링을 통해 전파되지만, `addEventListener()`의 세 번째 인자로 `{ capture: true }`를 전달하면, 캡처링 단계에서도 이벤트를 처리할 수 있습니다.

이벤트 전파는 웹 페이지에서 요소 간의 상호작용을 제어하는 데 중요한 역할을 합니다. 다만 이러한 이벤트 전파가 정상 동작에 방해가 되는 경우, `event.stopPropagation()` 메서드를 사용하여 특정 단계에서 이벤트의 전파를 중단할 수 있습니다.

---

참고 링크: <https://www.maeil-mail.kr/question/39>