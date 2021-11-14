---
title: "Web : HTTP 상태 코드"
excerpt_separator: <!--more-->
categories:
  - Web
tags:
  - Web
  - "Web : Status Code"
toc: true
toc_sticky: true
toc_label: 목차
---

# HTTP 상태 코드

웹 개발을 할 때에 요청과 응답에 따른 다양한 상태 코드가 있고, 이 상태 코드에 따라 다른 로직을 구현해야할 수도 있기 때문에 상태 코드를 알아야할 필요성이 있습니다.

주요 상태 코드는 200번대부터 500번대까지 다양하게 있지만 주요한 상태 코드만 몇 개 살펴보겠습니다.



## 2xx - 성공

200번대의 상태 코드는 대부분 성공을 의미합니다.

- 200 : GET 요청에 대한 성공
- 204 : No Content. 성공했으나 응답 본문에 데이터가 없음
- 205 : Reset Content. 성공했으나 클라이언트의 화면을 새로 고침하도록 권고
- 206 : Partial Conent. 성공했으나 일부 범위의 데이터만 반환



## 3xx - 리다이렉션

300번대의 상태 코드는 대부분 클라이언트가 이전 주소로 데이터를 요청하여 서버에서 새 URL로 리다이렉트를 유도하는 경우입니다.

- 301 : Moved Permanently, 요청한 자원이 새 URL에 존재
- 303 : See Other, 요청한 자원이 임시 주소에 존재
- 304 : Not Modified, 요청한 자원이 변경되지 않았으므로 클라이언트에서 캐싱된 자원을 사용하도록 권고. [ETag](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag)와 같은 정보를 활용하여 변경 여부를 확인



## 4xx - 클라이언트 에러

400번대 상태 코드는 대부분 클라이언트의 코드가 잘못된 경우입니다. 유효하지 않은 자원을 요청했거나 요청이나 권한이 잘못된 경우 발생합니다. 가장 익숙한 상태 코드는 404 코드입니다. 요청한 자원이 서버에 없다는 의미죠.

- 400 : Bad Request, 잘못된 요청
- 401 : Unauthorized, 권한 없이 요청. Authorization 헤더가 잘못된 경우
- 403 : Forbidden, 서버에서 해당 자원에 대해 접근 금지
- 405 : Method Not Allowed, 허용되지 않은 요청 메서드
- 409 : Conflict, 최신 자원이 아닌데 업데이트하는 경우. ex) 파일 업로드 시 버전 충돌



## 5xx - 서버 에러

500번대 상태 코드는 서버 쪽에서 오류가 난 경우입니다.

- 501 : Not Implemented, 요청한 동작에 대해 서버가 수행할 수 없는 경우
- 503 : Service Unavailable, 서버가 과부하 또는 유지 보수로 내려간 경우



---

참고 : [https://joshua1988.github.io/web-development/http-part1/#http-%EC%83%81%ED%83%9C-%EC%BD%94%EB%93%9C](https://joshua1988.github.io/web-development/http-part1/#http-%EC%83%81%ED%83%9C-%EC%BD%94%EB%93%9C)