---
title: "[React] : 비동기 데이터 캐싱/연관 데이터 자동 갱신을 위한 RTK(ReduxToolKit) Query"
excerpt_separator: <!--more-->
categories:
  - React
  - "RTK Query"
tags:
  - React
  - "RTK Query"
  - "ReduxToolKit Query"
# 이미지 url (썸네일 필요한 경우 추가)
image: /assets/img/thumbnail/rtk-query-logo.png
---

## 개요

사내 프로젝트 개발 중 전역적으로 정보가 필요한 비동기 데이터(워크스페이스, 채널, 유저 정보 등등...)를 다룰 때, 해당 비동기 데이터를 캐싱할 필요성이 있었습니다.

`React-query`와 `Redux ToolKit Query` 중 어느 것을 적용할지에 대한 고민을 했는데, 기존 프로젝트 프론트 단에서 이미 RTK를 사용하고 있고 기능도 비슷해서 `Redux ToolKit Query`를 적용하기로 결정하였습니다. (사용하다보니, axios 대신 사용해도 괜찮을 거 같다는 생각을 하긴 했습니다만,,,,)

해당 글은 공식문서를 참고하여 `Redux ToolKit Query` 적용 및 유용한 기능에 대하여 기재한 글입니다.

지속적으로 업데이트 예정이며, 언급되지 않은 옵션도 많으니 필요한 기능이 있다면 아래의 Redux Toolkit 공식문서를 살펴보는 것을 추천합니다

- RTK Query 사용가이드 : <https://redux-toolkit.js.org/rtk-query/overview>
- RTK Query API 가이드: <https://redux-toolkit.js.org/rtk-query/api/createApi>


## 1. RTK(Redux ToolKit) Query란?

---

`Redux ToolKit(이하 RTK) Query`는 웹 애플리케이션에서 데이터를 로드하는 일반적인 경우를 단순화하여 데이터 로드 및 캐싱을 간편하게 사용할 수 있게 도와주는 라이브러리입니다.

또한, 기존에 Redux로 상태를 관리하였지만, 몇 년 동안 React 커뮤니티를 통해 상태관리와 데이터 로드 및 캐싱을 다른 관심사로 보고 제작한 라이브러리입니다.

RTK Query를 사용하면 아래 동작들을 손쉽게 할 수 있습니다.

- UI 스피너를 표시하기 위한 로드 상태 추적
- 동일한 데이터에 대한 중복 요청 방지
- UI가 더 빠르게 느껴지도록 업데이트
- 사용자가 UI와 상호 작용할 때의 캐시 수명 관리


## 2. RKT Query 구조 및 Store 등록 방법

---

RTK Query는 이름과 같이 Redux Toolkit으로 만들어졌기 때문에, <span class="highlighting-underline">기본 RTK의 store에 등록하는 방법이 유사</span>합니다. (RTK Query에 관한 글이기 때문에 RTK에 관한 설명은 제외하겠습니다.)

RTK처럼 createSlice()로 슬라이스를 만드는 것이 아닌, `createApi` 메서드를 이용해 만들게 됩니다

```javascript
export const apiSlice = createApi({ 
  reducerPath: 'api', // store에 등록될 Api 고유 키
  baseQuery: refreshFetchBase, // endpoints에 기본으로 사용될 baseQuery
  endpoints: (builder) => ({ // api를 기재하는 영역
  tagTypes: ['Workspace'], // 문자열 태그 배열. 해당 태그로 연관 데이터 갱신 가능
    ... 
  }),
});
```

### 2-1. endpotins 분할

기본 적으로 RTK Query는 한 곳에서 Api를 정의하게 됩니다. 하지만 서비스 크기가 커지면 API가 많아져 파일이 점점 커지면서 관리가 어려워 질 것입니다.

때문에 <span class="highlighting-underline">injectEndpoints를 사용하여 연관된 범위 별(보통 tag 별)로 endpoints</span>를 나누는 게 좋습니다. (아쉽게도 createApi 에 정의된 기본 설정은 변경할 수 없다고 합니다.)

**injectEndpoints**

