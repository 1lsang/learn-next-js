# 12 Mutating Data
- React 서버 액션과 이를 사용하여 데이터를 변경하는 방법
- `form`과 서버 컴포넌트를 사용하는 방법
- 타입 유효성 검사를 포함해서 `formData` 객체를 사용하는 Best practice
- `revalidatePath` API를 사용해서 클라이언트 캐시를 재검증하는 방법
- 아이디를 사용하여 동적 결로를 생성하는 방법
- Optimistic 업데이트를 위한 React의 `useFormStatus` 훅을 사용하는 방법

## What are Server Actions?
React 서버 액션을 통해 서버에서 직접 비동기 코드를 실행할 수 있다. 데이터를 변경하기 위해 API 엔드포인트를 만들 필요없이 서버에서 실행되는 비동기 함수를 작성하고 클라이언트 또는 서버 컴포넌트에서 호출할 수 있다.

웹 어플리케이션은 다양한 위협에 취약하여 보안이 최우선이 되어야 하며, 이 때 서버 액션을 사용하면 좋다. 서버 액션은 다양한 유형의 공격으로부터 데이터를 안전하게 보호하며, 권한이 부여된 액세스를 보장하는 보안 솔루션을 제공한다. POST 요청, encrypted closures, 엄격한 입력 확인, 에러 메시지 해싱, 호스트 제한 등을 통해 앱의 안정성을 크게 향상 시킨다.

## Using forms with Server Actions
React에서는 `<form>` 요소의 `action` 속성으로 액션을 호출할 수 있고, `action`은 캡처된 데이터가 포함된 네이티브 `FormData` 객체를 자동으로 받는다.

**예시**
```jsx
// Server Component
export default function Page() {
  // Action
  async function create(formData: FormData) {
    'use server';
 
    // Logic to mutate data...
  }
 
  // Invoke the action using the "action" attribute
  return <form action={create}>...</form>;
}
```

서버 컴포넌트 내에서 서버 액션을 호출하면 클라이언트에서 자바스크립트가 비활성화되어 있어도 폼이 작동 가능하다는 장점이 있다.

## Next.js with Server Actions
서버 액션은 Next.js 캐싱과도 연관되어 있다. `form`이 서버 액션을 통해 제출되면 액션을 사용하여 데이터를 변경할 수 있을 뿐만 아니라 `revalidatePath` 및 `revalidateTag`와 같은 API를 사용하여 관련 캐시의 유효성을 다시 검사할 수도 있다.

## Creating an invoice
1. 사용자의 입력을 캡처할 form을 만든다.
2. 서버 액션을 만들고 form에서 호출한다.
3. 서버 액션 내에서 formData 객체에서 데이터를 추출한다.
4. 데이터베이스에 삽입할 데이터의 유효성을 검사하고 준비한다.
5. 데이터를 삽입하고 오류를 처리한다.
6. 캐시의 유효성을 다시 검사하고 사용자를 인보이스 페이지로 다시 리디렉션한다.

### 1. Create a new route and form
새로운 경로를 만들고 form 요소를 적절하게 배치한다.

### 2. Create Server Action
`/app/lib/action.ts` 등의 이름으로 form이 제출될 때 호출할 서버액션을 만든다. 이 때, 파일의 맨 위에 React의 `use server` 지시문을 추가한다. `'use server'` 지시문을 추가하면 파일 내에서 내보낸 모든 함수를 서버 함수로 표시하며, 이를 클라이언트 및 서버 컴포넌트로 가져올 수 있다. 만든 서버 액션을 `form` 요소의 `action` 속성으로 전달하여 연결한다.

> [!TIP] Good to know
> HTML에서는 `action` 속성에 form 데이터를 전달해야하는 URL을 전달한다. 하지만 React에서는 `action` 속성을 특별한 props이며, React가 `action`을 호출할 수 있도록 그 위에 빌드된다.
> 내부에서 서버 액션은 POST API 엔드포인트를 생성하므로 서버 액션을 사용할 API 엔드포인트를 수동으로 만들 필요가 없다.

### 3. Extract the data from `formData`
`formData`에서 필요한 값을 추출해야 한다. `get` 메서드를 사용하여 값을 추출한다. form 데이터가 많을 경우, `const rawFormData = Object.fromEntries(formData.entries())`와 같은 방법을 사용할 수도 있다.

