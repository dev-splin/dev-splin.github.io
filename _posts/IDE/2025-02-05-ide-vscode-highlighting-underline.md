---
title: "[IDE] : 블로그 밑줄 효과 적용하기(+ VSCODE Snippets) "
excerpt_separator: <!--more-->
categories:
  - IDE
  - VSCODE
tags:
  - IDE
  - VSCODE
  - "Highlighting Underline"
# 이미지 url (썸네일 필요한 경우 추가)
image: /assets/img/thumbnail/visual-studio-code-logo.png
published: false
---

chripy 블로그 작성 시 <span class="highlighting-underline">이와 같이 다양한 스타일링 효과</span>를 적용할 수 있는데 적용 방법과 `vscode 내 snippets`을 활용하여 더 쉽게 작성하는 방법을 알아보겠습니다.

> 해당 글은 `chripy` GitHub 블로그 기준입니다.
{: .prompt-info }

## 스타일링 적용 방법

---

chripy 내 스타일링은 `/assets/css/jekyll-theme-chirpy.scss`에 설정할 수 있습니다. 맨 하단에 아래와 같은 스타일링일 넣어줍니다.

```scss
.highlighting-underline {
  // 색깔은 원하는 색으로 지정
  background: linear-gradient(transparent 60%, #1b99f3 80%);
  padding: 2px 4px;
}
```

해당 스타일링은 다음과 같이 사용할 수 있습니다.

```html
<span class="highlighting-underline">Highlighting Underline</span>
```

## vscode 내 snippets으로 더 쉽게 md파일 작성하기

---

vscode 내 `snippets`을 이용하여 단축키로 템플릿화 하여 md파일을 더 편하게 작성할 수 있습니다.

1. 명령어 창 열기 (`⇧⌘P`)
2. `Snippets : Configure User Snippets`검색
3. 원하는 확장자 선택(여기선 `markdown`)
4. `markdown.json`파일 다음과 같이 수정
5. 해당하는 확장자 파일에서 `단축키 + Tab`으로 템플릿 불러오기

```markdown
"Highlighting Underline": { // 템플릿 제목
  "prefix": "hu", // 단축키
  "body": ["<span class=\"highlighting-underline\">$CLIPBOARD</span>"], // 사용할 템플릿 $CLIPBOARD는 복사한 텍스트를 의미
  "description": "형광펜 밑 줄", // 설명
}
```

> 만약 `단축키 + Tab`이 작동하지 않는다면, 아래 방법을 따라 설정하면 됩니다.
> 1. 설정 창 열기 (`⌘,`)
> 2. `Editor: Tab Completion` 검색
> 3. on으로 변경
{: .prompt-danger }

## chripy prompt snippet 적용

---

> tip prompt
> ```markdown
> > tip prompt
> {: .prompt-tip }
> ```
{: .prompt-tip }

> info prompt
> ```markdown
> > info prompt
> {: .prompt-info }
> ```
{: .prompt-info }

> warning prompt
> ```markdown
> > warning prompt
> {: .prompt-warning }
> ```
{: .prompt-warning }

> danger prompt
> ```markdown
> > danger prompt
> {: .prompt-danger }
> ```
{: .prompt-danger }

`chiripy`에는 위와 같이 4개의 프롬프트를 지원하는데, md파일을 작성할 때, 각 프롬프트마다 특정 명령어를 입력해주어야 합니다.

이런 귀찮은 작업도 <span class="highlighting-underline">snippet을 이용하여 간소화</span>할 수 있습니다.

```json
{
  "Prompt Tip": {
    "prefix": "pt",
    "body": ["{: .prompt-tip }"],
    "description": "prompt-tip"
  },
  "Prompt Info": {
    "prefix": "pi",
    "body": ["{: .prompt-info }"],
    "description": "prompt-info"
  },
  "Prompt Warning": {
    "prefix": "pw",
    "body": ["{: .prompt-warning }"],
    "description": "prompt-warning"
  },
  "Prompt Danger": {
    "prefix": "pd",
    "body": ["{: .prompt-danger }"],
    "description": "prompt-danger"
  },
}
```

## 마치며

---

블로그 작성 뿐만 아니라 개발 시에도 `snippet`을 이용하면 개발 생산성을 높일 수 있을 것 같습니다. 평소에 반복해서 사용하는 코드가 있다면 snippet으로 등록해보는 건 어떨까요?