기존 api 슬라이스에 존재하는 endpoints와 같은 이름으로 endpoints를 정의한다면 override합니다. (다만, overrideExisting을 true로 설정하지 않으면, 재정의 되지 않고 경고 표시를 노출합니다. 기본 false)

```javascript
export const workspaceEndpoints = apiSlice.injectEndpoints({ 
  endpoints: (builder) => ({
     ... 
    }), 
    // overrideExisting: true, 정의하지 않으면 false 
});
```

### 2-2. store에 등록

위에 주석에 기재된 것 처럼 <span class="highlighting-underline">Api 고유 키를 이용하여 store에 api를 등록</span>할 수 있습니다.

```javascript
export default configureStore({ 
  reducer: {
    ...   
    [apiSlice.reducerPath]: apiSlice.reducer, // reducerPath(키) : reducer(값)방식으로 등록
     
    ... 
  }, 
  middleware: (getDefaultMiddleware) => getDefaultMiddleware({ serializableCheck: false }).concat(apiSlice.middleware), // middleware로 연결해 줌 
});
```

## 3. endpoints 정의 방법 및 사용법

---

자주 사용되는 endpoints 정의하는 방법은 아래와 같습니다.

간단하게 주석을 작성하였으며, 자세한 내용은 아래에서 확인하면 되겠습니다. (주석으로 어느 부분을 봐야하는지 표시하였습니다.

```javascript
export const transformResponse = (baseQueryReturnValue, meta, arg) => baseQueryReturnValue.body;
  
// 에러 notify 노출
const showErrorNotify = (response, message) => {
  notify.error(message);
 
  return response;
};
 
 
  
export const workspaceEndpoints = apiSlice.injectEndpoints({
  endpoints: (build) => ({
  
    ...
  
    // 워크스페이스 정보 조회
    fetchWorkspace: build.query({
      query: (workspaceNo) => `/workspaces/${workspaceNo}`, // api 정의 -> 2-1. build의 query, mutation 참고
      providesTags: ['Workspace'], // invalidatesTags에 같은 태그가 붙은 메서드를 실행 시 해당 api가 갱신 -> 2-2. tag시스템 참고
      transformResponse, // response를 변형시킬 수 있음 -> 2-3. custom response 참고
      transformErrorResponse: (response) => showErrorNotify(response, '워크스페이스 정보를 가져오지 못했어요'), // error response를 변형시킬 수 있음 -> 2-3. custom response 참고
    }),
    // 워크스페이스 정보 변경
    updateWorkspace: build.mutation({
      query: ({ workspaceNo, ...data} ) => ({
        url: `/workspaces/${workspaceNo}`,
        method: 'PUT',
        body: data,
      }),
      invalidatesTags: ['Workspace'], // 해당 태그가 붙은 메서드를 실행 시 providesTags에 같은 태그가 붙은 api를 갱신 -> 2-2. tag시스템 참고
    }),
    // 워크스페이스 멤버 수 조회
    fetchWorkspaceMembersCount: build.query({
      query: (workspaceNo) => `/workspaces/${workspaceNo}/members/count`,
      providesTags: [{ type: 'Workspace', id: tags.membersCount }],
      transformResponse,
      transformErrorResponse: (response) => showErrorNotify(response, '워크스페이스 멤버 정보를 가져오지 못했어요'),
    }),
 
    ...
  
  }),
});
  
// react에서 사용할 수 있는 hook 형식으로 export, RTK Query가 hook을 제공
// use + 메서드 이름 + Query/Mutation 으로 이루어짐
// 2-1. build의 query, mutation 참고
export const {
  useFetchWorkspaceQuery,
  useUpdateWorkspaceMutation,
  useLazyFetchWorkspaceMembersCountQuery, // Lazy 즉 수동으로 데이터를 제공
  ...
}
```

### 3-1. build의 query(데이터 조회), mutation(데이터 변경)
데이터 조회 및 변경 시 endpoints의 `query(조회)와 mutation(변경)` 메서드사용할 수 있습니다.

해당 api를 요청할 때, RTK Query가 제공해주는 hook을 사용하는데, <span class="highlighting-underline">api에 여러개의 parameter가 필요하다면 반드시 객체로 제공</span>하여야 합니다.

#### 3-1-1. query
데이터 조회 시 사용하는 `endpoints build 메서드`입니다. javascript fetch 메서드를 이용한 fetchBaseQuery로 이루어져 있다고 합니다.

해당 build 메서드로 endpoints를 정의하고 <span class="highlighting-underline">RTK Query가 제공해주는 조회 hook을 사용하여 해당 API 데이터를 사용</span>할 수 있습니다.

RTK Query가 제공해주는 query 관련 hook은 아래와 같이 5가지가 있는데, 여기서는 `useQuery, useLazyQuery`만 다루겠습니다. 

- `useQuery` : api 데이터 요청 or 캐시 데이터 조회
- `useLazyQuery` : api 데이터 조회를 수동으로 제어
- `useQueryState` : 요청 상태 및 캐시된 데이터를 조회
- `useQuerySubscription` : 재조회 및 캐시 데이터 구독
- `useLazyQuerySubscription` : 재조회 및 캐시 데이터 구독을 수동으로 제어

**useQuery**

useQuery는 <span class="highlighting-underline">use + endpoints이름 + Query 형식으로 제공</span>됩니다. hook의 parameter에 들어갈 값과 return값은 아래와 같습니다.

- useQuery hook의 parameter : api에 사용될 parameter와 queryOptions을 넣을 수 있습니다.
- useQuery hook의 return : 객체로 반환 isLoading, isSuccess같은 요청상태 값과 data,error, 시작/종료시간 등 data값을 얻을 수 있습니다.

기본적인 사용법은 아래와 같습니다.

```javascript
// workspaceEndpoints.js
export const workspaceEndpoints = apiSlice.injectEndpoints({
  endpoints: (build) => ({
    // 워크스페이스 정보 조회
    fetchWorkspace: build.query({ // query 메서드로 api 정의
      query: (workspaceNo) => `/workspaces/${workspaceNo}`,
      providesTags: ['Workspace'],
      transformResponse,
      transformErrorResponse: (response) => showErrorNotify(response, '워크스페이스 정보를 가져오지 못했어요'),
    }),
  }),
});
  
export const {
  useFetchWorkspaceQuery, // RTK Query hook 추출
}
 
 
 
// WorkspaceInfo.jsx
const WorkspaceInfo = ({ workspaceNo }) => {
 
  ...
  
  // 자동으로 api를 요청,
  // api.endpoints.fetchWorkspace.useQuery(workspaceNo) 형식으로 사용 가능
  const { data: workspace = [], isSuccess } = useFetchWorkspaceQuery(workspaceNo); // hook의 파라미터로 api에 사용될 파라미터를 전달
}
```

기존에 axios를 활용한 비동기 요청은 데이터를 넣을 state가 필요했는데, <span class="highlighting-underline">RTK Query의 hook을 사용하면 위와 같이 hook을 생성한 후 data를 가져다 사용</span>하면 됩니다. (추가로 isLoading 같은 상태값도 사용할 수 있음)

RTK Query의 parameter, return 값은 아래 링크에서 확인할 수 있습니다.

- <https://redux-toolkit.js.org/rtk-query/api/created-api/hooks#usequery>

**useLazyQuery**

useLazyQuery는 수동으로 제어할 수 있기 때문에 실행 메서드를 제공합니다.

hook의 형식은 <span class="highlighting-underline">useLazy + endpoints이름 + Query 형식으로 제공</span>됩니다.

hook의 parameter에 들어갈 값과 return값은 아래와 같습니다.

- useLazyQuery hook의 parameter : api 실행 시의 queryOptions을 넣을 수 있습니다.
- useLazyQuery hook의 return : 배열로 반환하며, [ 실행 메서드, 결과 객체, lastPromiseInfo ] 로 되어있습니다.

기본적인 사용법은 아래와 같습니다.

```javascript
// workspaceEndpoints.js
export const workspaceEndpoints = apiSlice.injectEndpoints({
  endpoints: (build) => ({
    // 워크스페이스 멤버 수 조회
    fetchWorkspaceMembersCount: build.query({ // query 메서드로 api 정의
      query: (workspaceNo) => `/workspaces/${workspaceNo}/members/count`,
      providesTags: [{ type: 'Workspace', id: tags.membersCount }],
      transformResponse,
      transformErrorResponse: (response) => showErrorNotify(response, '워크스페이스 멤버 정보를 가져오지 못했어요'),
    }),
  }),
});
  
export const {
  useLazyFetchWorkspaceMembersCountQuery, // RTK LazyQuery hook 추출
}
 
 
// WorkspaceAuthorityManage.jsx
const WorkspaceAuthorityManage = ({ workspaceNo, setSelectedMember }) => {
 
  ...
  
  // 해당 실행 메서드를 실행함으로써 api를 요청 (자동으로 api를 요청 X)
  // api.endpoints.fetchWorkspaceMembersCount.useLazyQuery() 형식으로 사용 가능
  const [fetchWorkspaceMembersCount] = useLazyFetchWorkspaceMembersCountQuery();
  const [fetchInvitedMembers] = useLazyFetchInvitedMembersQuery();
  
  const moveMemberInvite = useCallback(async () => {
    const [workspaceMembersCount, invitedMembers] = await axios.all([
      fetchWorkspaceMembersCount(workspaceNo).unwrap(), // unwrap()을 사용하지 않으면 결과 객체가 반환됨
      fetchInvitedMembers(workspaceNo).unwrap(),
    ]);
  
    ...
  
  }
}
```

useLazyQuery를 활용하면 자동으로 api를 요청했는데, <span class="highlighting-underline">useLazyQuery를 사용한다면, 실행메서드 실행 시점에 api를 요청</span>하게 됩니다.

> 이 때, unwrap() 메서드를 사용하지 않으면 요청의 response가 반환되지 않고 isFetching, data 등등이 들어있는 useLazyQuery의 결과 객체가 반환됩니다.
{: .prompt-danger }

RTK LazyQuery의 parameter, return 값은 아래 링크에서 확인할 수 있습니다.

- <https://redux-toolkit.js.org/rtk-query/api/created-api/hooks#uselazyquery>

#### 3-1-2. mutation
데이터 변경 시 사용하는 endpoints build 메서드입니다.

해당 build 메서드로 endpoints를 정의하고 RTK Query가 제공해주는 변경 hook을 사용하여 해당 API 데이터를 사용할 수 있습니다.

RTK Query가 제공해주는 변경 hook은 `useMutation` 하나 뿐입니다.

**useMutation**

useMutation는 <span class="highlighting-underline">use + endpoints이름 + Mutation 형식으로 제공</span>됩니다.hook의 parameter에 들어갈 값과 return값은 아래와 같습니다.

- useMutation hook의 parameter : api 실행 시의 queryOptions을 넣을 수 있습니다.
- useMutation hook의 return : 배열로 반환하며, [ 실행 메서드, 결과 객체 ] 로 되어있습니다.

기본적인 사용법은 아래와 같습니다.

```javascript
// workspaceEndpoints.js
export const workspaceEndpoints = apiSlice.injectEndpoints({
  endpoints: (build) => ({
    // 워크스페이스 정보 변경
    updateWorkspace: build.mutation({ // mutation 메서드로 api 정의
      query: ({ workspaceNo, ...data }) => ({ // 여러 데이터가 있다면 객체 파라미터 제공
        url: `/workspaces/${workspaceNo}`,
        method: 'PUT',
        body: data,
      }),
      invalidatesTags: ['Workspace'],
    }),
  }),
});
  
export const {
  useUpdateWorkspaceMutation, // RTK Mutation hook 추출
}
 
 
// WorkspaceInfo.jsx
const WorkspaceInfo = ({ workspaceNo }) => {
  
  // 해당 실행 메서드를 실행함으로써 api를 요청
  // api.endpoints.updateWorkspace.useMutation() 형식으로 사용 가능
  const [updateWorkspace] = useUpdateWorkspaceMutation();
 
  ...
  
  const onUpdateWorkspace = async (data) => {
    const params = {
      workspaceNo: data.no,
      companyName: data.name,
      magic: data.magic,
      profileColorValue: data.profileColorValue,
    };
    try {
      await updateWorkspace(params).unwrap(); // unwrap()을 사용하지 않으면 결과 객체가 반환됨
      notify.info('워크스페이스 정보가 변경되었습니다.');
      closeModal('profileSettings');
    } catch (error) {
      notify.error('워크스페이스 정보 수정에 실패하였습니다.');
    }
  }
}
```

<span class="highlighting-underline">useMutation의 실행메서드 실행 시점에 api를 요청</span>하게 됩니다.

> 이 때, unwrap() 메서드를 사용하지 않으면 요청의 response가 반환되지 않고 isFetching, data 등등이 들어있는 useMutation의 결과 객체가 반환됩니다.
{: .prompt-danger }

RTK MutationQuery의 parameter, return 값은 아래 링크에서 확인할 수 있습니다.

- <https://redux-toolkit.js.org/rtk-query/api/created-api/hooks#usemutation>

### 3-2. tag 시스템(연관 데이터 자동 갱신)
기본적으로 RTK Query는 캐시 데이터가 존재하지 않는 경우에만 API 요청이 전송됩니다. 하지만 Mutation을 통하여 데이터를 변경할 때, 연관 데이터를 재요청해야합니다.

이런 경우에 <span class="highlighting-underline">tag 시스템을 사용하면 연관 데이터를 자동으로 갱신</span>할 수 있습니다. (RTK Query에서는 각 <span class="highlighting-underline">endpoint + 매개변수 조합으로 CacheKey를 만들어, 특정 캐시 데이터를 구분</span>할 수 있습니다.)

```javascript
// tag의 type
export const tagTypes = {
  workspace: 'Workspace',
  evaluationTemplate: 'EvaluationTemplate',
};
  
export const apiSlice = createApi({
  reducerPath: 'api',
  baseQuery: refreshFetchBase,
  tagTypes: [Object.values(tagTypes)], // ['Workspace', 'EvaluationTemplate']과 같음. 문자열 태그 배열. 해당 태그로 연관 데이터 갱신 가능
  endpoints: (builder) => ({
    // 워크스페이스 정보 조회
    fetchWorkspace: build.query({
      query: (workspaceNo) => `/workspaces/${workspaceNo}`,
      providesTags: [workspace], // invalidatesTags에 같은 태그가 붙은 메서드를 실행 시 해당 api가 갱신
      transformResponse,
      transformErrorResponse: (response) => showErrorNotify(response, '워크스페이스 정보를 가져오지 못했어요'),
    }),
    // 워크스페이스 정보 변경
    updateWorkspace: build.mutation({
      query: ({ workspaceNo, ...data }) => ({
        url: `/workspaces/${workspaceNo}`,
        method: 'PUT',
        body: data,
      }),
      invalidatesTags: [workspace], // 해당 태그가 붙은 메서드를 실행 시 providesTags에 같은 태그가 붙은 api를 갱신
    }),
    // 워크스페이스 멤버 수 조회
    fetchWorkspaceMembersCount: build.query({
      query: (workspaceNo) => `/workspaces/${workspaceNo}/members/count`,
      providesTags: [{ type: workspace, id: tagIds.membersCount }], // id를 지정함으로써 캐시 데이터를 디테일하게 구분 가능
      transformResponse,
      transformErrorResponse: (response) => showErrorNotify(response, '워크스페이스 멤버 정보를 가져오지 못했어요'),
    }),
  }),
});
```

#### tagTypes
Slice에서 사용되는 태그들은 tagTypes에 문자열 배열로 기재해야합니다. 

#### providesTags
이름과 같이 캐시된 데이터에 태그를 제공합니다. 태그의 표현 방법은 아래와 같습니다.

1. ['Workspace'] : tagTypes와 같은 형태로 제공
  - 배열로 되어 있는 것으로 유추할 수 있지만, 여러 개의 태그 지정 가능
  - 즉, 위의 fetchWorkspace endpoint에 ['Workspace', 'EvaluationTemplate'] 로 지정해준다면 EvaluationTemplate 와 연관되어 갱신이 가능하다는 의미
2. [{type: 'Workspace'}] : 객체 형태로 제공
  - 형태만 다를 뿐 1번과 같음
3. [{type: 'Workspace', id: 1}] : type과 id 형태로 제공




**id를 이용하는 방법**

providesTags에 태그를 지정할 때, 배열뿐만 아니라 (<span class="highlighting-underline">result, error, arg) => {} 같은 콜백 함수 형태</span>로 넣어줄 수도 있습니다.

해당 함수의 파라미터를 활용하여 아래의 예시처럼 id로 디테일하게 캐시 데이터 구분이 가능합니다.

```javascript
providesTags: (result, error, arg) => result ? [ ...result.map(({id}) => ({ type: 'Workspace', id })) ] : [{type: 'Workspace', id: 'LIST' }];
```

여기서 주목할 점은 <span class="highlighting-underline">[{type: 'Workspace', id: 'LIST' }] 처럼 id에 문자열 값을 넣어 디테일한 부분을 임의로 지정</span>할 수도 있다는 것입니다.

#### invalidatesTags

해당 <span class="highlighting-underline">속성을 정의한 Mutation을 실행하면 providesTags에 정의된 태그를 기반으로 특정 캐시 데이터를 갱신</span>할 수 있습니다.

providesTags와 마찬가지로 콜백 함수를 제공합니다.

#### 태그 갱신 동작 범위
> type에 id를 지정하지 않을 경우 type과 관련된 전체 캐시 데이터를 갱신하게 되니 주의하여야 합니다.
{: .prompt-danger }

![Image](https://github.com/user-attachments/assets/79e4b75c-db25-4168-80e7-4a2c5ae8c762)

### 3-3. custom response(response 구성 변경)
`transformResponse`와 `transformErrorResponse`를 이용하면 endpoint의 결과(성공/실패) 값을 받을 때, 결과 값을 Custom하여 받을 수 있습니다.

#### transformResponse
특정 endpoint가 예외를 던지지 않으면 아래와 같은 <span class="highlighting-underline">해당 콜백 함수를 통해서 결과값을 반환</span>합니다.

```javascript
transformResponse: (response, meta, arg) => response.body;
```

#### transformErrorResponse
특정 endpoint가 예외를 던지면 아래와 같은 <span class="highlighting-underline">해당 콜백 함수를 통해서 결과값을 반환</span>합니다.


```javascript
 transformErrorResponse: (response, meta, arg) => response.code;
```

## 4. 주의 사항

---

이 항목은 RTK Query 사용 시 주의해야할 사항에 대하여 기재하였습니다.

### API 데이터 캐싱 시 매개변수 타입 주의
전역 데이터 캐싱 작업 시, 분명 같은 API를 호출하는데 여러 번 API 요청하는 현상이 발생하였습니다.

Redux DevTools를 통하여 디버깅한 결과 <span class="highlighting-underline">Local Storage에서 가져온 workspaceNo는 String이고 API에서 가져온 매개변수는 Number</span>였습니다.

RTK Query에서는 API 데이터 <span class="highlighting-underline">캐싱 시 endpoint 이름 + 매개변수로 Key를 만들어 캐싱</span>하게 됩니다.

이 때, 아래와 같이 endpoint 객체 안에 매개변수로 된 Key로 API 데이터를 구분하기 때문에 Key가 String인 '103'으로 캐싱을 한 후, <span class="highlighting-underline">key가 Number인 103으로 API를 요청하였기 때문에 API를 재요청하게 된 것</span>입니다.

```javascript
 // 1차로 API 요청한 결과
{
  api
    provided : {
        Workspace :
        {
          "103": [ "fetchWorkspace(\"103\")" ] // Key/값의 매개변수가 문자로 되어 있음
        }
    },
    ...
}
 
// 2차로 API 요청한 결과
{
  api
    provided : {
        Workspace :
        {
          103: [ "fetchWorkspace(103)" ] // Key/값의 매개변수가 숫자로 되어 있음
        }
    },
 
    ...
 
}
```