# 14 Improving Accessibility
양식 유효성 검사를 구현하는 방법과 접근성을 염두에 두고 form 오류를 표시하는 방법
## What is accessibility?
접근성은 장애인을 포함한 모든 사람이 사용할 수 있도록 웹 어플리케이션을 설계하고 구현하는 것을 말한다. 키보드 navigation, 시맨틱 HTML, 이미지, 색상, 동영상 등 많은 영역이 포함된다. Next.js에서 사용할 수 있는 접근성 기능과 어플리케이션의 접근성을 높이기 위한 관행을 살펴본다.

## Using the ESLint accessibility plugin in Next.js
기본적으로 Next.js는 접근성 문제를 미리 발견하는 데 도움을 주는 `eslint-plugin-jsx-a11y` 플러그인이 포함되어 있다. 예를 들어, 이 플러그인은 대체 텍스트(`alt`)가 없는 이미지가 있거나 `aria-*` 및 `role` 속성을 잘못 사용하는 등 접근성을 해치는 요소에 대해 경고합니다.

`package.json`에 `next lint`를 스크립트로 추가하고 배포 이전에 린트를 실행하여 접근성 문제를 파악하는 것이 좋다.

## Improving form accessibility
form 접근성을 접근하기 위한 방법은 다음과 같다.
- Semantic HTML: `<div>` 대신 시맨틱 요소(`<input>`, `<option>` 등)를 사용한다. 이를 통해 보조 기기가 input 요소에 포커스하고 사용자에게 적절한 문맥 정보를 제공하여 form을 더 쉽게 탐색하고 이해할 수 있다.
- Labelling: `<label>`과 `htmlFor` 속성을 포함하면 폼 필드에 설명 텍스트 레이블이 지정된다. 이를 통해 컨텍스트를 제공하여 보조 기기에 대한 지원을 높이며, 사용자가 레이블을 클릭하여 포커스할 수 있어 사용성이 향상됩니다.
- Focus Outline: 필드에 초점이 맞춰져 있을 때, 윤곽선이 표시되도록 적절한 스타일을 사용한다. 이는 페이지의 활성화된 요소를 시각적으로 표시하여 키보드 및 화면 리더기 사용자가 양식의 현재 위치를 이해하는데 도움을 줄 수 있다. 이는 접근성을 위해 중요하며 `tab`을 눌러 확인할 수 있다.
  이러한 관행은 많은 사용자가 form에 더 쉽게 접근할 수 있도록 하지만, 유효성 검사 및 오류는 해결하지 못한다.

## Form validation
빈 form을 서버 액션을 통해서 전송하면 오류가 발생한다. 클라이언트 혹은 서버에서 form의 유효성을 검사하여 문제를 방지할 수 있다.

### Client-Side validation
클라이언트에서 양식의 유효성을 검사하는 방법은 여러가지가 있다. 가장 간단한 방법은 form의 `<input>`과 `<select>` 요소에 필수 속성을 추가하여 브라우저에서 제공하는 양식 유효성 검사를 사용하는 것이다. 해당 속성을 사용할 경우 빈 값으로 양식을 제출하려고 하면 브라우저에 경고가 표시된다.

### Server-Side validation
서버에서 form의 유효성을 검사할 수도 있으며, 다음과 같은 특징을 가진다.
- 데이터를 데이터 베이스로 보내기 전에 데이터가 올바른 포맷인지 확인한다.
- 악의적인 사용자가 클라이언트 유효성 검사를 우회하는 위험을 줄인다.
- 유효한 데이터로 확인되는 신뢰할 수 있는 단일 소스를 확보한다.

`react-dom`에서 `useFormState` 훅을 사용한다. (`"use client"`) 파라미터로 액션 함수와 초기 상태를 전달하면, `state`와 `dispatch`를 반환하며 form의 서버 액션에 `dispatch`를 연결해주면 된다.

그 후, 데이터의 유효성을 검사하는 로직을 액션 함수에 추가하고 실패시 에러와 메시지를 반환하도록 설정한다. 그리고 `form` 컴포넌트 내부의 `<input>`, `<select>` 요소 등에 `aria-describeby`와 `id` 속성을 통해 연결한 오류를 표시하는 요소를 추가한다.
