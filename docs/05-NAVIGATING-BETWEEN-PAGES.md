# 05 Navigating Between Pages

## Why optimize navigation?
페이지를 연결하기 위해 전통적으로 HTML의 `<a>` 태그를 사용하였다. `<a>` 태그를 사용하면 각 페이지의 탐색에서 전체 페이지가 새로고침 된다.

## The `<Link>` component
Next.js에서 어플리케이션 내에서 페이지를 연결하기 위해 `<Link />` 컴포넌트를 사용한다. `<Link />` 컴포넌트는 자바스크립트로 클라이언트단 네비게이션을 가능하게 한다. `<a>` 태그를 사용하는 것과 비슷하게 `<Link />` 컴포넌트를 사용하면 페이지 새로 고침이 없어 웹 앱처럼 느껴지게 될 것이다.

## Automatic code-splitting and prefetching
네비게이션 환경을 개선하기 위해 Next.js는 경로를 기준으로 어플리케이션의 코드를 자동으로 분할한다. 기존의 React SPA와 다르게 브라우저가 초기 로드 시 모든 어플리케이션 코드를 로드하지 않는다. 즉, 경로별로 코드를 분할하여 페이지를 격리시키며, 특정 페이지에서 오류가 발생해도 나머지 어플리케이션을 계속 작동한다.

또한, 프로덕션 환경에서는 `<Link />` 컴포넌트가 브라우저의 뷰포트에 표시될 때 Next.js가 백그라운드에서 링크된 경로에 대해 코드를 자동으로 프리패치한다. 사용자가 링크를 클릭할 때, 이미 페이지의 코드가 백그라운드에서 로드되어 있어 페이지 전환이 매우 빠르게 이루어진다.

[Next.js의 네비게이션 작동방식](https://nextjs.org/docs/app/building-your-application/routing/linking-and-navigating#how-routing-and-navigation-works)

## Pattern: Showing active links
일반적으로 UI에서 사용자가 현재 페이지에 있는 것을 알리기 위해서 활성화된 링크를 보여준다. 이를 위해, 사용자의 현재 경로를 알아야 한다. Next.js에서는 `usePathname()` 훅을 통해 경로를 확인하여 이러한 패턴을 구현할 수 있다. 
