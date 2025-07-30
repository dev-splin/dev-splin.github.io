---
title: "[최적화] : 블로그 서비스 최적화"
excerpt_separator: <!--more-->
categories:
  - PERFORMANCE
  - "Analysis Tools"
  - Lighthouse
tags:
  - PERFORMANCE
  - "Analysis Tools"
  - Lighthouse
# 이미지 url (썸네일 필요한 경우 추가)
# image: /assets/img/thumbnail/image.png
# 기본 비노출 상태, 노출하고 싶은 경우 아래 옵션 제거
published: false
---

## 초기 Lighthouse 검사 결과

![Lighthouse Result](https://i.imgur.com/Fps3YPq.png)
![Diagnostics](https://i.imgur.com/hpm2dtl.png)

Lighthouse 검사를 돌리면 위와 같은 결과를 받을 수 있습니다.
여기서 `Diagnostics`를 보면 어떤 부분을 개선해야하는 지 제안해주고 있는 것을 볼 수 있습니다.

## 이미지 사이즈 최적화

![Properly size images](https://i.imgur.com/1DyabsG.png)

먼저 `Properly size images`를 살펴보면 이미지를 적절한 사이즈로 변경하면 용량을 줄이고 로드 시간을 개선할 수 있다고 나옵니다.

![element size images](https://i.imgur.com/agTBcj5.png)

실제로 element의 이미지를 보면 `렌더링 사이즈`는 `120 X 120px`인 반면, 실제 사이즈는 `1200 X 1200px`로 매우 큰 것을 알 수 있습니다.
이 때, 화면에 표시되는 사이즈은 `120 X 120px`로 변경하면 된다고 생각할 수 있지만, 애플의 고밀도 디스플레이 `레티나 디스플레이`는 같은 공간에 더 많은 픽셀을 그릴 수 있기 때문에, <span class="highlighting-underline">너비 기준 두 배 정도 큰 이미지를 사용하는 것이 적절</span>합니다.

### 이미지 CDN

`CDN(Content Delivery Network)`이란 물리적 거리의 한계를 극복하기 위해 소비자(사용자)와 가까운 곳에 콘텐츠 서버를 두는 기술을 의미합니다.

> e.g. 미국에 있는 서버에서 이미지를 다운로드 하는 경우 물리적 거리가 멀기 때문에 시간이 오래걸려, 미국에 있는 서버를 미리 한국으로 복사해두고 사용자가 이미지를 다운로드 할 때 미국 서버가 아닌 한국 서버에서 다운로드하게 하는 것

<span class="highlighting-underline">이미지 CDN은 이미지를 사용자에게 보내기 전에 특정 형태로 가공하여 전해주는 기능</span>이 있습니다. 이를 이용해 특정 포맷으로 변경하는 등의 작업이 가능합니다.

따라서 다음과 같이 `query string`으로 이미지 사이즈를 조절해보겠습니다.

```javascript
function getParametersForUnsplash({width, height, quality, format}) {
  return `?w=${width}&h=${height}&q=${quality}&fm=${format}&fit=crop`
}

...

// AS-IS 1200 -> TO-BE 240
<img src={props.image + getParametersForUnsplash({width: 240, height: 240, quality: 80, format: 'jpg'})} alt="thumbnail" />
```

이후에 `Lighthouse`를 다시 돌려보면, 점수가 1점 올라가고 `Properly size images`도 `3,299KB -> 244KiB`로 확 줄어든 것을 볼 수 있습니다.

![Lighthouse Result-2](https://i.imgur.com/xCrAxN1.png)
![Diagnostics-2](https://i.imgur.com/JtgXvCn.png)
