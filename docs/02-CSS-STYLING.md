# 02 CSS Styling
- Global CSS 파일을 프로젝트에 추가하는 방법
- Tailwind CSS와 CSS 모듈을 사용한 스타일링
- `clsx` 유틸리티 패키지를 사용하여 조건에 따른 클래스명 생성

## Global styles
`global.css`를 최상위 컴포넌트에 추가하는 것으로 전역 스타일링을 할 수 있다. Next.js에서는 **root layout**이 최상위 컴포넌트에 해당한다.

## Tailwind
Tailwind는 TSX 마크업에서 직접 유틸리티 클래스를 작성하여 빠르게 개발할 수 있는 CSS 프레임워크이다. JSX와 스타일을 분리하기를 원하거나 전통 CSS 규칙을 작성하는 것을 선호한다면 CSS 모듈이 좋은 대체 선택이 될 것이다.

## CSS Modules
CSS Module을 사용하면 고유한 클래스 이름을 자동으로 생성하여 컴포넌트에 CSS 범위를 지정할 수 있어 스타일 충돌이 발생하지 않는다.

## Using the `clsx` library to toggle class names
상태나 다른 조건에 따라 요소의 스타일을 조건부로 지정해야 하는 경우가 있다. `clsx`는 클래스 이름을 쉽게 전환할 수 있는 라이브러리이다.
