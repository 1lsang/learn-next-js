# 13 Handling Errors
자바스크립트의 `try/catch`문과 Next.js API를 사용하여 에러를 처리하는 방법
## Adding `try/catch` to Server Actions
서버 액션에 `try/catch` 블록 외부에서 `redirect`를 호출한다. `redirect`는 에러를 발생시켜 작동하며, 이 에러는 `catch` 블록에 의해 포착된다. 이를 방지하기 위해 `try/catch` 구문 후에 `redirect`를 호출하여 `try` 블록이 성공한 경우에만 `redirect`에 도달할 수 있다.

이후 강제로 에러를 발생시키면 localhost에 에러가 표시된다. 에러를 확인하여 잠재적인 문제를 조기에 발견할 수 있으나, 갑작스러운 오류를 방지하고 어플리케이션을 계속 실행할 수 있도록 사용자에게 오류를 표시해야 한다면 Next.js의 `error.tsx` 파일을 사용할 수 있다.

## Handling all errors with `error.tsx`
`error.tsx` 파일은 route의 UI 바운더리를 정의하는 데 사용할 수 있다. 이 파일은 예상치 못한 오류에 대한 포괄적인 역할을 하고 사용자에게 fallback UI를 표시할 수 있다.

`error.tsx`는 다음과 같은 특징을 가진다.

- `error.tsx`는 클라이언트 컴포넌트여야 한다.
- 다음과 같은 props를 허용한다.
    - `error`: 자바스크립트의 기본 Error 객체의 인스턴스
    - `reset`: 에러 바운더리를 재설정하는 함수로, 이 함수가 실행되면 route 세그먼트 재렌더링을 시도한다.

## Handling 404 errors with the `notFound` function
오류를 처리하는 또 다른 방법은 `notFound` 함수를 사용하는 것이다. `error.tsx`는 모든 오류를 포착하는 데 유용한 반면, `notFound`는 존재하지 않는 리소스를 가져올 때 사용할 수 있다.

데이터를 불러오지 못하는 경우, 조건문을 통해 `notFound` 함수를 호출하여 not found 페이지를 렌더링할 수 있다.

```tsx
import { notFound } from "next/navigation";

export default async function Page({ params }: { params: { id: string } }) {
	// ...
    if (!invoice) {
        notFound();
    }
    // ...
}
```

위와 같이 `notFound`를 호출할 경우, 같은 경로 내의 `not-found.tsx` 파일의 오류 UI를 화면에 표시할 수 있다. `notFound`는 `error.tsx`보다 우선하므로 보다 구체적인 오류를 처리하고 싶을 때 사용할 수 있다.
