# 01 About React and Next.js

Next.js는 빠르게 Full stack 웹 어플리케이션을 만들기 위한 React 프레임워크이다.

## 웹 어플리케이션의 구성요소
- User Interface
- Routing
- Data Fetching
- Rendering
- Integrations
- Infrastructure
- Performance
- Scalability
- Developer Experience

## What is React?
React는 인터랙티브한 UI를 만들기 위한 자바스크립트 라이브러리이다. 즉, 라이브러리라는 의미는 React가 UI를 구축하는 데 유용한 함수(API)를 제공하지만 어플리케이션에서 해당 함수를 어디에 사용할지 개발자가 결정할 수 있다는 것이다. 하지만 React 어플리케이션을 처음부터 구축하기 위해서는 요구사항에 맞게 도구를 구성하고 솔루션을 만드는 데에 많은 시간을 투자해야 합니다.

## What is Next.js?
Next.js는 웹 어플리케이션을 만들기 위한 구성요소를 제공하는 React 프레임워크이다. 프레임워크라는 의미는 Next.js가 React에 필요한 다양한 도구와 설정을 다루고 어플리케이션을 위한 추가적인 구조•특징•최적화를 제공한다는 것이다. React를 사용하여 UI를 구축하고, Next.js의 기능을 사용하여 Routing, Data Fetching, Caching 등을 통해 DX와 UX를 개선할 수 있다.

# 02 Rendering User Interfaces (UI)
React의 동작 방식을 알기 전에, 브라우저가 UI를 어떻게 만드는지 알아야 한다. 웹페이지를 방문하면 서버는 HTML 파일을 브라우저에 반환하고, 브라우저는 HTML을 읽고 DOM(Document Object Model)을 구성한다.

## What is the DOM?
DOM은 HTML 요소의 객체 표현으로, 코드와 UI를 연결하는 다리 역할을 하며 부모 자식 관계를 가지는 트리와 같은 구조이다. DOM 메소드와 자바스크립트를 사용하여 사용자 이벤트를 감지하고 선택, 추가, 업데이트, 삭제 등의 DOM 조작을 수행할 수 있다.

# 03 Updating UI with Javascript
## HTML vs the DOM
브라우저에서 개발자 도구로 DOM 요소를 살펴보면 페이지의 DOM은 소스코드(원본 HTML 파일)과 다릅니다. 이는 HTML은 최초 페이지의 콘텐츠를 나타내고, DOM은 자바스크립트 코드에 의해 변경된 업데이트된 페이지의 콘텐츠를 나타내기 때문입니다.

하지만 Plain Javascript를 사용하여 DOM을 조작하는 것은 매우 복잡합니다. 앱이나 팀의 크기가 커질수록 어플리케이션을 개발하는 것이 매우 어려워집니다.

## 명령형 vs 선언형 프로그래밍
Plain Javascript로 DOM을 조작하는 것은 명령형 프로그래밍의 좋은 예시이다. 이 방법은 어떻게 UI를 업데이트할지 단계별로 작성한다. 하지만 개발 과정의 속도를 높일 수 있기 때문에 선언형 접근방법이 선호된다. DOM 메소드를 사용하는 대신 개발자가 무엇을 보여주고 싶은지 선언하는 것이다. 즉, **명령형 프로그래밍**이 단계별로 어떻게 피자를 만드는지 주는 것이라면 **선언형 프로그래밍**은 피자를 만드는 단계를 걱정하지 않고 피자를 주문하는 것이다. 그 중 React는 UI를 만들 수 있는 가장 유명한 선언형 라이브러리이다.

## React: A declarative UI library
개발자가 UI에 어떤 일이 일어나기를 원하는지 React에 알려주면, React가 개발자를 대신해 DOM을 업데이트하는 단계를 수행한다.

# 04 Getting Started with React
React를 사용하여 새로운 프로젝트를 만들면, 2개의 라이브러리를 불러와야 한다.
- **react**는 React 코어 라이브러리이다.
- **react-dom**은 DOM과 함께 React를 사용할 수 있는 DOM 전용 메서드를 제공한다.

## What is JSX?
JSX는 HTML과 유사한 문법으로 UI를 만드는 자바스크립트의 확장 구문입니다. JSX의 장점은 세 가지 JSX 규칙을 따르는 것 외에는 HTML과 JavaScript 이외의 새로운 기호나 구문을 배울 필요가 없다는 것입니다.

하지만 브라우저는 기본적으로 JSX를 이해하지 못하므로 JSX 코드를 일반 자바스크립트로 변환하려면 바벨과 같은 자바스크립트 컴파일러가 필요합니다.

## React를 위한 필수 자바스크립트
- 함수와 화살표 함수
- 객체
- 배열과 배열 메서드
- 구조 분해 할당
- 템플릿 리터럴
- 삼항 연산자
- ES 모듈과 import/export 문법

# 05 Building UI with Components
## React 코어 컨셉
React 어플리케이션을 만드는데 세가지 코어 컨셉이 있다.
- Components
- Props
- State
## Components
UI는 **컴포넌트**라고 부르는 더 작은 구성요소로 쪼갤 수 있다.

컴포넌트로 독립적이고 재사용할 수 있는 코드 스니펫을 만들 수 있다. 레고 브릭처럼 생각하면 독립적인 브릭들을 결합하여 큰 구조를 만들 수 있느 것이다. UI의 일부분을 업데이트해야하면 특정 컴포넌트를 업데이트하면된다.

