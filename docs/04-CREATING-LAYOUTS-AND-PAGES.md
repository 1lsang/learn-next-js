# 04 Creating Layouts and Pages
## Nested routing
Next.js는 **파일 시스템 기반의 라우팅**을 사용하여, 폴더들이 중첩된 경로를 만든다. 각 폴더는 URL 세그먼트에 매핑되는 경로 세그먼트를 나타낸다.

그리고 `layout.tsx`와 `page.tsx` 파일을 통해서 각 경로의 UI를 분리할 수 있다.
- `page.tsx`는React 컴포넌트를 내보내는 파일이며 경로에 접근하기 위해서는 이 파일이 필요하다.
  `/app/page.tsx`는 `/` 경로의 홈페이지와 연결된다. 중첩 경로를 만들기 위해서는 폴더 내부에 중첩 폴더를 생성하고 그 내부에 `page.tsx`를 추가하면 된다.

## Creating the dashboard page
`page`라는 특수한 파일명을 가지면서, Next.js는 UI 컴포넌트, 테스트 파일, 경로에 연관된 코드 등을 함께 배치할 수 있게 되었다. 오직 `page` 파일 내부의 콘텐츠만 공개적으로 접근 가능하다. 따라서 `/ui`, `/lib` 등의 폴더가 `/app` 내부에 함께 배치될 수 있다.

## Creating the dashboard layout
대시보드에는 여러 페이지에서 공유되는 네이게이션이 있다. Next.js에서는 `layout.tsx` 파일을 사용하여 여러 페이지 간에 공유되는 UI를 만들 수 있다.

`<Layout/>` 컴포넌트는 `children` 프로퍼티를 받으며, 페이지 혹은 다른 레이아웃이 `children`일 수 있다.  레이아웃은 재렌더링 되지 않고 페이지 컴포넌트만을 업데이트하며 이를 partial rendering이라고 한다.

## Root layout
`/app` 폴더 내부의 `layout.tsx`를 Root layout이라고 부흔다. Root layout은 필수이다.
Root layout에 추가하는 모든 UI는 어플리케이션의 모든 페이지에서 공유된다. Root Layout을 사용하여 `<html>`과 `body` 태그, 메타데이터를 수정할 수 있다.
