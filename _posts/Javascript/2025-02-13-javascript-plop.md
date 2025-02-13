---
title: "[Javascript] : plop 라이브러리로 디자인 시스템 컴포넌트 파일을 템플릿화 해보자!"
excerpt_separator: <!--more-->
categories:
  - Javascript
  - plop
tags:
  - Javascript
  - plop
# 이미지 url (썸네일 필요한 경우 추가)
image: /assets/img/thumbnail/javascript-logo.png
# 기본 비노출 상태, 노출하고 싶은 경우 아래 옵션 제거
# published: false
---

> 이 글로 얻을 수 있는 정보
> 1. plop를 이용한 파일 템플릿화
> 2. plop 라이브러리 설치 및 사용법
> 2. plop 템플릿
> 3. handlebars 템플릿
{: .prompt-tip }

## 0. 개요
디자인 시스템 컴포넌트를 구성하면서 스토리북 관련 구글링 중에 파일을 템플릿화 하여 쉽게 생성할 수 있는 plop 라이브러리를 알게되어 디자인 시스템 파일 템플릿 방법을 알아보고 템플릿을 공유해보려 합니다.

## 1. plop 라이브러리란?
**[plop](https://www.npmjs.com/package/plop)**라이브러리는 개요에서 말했듯이 <span class="highlighting-underline">파일을 템플릿화 하여 쉽게 생성</span>할 수 있게 도와주는 라이브러리 입니다.

plop은 프롬프트 라이브러리인 **[inquirer](https://www.npmjs.com/package/inquirer)**와 텍스트 형식을 생성하는 템플릿 언어를 사용해 템플릿을 만드는 **[handlebars](https://www.npmjs.com/package/handlebars)** 라이브러리로 만들어졌습니다. 즉, 프롬프트를 통해 정보를 입력받고 그 정보로 템플릿 언어로 템플릿을 만들어 파일을 생성해주는 것입니다.

## 2. plop 라이브러리 설치 및 사용법
자세한 내용은 **[plop 라이브러리 공식 홈페이지](https://www.npmjs.com/package/plop)**에서 보는 게 더 낫기 때문에 설치 방법과 사용법을 간략하게 핵심만 알아보겠습니다.

### 2-1. plop 라이브러리 설치
plop 라이브러리를 파일 만들 때만 사용하니, production 환경까지 반영되지 않아도 되기 때문에 dev로 설치해줍니다.

```typescript
// yarn
yarn add -dev plop

// npm
npm install --save-dev plop
```

### 2-2. plop script 등록 및 type 설정
<span class="highlighting-underline">plop을 사용하기 위한 script와 ESM(ECMAScript Modules)를 사용하기 위해 type을 지정</span>해줍니다. (CJS(CommonJS)를 사용하려면 type을 지정해주지 않아도 괜찮습니다.)

> CJS(CommonJS), ESM(ECMAScript Modules)란?
> 
> 파일 모듈화를 진행하기 위한 모듈 시스템을 일컫습니다. 따로 package.json에 설정하지 않으면 CJS가 기본입니다.<br/>
> `CJS는 require / module.exports`를 사용하고, `ESM은 import/export` 문을 사용해서 파일 모듈화를 진행할 수 있습니다.<br/>
> CJS/EMS와 package.json의 type, exports 관심이 있으시다면 관련 다음 글을 읽어봐도 좋습니다.<br/>
> [토스 - CommonJS와 ESM에 모두 대응하는 라이브러리 개발하기: exports field](https://toss.tech/article/commonjs-esm-exports-field)
{: .prompt-info }

```typescript
// package.json
{
  ...
  "type": "module",
  "scripts": {
    ...
    "plop": "plop"
  }
}
```

### 2-3. plopfile.js 작성
폴더의 root 경로에 `plopfile.js`를 만들고 다음과 같이 <span class="highlighting-underline">setGenerator를 사용하여 프롬프트를 구성하고 템플릿 파일을 이용해 파일들을 템플릿화</span> 할 수 있습니다.

setGenerator의 prompots 관련 옵션은 **[inquirer](https://www.npmjs.com/package/inquirer)**에서 actions의 templateFile 관련 옵션은 **[handlebars](https://www.npmjs.com/package/handlebars)**에서 확인할 수 있습니다.

더 자세한 내용은 다음에서 이어집니다.

```typescript
export default function (plop) {
	// setGenerator로 프롬프트 구성과 파일 템플릿화를 진행할 수 있습니다.
	plop.setGenerator('generator name', {
		description: 'generator description',
		prompts: [], // inquirer를 이용해 prompts를 구성할 수 있습니다.
		actions: []  // actions 관련 옵션은 plop에서 actions내부에 templateFile 관련해서는 handlebars에서 확인할 수 있습니다.
	});
};
```

### 2-4. plop 실행
script를 지정한 plop 명령어를 통해 구성한 plop을 실행하여 설정한 프롬프트를 보여주고 파일을 생성할 수 있습니다.

```typescript
// yarn
yarn plop

// npm
npm plop
```

![plop](https://camo.githubusercontent.com/7bca9ee3b02142d21a35ded0d74eac47fc76aebdba2dbb9eb9a9798e34ad13ba/68747470733a2f2f692e696d6775722e636f6d2f70656e55466b722e676966)

## 3. plop 템플릿 공유 및 꿀팁
팀에서는 Next.js14를 사용하고 있으며, 만들고 있는 디자인 시스템 컴포넌트의 기본적인 파일 구조는 다음과 같이 이루어져 있어 필요한 부분은 수정하여서 사용하면 좋을 것 같습니다.

```typescript
Component
ㄴindex.tsx
ㄴtype.d.ts
ㄴindex.modules.scss
ㄴindex.stories.tsx
```

### 3-1. plop파일
plopfile.js에서 눈에 띄는 부분과 관련 링크를 적어놓겠습니다.

- `plop.setHelper`: handlebars에서 사용하기 위한 메서드 여기에서는 propmts 폴더 선택을 도와주는 라이브러리를 사용 - <https://handlebarsjs.com/guide/#custom-helpers>
- `plop.setPrompt`: 사용자들이 plop을 이용해 만든 다양한 라이브러리를 등록해서 사용할 수 있음 - <https://github.com/plopjs/awesome-plop>

위에서 <span class="highlighting-underline">handlebars 라이브러리는 {{ }} 내부에 있는 값들을 치환하여 파일을 만들어줄 수 있습니다.</span> handlebars 내부에서 사용할 수 있는 함수 조건문 등을 사용할 수도 있는데, 공식 문서가 잘 되어 있어 공식 문서를 참고하시면 되겠습니다.

```javascript
// plopfile.js

import inquirer from "inquirer";
import inquirerDirectory from "inquirer-directory";

export default function (plop) {
  // hbs 템플릿(handlebars)에서 사용할 포함 여부 함수 정의
  plop.setHelper("includes", function (arr, values) {
    const valueList = values.split(",");

    return valueList.some((value) => arr.includes(value));
  });

  // 폴더 선택 라이브러리 적용
  plop.setPrompt("directory", inquirerDirectory);
  // prompt 생성
  plop.setGenerator("design-system-ui", {
    description: "Create design system ui",
    prompts: [
      // 폴더 선택(setPrompt에서 적용한 폴더 선택 라이브러리 사용)
      {
        type: "directory",
        name: "path",
        message: `1. 컴포넌트를 생성할 폴더를 선택해주세요 (⬆️ 버튼을 누르면 빠르게 선택(choose this directory) 할 수 있어요)`,
        basePath: "ui",
      },
      // 컴포넌트 이름 입력
      {
        type: "input",
        name: "name",
        message: "2. 컴포넌트를 이름을 입력해주세요",
      },
      // 스토리북 옵션 선택
      {
        type: "checkbox",
        message: "3. 스토리북 Meta에 사용할 옵션을 선택해주세요",
        name: "options",
        loop: false,
        choices: [
          new inquirer.Separator("====== 기본 옵션 ======"),
          {
            value: "args",
            name: "args ",
            disabled: "컴포넌트 props 값 설정",
          },
          new inquirer.Separator("====== 선택 옵션 ======"),
          {
            value: "storyHeight",
            name: "story.height (Story가 보여질 영역 높이 조절)",
          },
          {
            value: "sourceCode",
            name: "source.code (Story에 보여질 source code 관련 설정)",
          },
          {
            value: "argTypes",
            name: "argTypes (컴포넌트 props의 type관련 내용 설정)",
          },
          {
            value: "renderOrDecorators",
            name: "render or decorators (Story 렌더링 마크업,스타일링,동작 제어)",
          },
        ],
      },
    ],
    // templateFile에서 prompt로 입력한 정보를 받아 파일을 생성
    actions: [
      // index.tsx 생성
      {
        type: "add",
        path: "ui/{{path}}/{{pascalCase name}}/index.tsx",
        templateFile: "plop-templates/design-system-ui/Component.tsx.hbs",
      },
      // type.d.ts 생성
      {
        type: "add",
        path: "ui/{{path}}/{{pascalCase name}}/type.d.ts",
        templateFile: "plop-templates/design-system-ui/Type.d.ts.hbs",
      },
      // index.module.scss 생성
      {
        type: "add",
        path: "ui/{{path}}/{{pascalCase name}}/index.module.scss",
      },
      // index.stories.tsx 생성
      {
        type: "add",
        path: "ui/{{path}}/{{pascalCase name}}/index.stories.tsx",
        templateFile: "plop-templates/design-system-ui/Story.tsx.hbs",
      },
    ],
  });
}
```

### 3-2. handlebars 템플릿 파일
plop파일에서 정의한 `includes helper`를 사용하는 것을 볼 수 있고 **if문을 통하여 템플릿을 조건부로 적용**하였습니다.

> handlebars에서 helper 사용법
> 
> 기본 적으로 **\{\{ 헬퍼이름 메서드인자 \}\}** 형식으로 사용하며, 조건문 안에 넣을 때는 괄호로 감싸서( **\{\{#if (includes options "storyHeight,sourceCode")\}\}** ) 사용하게 됩니다.<br/>
> e.g. **\{\{ pascalCase name \}\}** : pascalCase는 handlebars에서 기본으로 제공하는 메서드 인데, prompt에서 받은 name 값을 pascalCase 메서드를 통해 pascalCase로 만들어줍니다.
{: .prompt-info }

#### Story.tsx.hbs

```javascript
import {{pascalCase name}} from "@design-system/ui/{{path}}/{{pascalCase name}}";
import type { Meta, StoryObj } from "@storybook/react";

/**
 * 여기에 해당 컴포넌트에 대한 설명을 적어주세요. 미기재 시 {{pascalCase name}} 컴포넌트의 JSDoc이 노출됩니다.
 * (Storybook에서 parameters.docs.description.component 보다 JSDoc을 권장합니다.)
 * @see {https://storybook.js.org/docs/api/doc-blocks/doc-block-description#writing-descriptions}
 */
const meta: Meta<typeof {{pascalCase name}}> = {
  component: {{pascalCase name}},
  {{#if (includes options "storyHeight,sourceCode")}}
  parameters: {
    docs: {
      {{#if (includes options "storyHeight")}}
      story: {
        // (Optional) story가 보여질 영역 높이를 조절할 수 있습니다.
        height: "200px",
      },
      {{/if}}
      {{#if (includes options "sourceCode")}}
      source: {
        /**
         * (Optional)
         * storybook에 노출되는 source code를 직접 작성할 수 있습니다. (dedent를 사용해 깔끔하게 작성하는 게 좋습니다.)
         *
         * code와 아래의 transform 둘 다 작성하면 transform은 무시됩니다.
         */
        code: dedent`
          const [state, setstate] = useState();

          return (
          <Component>
            <SubComponent/>
          </Component>
          );`,
        /**
         * (Optional)
         * storybook에 노출되는 source code를 변환할 수 있습니다.
         *
         * 합성 컴포넌트인 SubComponent를 표현하려고 Meta.component에 Component.SubComponent로 작성해주어도
         * source code에는 SubComponent로 노출되기 때문에 합성 컴포넌트를 표현할 때 사용할 수 있습니다.
         */
        transform: (code: string) =>
          code.replaceAll("SubComponent", "Component.SubComponent"),
      },
      {{/if}}
    },
  },
  {{/if}}
  {{#if (includes options "argTypes")}}
  argTypes: {
    props1: {
      // (Optional) 미기재 시 해당 prop의 JSDoc이 노출됩니다.
      description: "여기에 props1의 설명을 적어주세요.",
      table: {
        // description 아래 위치하며, type을 나타내줍니다
        type: {
          summary: "default 값을 표시해 줄 수 있습니다.",
          detail: "detail은 summary를 누르면 노출됩니다.",
        },
        /**
         * (Optional)
         * args에 props1이 있고 defaultValue가 정의되어 있지 않으면 args에 정의된 값이 default 값으로 노출됩니다.
         */
        defaultValue: {
          summary: "default 값을 표시해 줄 수 있습니다.",
          detail: "detail은 summary를 누르면 노출됩니다.",
        },
        /**
         * (Optional)
         * props1에 category를 정의하면 props1이 기존 위치가 아닌 토글로 된 category에 노출됩니다.
         * subcategory는 정의한 category 아래에 표시됩니다.
         * 합성 컴포넌트나, 중첩된 props를 표시하고 싶을 때 사용할 수 있습니다.
         */
        category: "category",
        subcategory: "subcategory",
        // (Optional) Args Table에서 props1을 제거합니다.
        disable: true,
        // (Optional) Args Table에서 props1이 읽기 전용임을 나타냅니다.
        readonly: true,
      },
      /**
       * (Optional)
       * Args Table에서 사용자 조작에 관한 설정을 넣을 수 있습니다.
       * 특정 type(select, radio 등..)의 경우에 options 값이 필요합니다.
       * @see {https://storybook.js.org/docs/api/arg-types#control}
       */
      control: "select",
      options: ["option1", "option2"],
    },
  },
  {{/if}}
  // (Optional) Meta에서 args에 입력한 값은 Args Table에 default 값으로 노출됩니다.
  args: {
    props1: "여기에 props1 타입에 맞는 값을 입력해주세요",
  },
  {{#if (includes options "renderOrDecorators")}}
  /**
   * (Optional) render or decorators
   * Storybook에 렌더링 될 컴포넌트에 추가로 마크업/스타일링을 하거나 동작 제어가 필요할 때 사용합니다.
   *
   * storybook 내 이벤트 발생 시 args를 변경할 때 useArgs Addon과 함께 사용하면 좋습니다.
   * @see {https://storybook.js.org/docs/writing-stories/args#setting-args-from-within-a-story}
   */
  // 하나의 컴포넌트를 렌더링할 때 주로 사용됩니다.
  decorators: [
    (Story, context) => {
      return <Story {...context} args={context.args} />;
    },
  ],
  // 여러 개의 컴포넌트나 합성컴포넌트를 렌더링할 때 주로 사용됩니다.
  render: (args) => {
    return (
      <>
        <Component>
          <SubComponent />
        </Component>
        <Component>
          <SubComponent />
        </Component>
      </>
    );
  },
  {{/if}}
};

export default meta;

type Story = StoryObj<typeof {{pascalCase name}}>;

/**
 * 여기에 해당 Story에 대한 설명을 적어주세요
 * Storybook에서 parameters.docs.description.story 보다 JSDoc을 권장합니다.
 * @see {https://storybook.js.org/docs/api/doc-blocks/doc-block-description#writing-descriptions}
 */
export const Default: Story = {
  args: { props1: "option1" },
};
```

#### Component.tsx.hbs

```javascript
import classNames from "classnames/bind";
import type { {{pascalCase name}}Props } from "@design-system/ui/{{path}}/{{pascalCase name}}/type";
import style from "@design-system/ui/{{path}}/{{pascalCase name}}/index.module.scss";

const cx = classNames.bind(style);

function {{pascalCase name}}(props: {{pascalCase name}}Props) {
  return <div />;
}

export default {{pascalCase name}};
```

#### Type.d.ts.hbs

```javascript
export interface {{pascalCase name}}Props {}
```

## 마치며
동료들과 개발할 때, 어느 정도 규칙을 만들며 반복되는 업무를 자동화하고 특정 부분들을 템플릿화 하는 것이 생산성을 높여줄 수 있다고 생각합니다. 이 plop 라이브러리뿐만 아니라 다양한 라이브러리를 잘 활용하면 생산성을 많이 높여줄 수 있을 것 같습니다.

더 좋은 plop 템플릿을 구성하시거나 다른 좋은 방법이 있으시다면 다양한 피드백으로 공유해주시면 감사하겠습니다 🙏