### 4. Validate and prepare the data
form 데이터를 데이터베이스로 보내기 전에 데이터가 올바른 형식과 올바른 유형으로 되어 있는지 확인해야 한다.
#### 타입 유효성 검사 및 강제
실제로 `type="number"`인 데이터도 문자열을 반환한다. 이 때, 유형 유효성 검사 라이브러리(ex. Zod)를 사용할 수 있다. 서버 액션 함수에 Zod를 가져와서 form object의 모양과 일치하는 스키마를 정의하고, 이 스키마를 formData를 데이터베이스에 저장하기 전에 유효성을 검사합니다.

#### 데이터형 변환
일반적으로 데이터베이스에 화폐 값을 센트 단위로 저장하는 것으로 자바스크립트 부동소수점 오류를 없애고 정확도를 높일 수 있다. 이처럼 필요한 경우 데이터 타입을 변환하여 데이터베이스에 저장한다.

#### 필요한 데이터 추가
날짜와 같이 데이터를 저장할 때 필요한 데이터를 추가한다.

### 5. Inserting the data into your database
서버 액션 내에 sql 쿼리를 추가하여 데이터베이스에 데이터를 추가할 수 있도록 한다.

### 6. Revalidate and redirect
Next.js에는 사용자 브라우저에 일정 시간 동안 경로 세그먼트를 저장하는 클라이언트 측 라우터 캐시가 있다. 이 캐시는 prefetching과 함께 사용자가 서버에 대한 요청 횟수를 줄이면서 경로 사이를 빠르게 탐색할 수 있도록 돕는다.

특정 경로에 표시되는 데이터를 업데이트할 때, 그 경로에 대한 캐시를 지우고 서버에 대한 새 요청을 트리거하는 것은 `revalidatePath` 함수를 사용하여 수행할 수 있다.

데이터베이스가 업데이트되면 해당 경로의 유효성이 재검사되어 서버에서 새 데이터를 가져온다. 또한, 필요하다면 `redirect` 함수를 통해 경로를 redirect한다.

## Updating an invoices
업데이트는 인보이스 만들기 양식과 비슷하지만 인보이스 ID를 전달하여야 데이터베이스를 업데이트할 수 있다.
1. 인보이스 ID가 포함된 동적 경로를 만든다.
2. 페이지 매개변수에서 인보이스 ID를 불러온다.
3. 데이터베이스에서 특정 인보이스를 가져온다.
4. form에 인보이스 데이터를 채운다.
5. 데이터베이스에서 인보이스 데이터를 업데이트합니다.
### 1. Create a Dynamic Route Segment with the invoice `id`
정확한 경로 이름을 모르거나 데이터를 기반으로 경로를 만들고자 할 때, 동적 경로를 만들 수 있다. 블로그 게시물 제목, 제품 페이지 등이 이에 해당한다. 폴더의 이름을 대괄호로 묶어 동적 경로 세그먼트를 만들 수 있다. (예시: `[id]`, `[post]`, `[slug]`)
### 2. Read the invoice `id` from page `params`
수정 페이지의 경우, 기존 데이터를 불러와야 한다. 이를 위해서 `id`값이 필요하다. 페이지 컴포넌트의 props로 `params`를 받을 수 있고, 이 객체를 통해서 `id`에 접근할 수 있다.
### 3. Fetch the specific invoice
id를 파라미터로 전달하여 특정 인보이스를 불러온다.

> [!TIP] UUID vs. Auto-incrementing Keys
> 증가하는 키(예: 1, 2, 3 등) 대신 UUID를 사용하면 URL이 길어지지만, ID 충돌의 위험을 없애고 전 세계적으로 고유하며 enumeration attacks의 위험을 줄여 대규모 데이터베이스에 이상적이다.

### 4. Pass the `id` to the Server Action
데이터베이스에서 레코드를 업데이트할 수 있도록 서버 액션에 ID를 전달해야 한다.  하지만 다음과 같이 서버 액션에 ID를 전달하면 안된다.

```tsx
<form action={updateInvoice(id)}>
```

대신 JS 바인딩을 사용하여 서버 액션에 id를 전달할 수 있습니다. 이렇게 하면 서버 액션에 전달된 모든 값이 인코딩됩니다.

```tsx
// ...
import { updateInvoice } from '@/app/lib/actions';
 
export default function EditInvoiceForm({
  invoice,
  customers,
}: {
  invoice: InvoiceForm;
  customers: CustomerField[];
}) {
  const updateInvoiceWithId = updateInvoice.bind(null, invoice.id);
 
  return (
    <form action={updateInvoiceWithId}>
      <input type="hidden" name="id" value={invoice.id} />
    </form>
  );
}
```

## Deleting an invoice
삭제의 경우도 마찬가지로 서버액션에 `bind`를 통해 `id`를 전달하여 삭제한다. 