이런 모듈성 덕분에 어플리케이션의 다른 부분을 건드리지 않고 컴포넌트를 추가, 업데이트 삭제할 수 있어 코드가 커져도 유지보수가 용이하다.

# 06 Displaying Data with Props
HTML 요소는 동작을 변경하는 정보를 전달하는데 속성(attribute)을 사용한다. 같은 방식으로 React 컴포넌트에서 정보를 프로퍼티로 전달할 수 있다. 이것을 `props`라고 부른다.

자바스크립트 함수처럼 컴포넌트의 동작이나 화면에 표시되는 내용을 변경하는 컴포넌트를 설계할 수 있고, 부모 컴포넌트에서 자식 컴포넌트로 전달할 수 있다.

# 07 Adding Interactivity with State
React는 상태(state)와 이벤트 핸들러로 상호작용을 추가할 수 있다.

## 이벤트 감지 & 이벤트 다루기
버튼이 클릭될 때 어떤 동작을 하도록 하려면 `onClick` 이벤트를 사용하면 된다. React에서 이벤트 이름은 카멜케이스로 작성한다. `onClick`, `onChange` 등의 이벤트 리스너를 사용할 수 있다. 이벤트가 트리거될 때마다 이벤트를 처리하는 함수를 정의하고 이를 이벤트 리스너에 등록하여 이벤트가 발생할 때마다 함수를 호출할 수 있다.

## State and Hooks
React는 hooks라고 부르는 함수 모음을 가지고 있다. Hooks는 컴포넌트에 **상태**와 같은 로직을 추가할 수 있다. `useState()`를 사용하면 프로젝트에 상태를 추가할 수 있다.

# 08 From React to Next.js
React는 UI를 구축하는 데는 매우 효과적이지만, 그 UI를 완전히 작동하는 확장 가능한 애플리케이션으로 독립적으로 구축하는 데는 약간의 작업이 필요하다. 또한 서버 및 클라이언트 컴포넌트와 같은 최신 React 기능에는 프레임워크가 필요합니다. 좋은 소식은 Next.js가 설정과 구성의 대부분을 처리하고 React 애플리케이션을 빌드하는 데 도움이 되는 추가 기능을 제공한다는 것입니다.

# 09 Installing Next.js
Next.js는 패키지 매니저를 통해서 설치할 수 있다.

# 10 Server and Client Component
서버 컴포넌트와 클라이언트 컴포넌트의 작동방식을 이해하기 위해서는 다음 두가지의 웹 개념을 숙지하는 것이 좋다.
- 어플리케이션 코드가 실행될 수 있는 환경: 서버&클라이언트
- 서버와 클라리언트 코드를 구분하는 네트워크 바운더리

## Server and Client Environments
- **클라이언트**는 애플리케이션 코드에 대한 요청을 서버로 보내는 사용자 기기의 브라우저를 의미한다. 서버로부터 받은 응답을 사용자가 상호 작용할 수 있는 인터페이스로 변환한다.
- **서버**는 애플리케이션 코드를 저장하고, 클라이언트로부터 요청을 받고, 일부 연산을 수행한 후 적절한 응답을 다시 보내는 데이터 센터의 컴퓨터를 의미한다.
  각 환경에는 고유한 기능과 제약 조건이 있다. 예를 들어, 렌더링과 데이터 가져오기를 서버로 옮기면 클라이언트로 전송되는 코드의 양을 줄여 애플리케이션의 성능을 향상시킬 수 있다. 하지만 UI를 인터랙티브하게 만들려면 클라이언트에서 DOM을 업데이트해야 한다. 따라서 서버와 클라이언트에 대해 작성하는 코드가 항상 동일하지 않다. 특정 작업은 한 쪽 환경이 다른 환경보다 적합하다.

## Network Boundary
**Network boundary**는 다른 환경을 구분하는 개념적인 선을 의미한다.

React에서는 컴포넌트 트리에서 서버와 클라이언트의 Network boundary를 어디에 배치활지 선택한다. 예를 들어, 서버에서 데이터를 가져와서 사용자의 게시물을 렌더링한 다음 클라이언트에서 각 게시물에 대한 인터랙티브한 좋아요 버튼을 렌더링할 수 있다. 마찬가지로 레이아웃에서 페이지 간에 공유되는 네비게이션을 만들 수 있지만 링크에 대해서 활성 상태를 표시하기 위해서는 링크는 클라이언트에서 렌더링해야 한다.

컴포넌트는 두 개의 모듈 그래프로 나뉜다. 서버 모듈 그래프에는 서버에서 렌더링되는 모든 컴포넌트가 포함되고, 클라이언트 모듈 그래프에는 클라이언트에서 렌더링되는 모든 컴포넌트가 포함된다.

클라이언트로 전송되는 코드의 양을 줄여 어플리케이션 성능을 향상시키도록 Network boundary를 컴포넌트 트리의 잎사귀에 가깝게 이동하는 것이 좋다.

## Client Components
Next.js는 어플리케이션 성능 향상을 위해 기본적으로 서버 컴포넌트를 사용한다. 따라서 서버 컴포넌트를 사용하기 위해 추가적인 설정을 하지 않아도 된다. 상태가 필요하고 인터랙티브하게 동작하는 컴포넌트는 코드의 맨 앞에 `'use client'`를 작성하여 클라이언트 컴포넌트로 사용할 수 있다.

