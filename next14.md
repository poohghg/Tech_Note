# next14

---

##  1.Routing

**Next.js는 폴더를** 사용하여 경로를 정의하는 파일 시스템 기반 라우터를 사용합니다 .

각 폴더는 **URL** 세그먼트 에 매핑되는 [**경로** 세그먼트를](https://nextjs.org/docs/app/building-your-application/routing#route-segments) 나타냅니다 . [중첩된 경로를](https://nextjs.org/docs/app/building-your-application/routing#nested-routes) 만들려면 폴더를 서로 중첩하면 됩니다.

### 페이지 및 레이아웃

App router에는 페이지,공유 레이아웃, 및 템플릿을 쉽게 생성 할 수 있는 새로운 파일 규칙이 도입되었다.

#### 알아두면 좋음점

- 페이지는 항상 경로 하위 트리의 리프이다.

- `.js`, `.jsx`, 또는 `.tsx`파일 확장자를 페이지에 사용할 수 있다

  - tsx파일 과 tx파일에 혼선이 있으면 안된다.

  - ```
    TS2749: refers to a value, but is being used as a type here.
    ```

- `page.js`경로 세그먼트를 공개적으로 액세스하려면 파일이 필요합니다.

- 페이지는 기본적으로 [서버 구성 요소 이지만 ](https://nextjs.org/docs/app/building-your-application/rendering/server-components)[클라이언트 구성 요소](https://nextjs.org/docs/app/building-your-application/rendering/client-components) 로 설정할 수 있습니다.

- 페이지는 데이터를 가져올 수 있습니다. 자세한 내용은 [데이터 가져오기](https://nextjs.org/docs/app/building-your-application/data-fetching) 섹션을 참조하세요.



### 레이아웃

레이아웃은 여러 페이지에서 공유 될 수 있다. 레이아웃의 상태는 공유,유지 되며 다시 렌더링 하지 않는다. 또한 레이아웃은 중첩될 수 있다.

```tsx
export default function DashboardLayout({
  children, // will be a page or nested layout
}: {
  children: React.ReactNode
}) {
  return (
    <section>
      {/* Include shared UI here e.g. a header or sidebar */}
      <nav></nav>
      {children}
    </section>
  )
}
```

#### 알두두면 좋은점

- 최상위 레이아웃을 루트 레이아웃 이라고 한다. 이 필수 레이아웃은 애플리케이션의 모든 페이지에서 공유 되며 루트 레이아웃에는 html 및 body 태그가 포함된다.
- 모든 경로 세그먼트는 선택적으로 자체 [레이아웃을](https://nextjs.org/docs/app/building-your-application/routing/pages-and-layouts#nesting-layouts) 정의할 수 있습니다 . 이러한 레이아웃은 해당 세그먼트의 모든 페이지에서 공유됩니다.
- 경로의 레이아웃은 기본적으로 **중첩 됩니다.** 각 상위 레이아웃은 React `children`prop을 사용하여 그 아래의 하위 레이아웃을 래핑합니다.
- 레이아웃은 기본적으로 [서버 구성요소 이지만 ](https://nextjs.org/docs/app/building-your-application/rendering/server-components)[클라이언트 구성요소](https://nextjs.org/docs/app/building-your-application/rendering/client-components) 로 설정할 수 있습니다 .
- 레이아웃은 데이터를 가져올 수 있습니다. 자세한 내용은 [데이터 가져오기](https://nextjs.org/docs/app/building-your-application/data-fetching) 섹션을 참조하세요 .
- 상위 레이아웃과 해당 하위 레이아웃 간에 데이터를 전달하는 것은 불가능합니다. 그러나 경로에서 동일한 데이터를 두 번 이상 가져올 수 있으며 React는 성능에 영향을 주지 않고 [자동으로 요청의 중복을 제거합니다](https://nextjs.org/docs/app/building-your-application/caching#request-memoization) .
- 레이아웃은 자체 아래의 경로 세그먼트에 접근할 수 없습니다. 모든 경로 세그먼트에 액세스하려면 클라이언트 구성 요소에서 [`useSelectedLayoutSegment`](https://nextjs.org/docs/app/api-reference/functions/use-selected-layout-segment)또는를 사용할 수 있습니다 .[`useSelectedLayoutSegments`](https://nextjs.org/docs/app/api-reference/functions/use-selected-layout-segments)
- `.js`, `.jsx`, 또는 `.tsx`파일 확장자를 레이아웃에 사용할 수 있습니다.
- A `layout.js`와 `page.js`파일은 동일한 폴더에 정의될 수 있습니다. 레이아웃이 페이지를 래핑합니다.

#### [루트 레이아웃(필수)](https://nextjs.org/docs/app/building-your-application/routing/pages-and-layouts#root-layout-required)

루트 레이아웃은 디렉터리 최상위 수준에서 정의되며 app모든 경로에 적용된다.이 레이아웃을 사용하면 서버에서 반환된 초기 HTML을 수정 할 수 있다.

#### 알아두면 좋은점

- 디렉토리 app에는 루트레이아웃이 포함되어여 한다.
- 루트 레이아웃은 자동으로 생성되지 않으므로 루트 레이아웃을 정의하고, <html>,<body>를 생성해야 한다.
- [내장된 SEO 지원을](https://nextjs.org/docs/app/building-your-application/optimizing/metadata)`<head>` 사용하여 HTML 요소(예: 요소)를 관리 할 수 있습니다 `<title>`.
- [경로 그룹을](https://nextjs.org/docs/app/building-your-application/routing/route-groups) 사용하여 여러 루트 레이아웃을 생성 할 수 있습니다 . [여기에서 예를](https://nextjs.org/docs/app/building-your-application/routing/route-groups#creating-multiple-root-layouts) 참조하세요 .
- 루트 레이아웃은 기본적으로 [서버 구성 요소 이며 ](https://nextjs.org/docs/app/building-your-application/rendering/server-components)[클라이언트 구성 요소](https://nextjs.org/docs/app/building-your-application/rendering/client-components) 로 설정할 **수 없습니다** .
- 루트 레이아웃은 [`_app.js`](https://nextjs.org/docs/pages/building-your-application/routing/custom-app)및 [`_document.js`](https://nextjs.org/docs/pages/building-your-application/routing/custom-document)파일을 대체합니다.



### 중첩 레이아웃

기본적으로 파일 계층 구조의 레이아웃은 중첩된다.즉, `children`prop을 통해 하위 레이아웃을 래핑합니다.

**알아두면 좋은 점** :

- 루트 레이아웃에만 `<html>`및 `<body>`태그가 포함될 수 있습니다.
- ![중첩 레이아웃](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fnested-layouts-ui.png&w=3840&q=75&dpl=dpl_Ekn7g1svMFrmiPHmLbm17NNjunHQ)



### 템플릿

템플릿은 각 하위 레이아웃이나 페이지를 래핑하여 렌더링 하는 방식은 레이아웃과 유사하지만 다음과 같은 차이점이 있다.

- 템플릿을 공유하는 페이지를 라우터시 해당 템플릿은 새로운 인스턴스를 마운트한다
  - 이는 상태가 공유되지 않는것을 의미한다.
- 탬플릿은 레이아웃과 하위 항목 사이에 렌더링 된다.

### 메타데이터

메타데이터 또는 파일의 [`metadata`개체](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#the-metadata-object) 나 [`generateMetadata`함수를](https://nextjs.org/docs/app/api-reference/functions/generate-metadata#generatemetadata-function) 내보내 정의할 수 있다.

```tsx
export const metadata: Metadata = {
  title: 'Next.js',
}
```

#### 정적 메타데이터/동적 메타데이터

##### 정적 메타데이터

```tsx
export const metadata: Metadata = {
  title: '...',
  description: '...',
}
export default function Page() {}
```

ㅎㅎ

### 라우팅

서버에서 애플리케이션 코드는 경로 세그먼트를 자동으로 코드 분할한다. **경로 세그먼트를 미리 가져 오고 캐싱한다**.즉 경로 이동시 브라우저는 페이지를 다시 로드하지 않고 변경된 경로만 다시 렌더링한다.



### 경로 그룹

app 폴더 내 하위 경로의 중첩된 세그먼트는 일반적으로 URL 경로에 매핑된다. 그러나 폴더가 경로의 URL 경로에 포함되지 않도록 폴더를 경로 그룹으로 표시 할 수 있다.

![Route Groups with Opt-in Layouts](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Froute-group-opt-in-layouts.png&w=3840&q=75&dpl=dpl_FMGsYbamaCihTR7jyf43krGr3wQk)

##### 경로 세그먼트 그룹화의 장점

- [여러 루트 레이아웃을 포함하여 동일한 세그먼트에 여러 중첩 레이아웃 만들기](https://nextjs.org/docs/app/building-your-application/routing/route-groups#creating-multiple-root-layouts)

  - top level의 루트 레이아웃을 제거하고 각 그룹화된 경로 세그먼트에 독립적인 레이아웃을 적용 할 수 있다.

  - 루트 레이아웃이 없기에 다음의 추가는 반드시 이루워져야 한다.

    - ```
      The <html> and <body> tags need to be added to each root layout.
      ```

- [공통 구간의 경로 하위 집합에 레이아웃 추가](https://nextjs.org/docs/app/building-your-application/routing/route-groups#opting-specific-segments-into-a-layout)

  - 각 그룹마다 공통의 레이아웃을 만들 수 있다.

##### 알아두면 좋은점

- 경로 그룹의 이름은 각 폴더의 관심사를 나타낸것이지, URL경로에 영향을 주지 않는다.
- 하지만 동일한 경로의 페이지를 중복 생성하면 안된다.
- 최상위 layout 파일이 없이 여러 루트 레이아웃을 사용하는 경우 홈 page.js파일은 경로 그룹 중 하나에 정의되어야 한다.

### UI로딩 및 스트리밍

경로 세그멘트의 콘테츠가 로드되는 동안 서버에서 즉시 로드 상태를 표시할 수 있다. 렌더링이 완료되면 새 콘텐츠가 자동으로 교체된다.

- 페이지 전체의 로딩 U를 표기한다.

![loading.js 특수 파일](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Floading-special-file.png&w=3840&q=75&dpl=dpl_FMGsYbamaCihTR7jyf43krGr3wQk)

#### 즉시 로딩 상태(Instant Loading states)

Instant Loading states는 네비게이션시 즉시 보여지는 대체 UI이다.

Loading Ui는 레이아웃 컴포넌트내 중첩형태로 표기되며, 자동으로 <Suspense> 컴포넌트의 fallback이 된다.

#### 서스펜스를 이용한 스트리밍

<Suspense> 사용한 스트리밍을 수동으로 생성 할 수 있다.

서스펜스는 비동기 작업을 구성하는 요소를 래핑하고 해당 작업이 진행되는 동안 대체 UI를 표시한 다음 구성요소를 교체한다.

##### 서스펜스를 이용한 이점

1. 스트리밍 서버 렌더링: 서버에서 클라이언트로 HTML을 점진적으로 렌더링한다.
2. 선택적 하이드레이션:  React는 사용자 상호 작용을 기반으로 어떤 구성 요소를 먼저 대화식으로 만들 것인지 우선순위를 정한다.

##### 스트리밍이란?

SSR시 일련의 단계

1. 먼저, 특정 페이지의 모든 데이터를 서버에서 가져옵니다.
2. 그런 다음 서버는 페이지의 HTML을 렌더링합니다.
3. 페이지의 HTML, CSS 및 JavaScript가 클라이언트로 전송됩니다.
4. 비대화형 사용자 인터페이스는 생성된 HTML 및 CSS를 사용하여 표시됩니다.
5. 마지막으로 React는 [수화물을 공급합니다.](https://react.dev/reference/react-dom/client/hydrateRoot#hydrating-server-rendered-html)사용자 인터페이스를 대화형으로 만듭니다.

![스트리밍 없이 서버 렌더링을 보여주는 차트](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fserver-rendering-without-streaming-chart.png&w=3840&q=75&dpl=dpl_FMGsYbamaCihTR7jyf43krGr3wQk)

이러한 단계는 순차적이며 차단이 발생 할 수 있다. 모든 데이터 패칭 후 서버에서 페이지의 HTML 렌더링 후 클라이언트에서 해당 파일의 다운로드된 후에야 UI를 하이드레이션 할 수 있다.

> 서버 컴포넌트는 서버에서 생성되는 스프리밍 파일이다?

- 서버자원의 다운이 완료된 후 사용자에게 표기 되기에 여전히 속도가 느릴 수 있다.
- 이때 **스트리밍**을 사용하면 페이지의 HTML을 더 작은 청크로 나누고 점진적으로 해당 청크를 서버에서 클라이언트로 보낼 수 있다.
- **스트리밍**은 각 구성 요소가 하나의 덩어리로 간주될 수 있기 때문에, 우선순위가 높거나 데이터에 의존적이지 않는 요소는 더 일찍 하이드레이션 할 수 있다.
- [스트리밍은 TTFB(Time To First Byte)를](https://web.dev/ttfb/) 줄일 수 있으므로 긴 데이터 요청으로 인해 페이지 렌더링이 차단되는 것을 방지하려는 경우 특히 유용하다.
- [첫 번째 콘텐츠가 포함된 페인트(FCP)](https://web.dev/first-contentful-paint/). 또한 [TTI(Time to Interactive)를 개선하는 데 도움이 된다.](https://developer.chrome.com/en/docs/lighthouse/performance/interactive/)

![스트리밍 없이 서버 렌더링](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fserver-rendering-without-streaming.png&w=3840&q=75&dpl=dpl_FMGsYbamaCihTR7jyf43krGr3wQk)

[SSR 과정]

![스트리밍을 통한 서버 렌더링 작동 방식](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fserver-rendering-with-streaming.png&w=3840&q=75&dpl=dpl_FMGsYbamaCihTR7jyf43krGr3wQk)

[스트리밍 과정]

![스트리밍을 사용한 서버 렌더링을 보여주는 차트](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fserver-rendering-with-streaming-chart.png&w=3840&q=75&dpl=dpl_FMGsYbamaCihTR7jyf43krGr3wQk)

### 오류 처리

런타임 오류를 적절하게 처리 할 수 있다.

- [React Error Boundary](https://react.dev/reference/react/Component#catching-rendering-errors-with-an-error-boundary) 에서 경로 세그먼트와 중첩된 하위 항목을 자동으로 래핑합니다..
- 세분성을 조정하기 위해 파일 시스템 계층 구조를 사용하여 특정 세그먼트에 맞는 오류 UI를 만듭니다.
- 나머지 애플리케이션 기능을 유지하면서 영향을 받은 세그먼트에 대한 오류를 격리합니다.
- 전체 페이지를 다시 로드하지 않고 오류 복구를 시도하는 기능을 추가합니다.



### 병렬 경로 ([Parallel Routes](https://nextjs.org/docs/app/building-your-application/routing/parallel-routes))

병렬 라우팅을 사용하면 동일한 레이아웃에서 하나 이상의 페이지를 동시에 또는 조건부로 렌더링할 수 있다.

- 컴포넌트를 렌더링 하는거랑 뭐가 다르지?



### 경로 차단([Intercepting Routes](https://nextjs.org/docs/app/building-your-application/routing/intercepting-routes))

경로를 가로채어 현재 레이아웃 내 애플리케이션의 다른 부분에서 경로를 로드할 수 있습니다.이는 사용자가 다른 컨텍스트로 전환하지 않고도 경로의 내용을 표시하려는 경우 유용하다.



### 라우터 핸들러

라우터 핸들러를 사용하면 Web [Request](https://developer.mozilla.org/docs/Web/API/Request) and [Response](https://developer.mozilla.org/docs/Web/API/Response) APIs을 사용하는 custom request handlers을 만들수 있다.

#### convention

- 라우터 핸들러는 route 파일 내부에 생성 되어야 한다.
- 라우터 핸들러는 중첩되어 사용 될 수 있다.

#### Caching

- GET 요청에 대한 응답은 기본적으로 캐싱된다.
- 캐싱 해제 방법
  - `Request`메소드 와 함께 객체를 사용합니다 `GET`.
  - 다른 HTTP 메서드를 사용합니다.
  - 및 와 같은 [동적 기능을](https://nextjs.org/docs/app/building-your-application/routing/route-handlers#dynamic-functions) 사용합니다 .`cookies``headers`
  - 세그먼트 [구성 옵션은](https://nextjs.org/docs/app/building-your-application/routing/route-handlers#segment-config-options) 동적 모드를 수동으로 지정합니다.



### Middleware

미들에워는 요청이 완료되기전에 코드를 실행 시킬 수 있게한다. 요청에 기초하여 응답을 수정할 수 있다. 요청 또는 응답 헤더를 수정, rewriting, redirecting 함으로써.

미들웨어는 캐시된 콘텐츠와 경로가 일치하기 전에 실행됩니다. 자세한 내용은 [일치하는 경로를](https://nextjs.org/docs/app/building-your-application/routing/middleware#matching-paths) 참조하세요 .

#### convention

- 미들웨어는 프로젝트 루트에 생성해야한다.

#### [Matching Paths](https://nextjs.org/docs/app/building-your-application/routing/middleware#matching-paths)

미들웨어는 모든 경로에 대해 호출된다, 호출 순서는 다음과 같다.

1. `headers` from `next.config.js`
2. `redirects` from `next.config.js`
3. Middleware (`rewrites`, `redirects`, etc.)
4. `beforeFiles` (`rewrites`) from `next.config.js`
5. Filesystem routes (`public/`, `_next/static/`, `pages/`, `app/`, etc.)
6. `afterFiles` (`rewrites`) from `next.config.js`
7. Dynamic Routes (`/blog/[slug]`)
8. `fallback` (`rewrites`) from `next.config.js`

#### matcher

`matcher`특정 경로에서 실행되도록 미들웨어를 필터링할 수 있습니다.

```tsx
// "/"으로 시작하여야함.
export const config = {
  matcher: ['/about/:path*', '/dashboard/:path*'],
}
```

#### [NextResponse](https://nextjs.org/docs/app/building-your-application/routing/middleware#nextresponse)

The `NextResponse` API allows you to:

- `redirect` the incoming request to a different URL
- `rewrite` the response by displaying a given URL
- Set request headers for API Routes, `getServerSideProps`, and `rewrite` destinations
- Set response cookies
- Set response headers

#### 라이플 사이클? 예전 자료

1. Next Server가 GET 요청을 받는다.
2. 요청에 맞는 Page를 찾는다.
3. _app.js의 getInitialProps가 있다면 실행한다.
4. Page Component의 getInitialProps가 있다면 실행한다. pageProps들을 받아온다.
5. _document.js의 getInitialProps가 있다면 실행한다. pageProps들을 받아온다.
6. 모든 props들을 구성하고, _app.js > page Component 순서로 rendering.
7. 모든 Content를 구성하고 _document.js를 실행하여 html 형태로 출력한다.

#### [쿠키 사용](https://nextjs.org/docs/app/building-your-application/routing/middleware#using-cookies)

```tsx
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'
 
                            
}
```

#### [헤더 설정](https://nextjs.org/docs/app/building-your-application/routing/middleware#setting-headers)

```tsx
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'
 
export function middleware(request: NextRequest) {
  // Clone the request headers and set a new header `x-hello-from-middleware1`
  const requestHeaders = new Headers(request.headers)
  requestHeaders.set('x-hello-from-middleware1', 'hello')
 
  // You can also set request headers in NextResponse.rewrite
  const response = NextResponse.next({
    request: {
      // New request headers
      headers: requestHeaders,
    },
  })
 
  // Set a new response header `x-hello-from-middleware2`
  response.headers.set('x-hello-from-middleware2', 'hello')
  return response
}
```

___

## 2.Data Fetching

### 데이터 가져오기, 캐싱 및 재검증

데이터 패칭은 애플리케이션의 핵심 부분이다.

데이터를 가져올 수 있는 방법

1. [서버에서`fetch`](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching-caching-and-revalidating#fetching-data-on-the-server-with-fetch)
2. [서버에서 타사 라이브러리 사용](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching-caching-and-revalidating#fetching-data-on-the-server-with-third-party-libraries)
3. [클라이언트에서 경로 핸들러를 통해](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching-caching-and-revalidating#fetching-data-on-the-client-with-route-handlers)
4. [클라이언트에서 타사 라이브러리를 사용](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching-caching-and-revalidating#fetching-data-on-the-client-with-route-handlers) .

#### Fetch

Next.js는 기본 [`fetch`웹 API를 확장합니다.](https://developer.mozilla.org/docs/Web/API/Fetch_API)서버의 각 가져오기 요청에 대한 [캐싱](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching-caching-and-revalidating#caching-data) 및 [재검증](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching-caching-and-revalidating#revalidating-data) 동작을 구성할 수 있습니다 . React는 React 컴포넌트 트리를 렌더링하는 동안 가져오기 요청을 `fetch`자동으로 [메모하도록 확장됩니다.](https://nextjs.org/docs/app/building-your-application/data-fetching/patterns#fetching-data-where-its-needed)

> You can use `fetch` with `async`/`await` in Server Components, in [Route Handlers](https://nextjs.org/docs/app/building-your-application/routing/route-handlers), and in [Server Actions](https://nextjs.org/docs/app/building-your-application/data-fetching/forms-and-mutations).
>
> 클라이언트 컴포넌트에서 Next 제공 fetch라이브러리는 사용 할 수 없음.

#### 데이터 캐싱

캐싱은 데이터를 저장하므로 요청이 있을 때마다 데이터 소스에서 다시 가져올 필요가 없습니다.

기본적으로 Next.js는 반환된 값을 서버의 [데이터 캐시](https://nextjs.org/docs/app/building-your-application/caching#data-cache)`fetch` 에 자동으로 캐시합니다. 이는 빌드 시 또는 요청 시 데이터를 가져오고, 캐시하고, 각 데이터 요청에서 재사용할 수 있음을 의미합니다.

```tsx
// 'force-cache' is the default, and can be omitted
fetch('https://...', { cache: 'force-cache' })
```

> **`fetch` requests that use the `POST` method are also automatically cached. Unless it's inside a [Route Handler](https://nextjs.org/docs/app/building-your-application/routing/route-handlers) that uses the `POST` method, then it will not be cached.**
>
> 라우터 핸들러를 통한 POST요청 도 캐시 될 수 있다.

### 데이터 재검증

재검증은 데이터 캐시를 제거하고 최신 데이터를 다시 가져오는 프로세스입니다. 이는 데이터가 변경되어 최신 정보를 표시하려는 경우에 유용합니다.

캐시된 데이터는 다음 두 가지 방법으로 유효성을 다시 검사할 수 있습니다.

- **시간 기반 재검증** : 일정 시간이 지나면 데이터를 자동으로 재검증합니다. 이는 자주 변경되지 않고 최신성이 중요하지 않은 데이터에 유용합니다.
- **주문형 재검증** : 이벤트(예: 양식 제출)를 기반으로 데이터를 수동으로 재검증합니다. 주문형 재검증에서는 태그 기반 또는 경로 기반 접근 방식을 사용하여 데이터 그룹을 한 번에 재검증할 수 있습니다. 이는 가능한 한 빨리 최신 데이터를 표시하려는 경우에 유용합니다(예: 헤드리스 CMS의 콘텐츠가 업데이트되는 경우).

```tsx
fetch('https://...', { next: { revalidate: 3600 } })
export const revalidate = 3600 // revalidate at most every hour
```

### 클라이언트에서 라우트 핸들러를 통한 패칭

만약 클라이언트에서 민감함 정보가 포함하여 요청시 서버에서 실행되는 라우트 핸들러를 사용 할 수 있다.

```
Server Components and Route Handlers

Since Server Components render on the server, you don't need to call a Route Handler from a Server Component to fetch data. Instead, you can fetch the data directly inside the Server Component.
```

### 데이터 패칭 패턴

#### [Fetching Data on the Server](https://nextjs.org/docs/app/building-your-application/data-fetching/patterns#fetching-data-on-the-server)

Whenever possible, we recommend fetching data on the server. This allows you to:

- Have direct access to backend data resources (e.g. databases).
- Keep your application more secure by preventing sensitive information, such as access tokens and API keys, from being exposed to the client.
- Fetch data and render in the same environment. This reduces both the back-and-forth communication between client and server, as well as the [work on the main thread](https://vercel.com/blog/how-react-18-improves-application-performance) on the client.
- Perform multiple data fetches with single round-trip instead of multiple individual requests on the client.
- Reduce client-server [waterfalls](https://nextjs.org/docs/app/building-your-application/data-fetching/patterns#parallel-and-sequential-data-fetching).
- Depending on your region, data fetching can also happen closer to your data source, reducing latency and improving performance.

You can fetch data on the server using Server Components, [Route Handlers](https://nextjs.org/docs/app/building-your-application/routing/route-handlers), and [Server Actions](https://nextjs.org/docs/app/building-your-application/data-fetching/forms-and-mutations).

### [Fetching Data Where It's Needed](https://nextjs.org/docs/app/building-your-application/data-fetching/patterns#fetching-data-where-its-needed)

If you need to use the same data (e.g. current user) in multiple components in a tree, you do not have to fetch data globally, nor forward props between components. Instead, you can use `fetch` or React `cache` in the component that needs the data without worrying about the performance implications of making multiple requests for the same data.

This is possible because `fetch` requests are automatically memoized. Learn more about [request memoization](https://nextjs.org/docs/app/building-your-application/caching#request-memoization)

> **Good to know**: This also applies to layouts, since it's not possible to pass data between a parent layout and its children.

### 병렬 및 순차 데이터 가져오기 ([Parallel and Sequential Data Fetching](https://nextjs.org/docs/app/building-your-application/data-fetching/patterns#parallel-and-sequential-data-fetching))

![순차 및 병렬 데이터 가져오기](https://nextjs.org/_next/image?url=%2Fdocs%2Fdark%2Fsequential-parallel-data-fetching.png&w=3840&q=75&dpl=dpl_2HRzcyVz924hNJUyo2USas69GBgM)

##### 순차적 데이터 가져오기

##### 순차적 데이터 가져오기를 사용하면 경로의 요청이 서로 종속되므로 폭포가 생성된다.이것은 의도적으로 이전 요청의 결과값을 사용할때 유용 할 수 있다. 하지만 의도된 동작이 아니라면 UI로딩 시간이 길어 질 수 있다.

> **데이터 요청 차단:**
>
> 폭포수를 방지하는 또 다른 접근 방식은 애플리케이션의 루트에서 전역적으로 데이터를 가져오는 것입니다. 하지만 이렇게 하면 데이터 로드가 완료될 때까지 그 아래의 모든 경로 세그먼트에 대한 렌더링이 차단됩니다. Either you have the entire data for your page or application, or none.
>
> 모든 가져오기 요청은 경계 에 래핑되거나 사용되지 `await`않는 한 그 아래의 전체 트리에 대한 렌더링 및 데이터 가져오기를 차단합니다 . [또 다른 대안은 병렬 데이터 가져오기](https://nextjs.org/docs/app/building-your-application/data-fetching/patterns#parallel-data-fetching) 또는 [사전 로드 패턴을](https://nextjs.org/docs/app/building-your-application/data-fetching/patterns#preloading-data) 사용하는 것입니다 .`<Suspense>``loading.js`

##### 병렬 데이터 가져오기

데이터를 병렬로 가져오려면 데이터를 사용하는 구성 요소 외부에서 요청을 정의한 다음 구성 요소 내부에서 호출하여 요청을 적극적으로 시작할 수 있다. 이렇게 하면 두 요청을 병렬로 시작하여 시간이 절약된다. 하지만 두 Promise의 요청 처리가 될 때까지 기다려야한다.

```tsx
import Albums from './albums'
 
async function getArtist(username: string) {
  const res = await fetch(`https://api.example.com/artist/${username}`)
  return res.json()
}
 
async function getArtistAlbums(username: string) {
  const res = await fetch(`https://api.example.com/artist/${username}/albums`)
  return res.json()
}
 
export default async function Page({
  params: { username },
}: {
  params: { username: string }
}) {
  // Initiate both requests in parallel
  const artistData = getArtist(username)
  const albumsData = getArtistAlbums(username)
 
  // Wait for the promises to resolve
  const [artist, albums] = await Promise.all([artistData, albumsData])
 
  return (
    <>
      <h1>{artist.name}</h1>
      <Albums list={albums}></Albums>
    </>
  )
}
```

사용자 경험을 향상시키기 위해 [Suspense Boundary를](https://nextjs.org/docs/app/building-your-application/routing/loading-ui-and-streaming) 추가하여 렌더링 작업을 분할하고 결과의 일부를 최대한 빨리 표시할 수 있습니다.

## 3.Rendering

### Server Components

서버 컴포넌트는 서버에서 렌더링되고 선택적으로 캐시할 수 있는 UI를 구성할 수 있다. 스트리밍 및 부분 렌더링을 활성하기 위해 렌더링 작업이 라우터에 의해 구분 되어진다. 다음은 서버 컴포넌트의 3가지 렌더링 전략이다.

> - [Static Rendering](https://nextjs.org/docs/app/building-your-application/rendering/server-components#static-rendering-default)
> - [Dynamic Rendering](https://nextjs.org/docs/app/building-your-application/rendering/server-components#dynamic-rendering)
> - [Streaming](https://nextjs.org/docs/app/building-your-application/rendering/server-components#streaming)

### 서버 렌더링의 이점

- 데이터 패칭 : 데이터 소스에 더 가까운 서버에서 데이터 패칭이 가능하다. 이는 데이터 패칭시간을 줄여 성능을 향상 시킬 수 있다.
- 보안 : 서버 컴포넌트 사용시 토큰 및 API 키와 같은 민감한 정보를 서버에서 처리 할 수 있다.
- 캐싱 : 서버 렌더링시 결과를 캐싱 하고 후속 요청 및 사용자 전체에게 사용 할 수 있게 할 수 있다.
- 번들 크기 : 서버 컴포넌트는 번들에 포함되지 않는다.
- 초기 페이지 로드 :  번들에 포함되지 않기에 브라우저에서 자바스크립트을 처리 시간을 단축 시켜 빠르게 html을 생성 할 수 있다.
- 스트리밍 : 서버 컴포넌트는 렌더링 작업을 청크로 분할되고, 해당 컴포넌트가 준비가 되면 스트리밍 된다. 이를 통해 전체 페이지가 서버에서 렌더링 될 때까지 기다리지 않고도 페이지의 일부를 더 일찍 볼 수 있다.

### 서버 컴포넌트의 렌더링

렌더링은 라우터 경로 및 서스펜스 경계를 기준으로 청크로 분할 된다.

##### 각 청크는 두 단계로 렌더링 된다.

1. **React는 서버 구성요소를 React Server Component Payload(RSC Payload)** 라는 특수 데이터 형식으로 렌더링합니다 .
2. Next.js는 RSC 페이로드 및 클라이언트 구성 요소 JavaScript 지침을 사용하여 서버에서 **HTML을 렌더링합니다.**

##### 클라이언트에서는 다음을 수행한다.

1. HTML은 즉각적으로 non-interactive preview of the route을 보여준다. 이것은 오직 초기 로드시에만 보여진다.
2. RSC Payload는 클라이언트/서버 컴포넌트 조정을 위해 사용되어진다. 그리고 DOM을 업데이트 한다.
3. 자바 스크립트는 클라이언트 컴포넌트를 하이드레이트 하고 어플리케이션은 사용자와 상호작용이 가능해진다.

> **React 서버 구성 요소 페이로드(RSC)란 무엇입니까?**
>
> RSC 페이로드는 렌더링된 React 서버 구성 요소 트리의 압축된 바이너리 표현입니다. 클라이언트의 React에서 브라우저의 DOM을 업데이트하는 데 사용됩니다. RSC 페이로드에는 다음이 포함됩니다.
>
> - 서버 구성 요소의 렌더링 결과
> - 클라이언트 구성 요소를 렌더링해야 하는 위치 및 해당 JavaScript 파일에 대한 참조에 대한 자리 표시자
> - 서버 구성 요소에서 클라이언트 구성 요소로 전달되는 모든 소품



### 서버 렌더링 전략

서버 렌더링에는 정적,동적,스트리밍의 렌더링이 있다.

#### 정적 렌더링(기본값)

정적 렌더링을 사용하면 경로(routes)는 빌드 타임에 렌더링 되거나, 데이터 재검증 후 백그라운드에서 렌더링 된다.결과는 캐쉬되어 CDN으로 푸시 될 수도 있다. 정적 렌더링은 경로에 **사용자에게 개인화되지 않은 데이터가 있고 정적 블로그 게시물이나 제품 페이지**와 같이 빌드 시 알 수 있는 데이터가 있는 경우 유용하다.

#### 동적 렌더링

동적 렌더링을 사용하면 요청 시 각 사용자에 대한 경로가 렌더링된다.동적 렌더링은 경로에 사용자에게 맞춤화된 데이터가 있거나 쿠키나 URL의 검색 매개변수와 같이 요청 시에만 알 수 있는 정보가 있는 경우 유용하다.

> **캐시된 데이터가 있는 동적 경로**
>
> 대부분의 웹사이트에서 경로는 완전히 정적이거나 완전히 동적이지 않습니다. 이는 스펙트럼입니다. 예를 들어, 주기적으로 재검증되는 캐시된 제품 데이터를 사용하지만 캐시되지 않은 개인화된 고객 데이터도 포함하는 전자 상거래 페이지가 있을 수 있습니다.
>
> Next.js에서는 캐시된 데이터와 캐시되지 않은 데이터가 모두 포함된 경로를 동적으로 렌더링할 수 있습니다. 이는 RSC 페이로드와 데이터가 별도로 캐시되기 때문입니다. 이를 통해 요청 시 모든 데이터를 가져올 때 성능에 미치는 영향을 걱정하지 않고 동적 렌더링을 선택할 수 있습니다.
>
> [전체 경로 캐시](https://nextjs.org/docs/app/building-your-application/caching#full-route-cache) 및 [데이터 캐시](https://nextjs.org/docs/app/building-your-application/caching#data-cache) 에 대해 자세히 알아보세요 .

##### 동적 렌더링으로 전환

렌더링하는 동안 [동적 기능](https://nextjs.org/docs/app/building-your-application/rendering/server-components#dynamic-functions) 이나 [캐시되지 않은 데이터 요청이](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching-caching-and-revalidating#opting-out-of-data-caching) 발견되면 Next.js는 전체 경로를 동적으로 렌더링하도록 전환합니다

> | Dynamic Functions | Data       | Route                |
> | ----------------- | ---------- | -------------------- |
> | No                | Cached     | Statically Rendered  |
> | Yes               | Cached     | Dynamically Rendered |
> | No                | Not Cached | Dynamically Rendered |
> | Yes               | Not Cached | Dynamically Rendered |

개발자는 Next.js가 사용된 기능과 API를 기반으로 각 경로에 가장 적합한 렌더링 전략을 자동으로 선택하므로 정적 렌더링과 동적 렌더링 중에서 선택할 필요가 없습니다. [대신 특정 데이터를 캐시하거나 재검증할](https://nextjs.org/docs/app/building-your-application/data-fetching/fetching-caching-and-revalidating) 시기를 선택 하고 UI의 일부를 [스트리밍](https://nextjs.org/docs/app/building-your-application/rendering/server-components#streaming) 하도록 선택할 수도 있습니다 .

##### 동적 기능

동적 기능은 사용자 쿠키, 현재 요청 헤더 또는 URL의 매개변수 와 같이 요청 시에만 알 수 있는 정보에 의존한다.

- **[`cookies()`](https://nextjs.org/docs/app/api-reference/functions/cookies)및[`headers()`](https://nextjs.org/docs/app/api-reference/functions/headers)** : 서버 구성 요소에서 이를 사용하면 요청 시 전체 경로가 동적 렌더링으로 선택됩니다.
- [`useSearchParams()`](https://nextjs.org/docs/app/api-reference/functions/use-search-params):
  - 클라이언트 구성 요소에서는 정적 렌더링을 건너뛰고 대신 클라이언트에서 가장 가까운 상위 Suspense 경계까지 모든 클라이언트 구성 요소를 렌더링합니다.
  - `useSearchParams()`경계 에서 사용하는 클라이언트 구성 요소를 래핑하는 것이 좋습니다 `<Suspense/>`. 이렇게 하면 그 위에 있는 클라이언트 구성 요소를 정적으로 렌더링할 수 있습니다. [예](https://nextjs.org/docs/app/api-reference/functions/use-search-params#static-rendering) .
- **[`searchParams`](https://nextjs.org/docs/app/api-reference/file-conventions/page#searchparams-optional)**: [Pages](https://nextjs.org/docs/app/api-reference/file-conventions/page) 소품을 사용하면 요청 시 페이지가 동적 렌더링으로 선택됩니다.



### Client Components

클라이언트 구성 요소를 사용하면 요청 시 클라이언트에 렌더링할 수 있는 대화형 UI를 작성할 수 있습니다. Next.js에서 클라이언트 렌더링은 **opt-in** 입니다 . 즉, React가 클라이언트에서 렌더링해야 하는 구성 요소를 명시적으로 결정해야 합니다.

`"use client"`서버와 클라이언트 구성 요소 모듈 사이의 [경계를](https://nextjs.org/docs/app/building-your-application/rendering#network-boundary) 선언하는 데 사용됩니다 . 즉, `"use client"`파일에 를 정의하면 하위 구성 요소를 포함하여 해당 파일로 가져온 다른 모든 모듈이 클라이언트 번들의 일부로 간주됩니다. 

#### [Subsequent Navigations](https://nextjs.org/docs/app/building-your-application/rendering/client-components#subsequent-navigations)

후속 탐색 시 클라이언트 구성 요소는 서버에서 렌더링된 HTML 없이 클라이언트에서 완전히 렌더링됩니다.

이는 클라이언트 구성 요소 JavaScript 번들이 다운로드되고 구문 분석됨을 의미합니다. 번들이 준비되면 React는 RSC 페이로드를 사용하여 클라이언트 및 서버 구성 요소 트리를 조정하고 DOM을 업데이트합니다.

### 구성 패턴



### 서버 액션



































































