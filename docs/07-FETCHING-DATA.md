# 07 Fetching Data
## Choosing how to fetch data
어플리케이션에 데이터를 받아오는 방법은 여러가지가 있다.
### API layer
API는 어플리케이션 코드와 데이터베이스 사이의 중간 계층이다.
- API를 제공하는 타사 서비스를 사용하는 경우
- 클라이언트에서 데이터를 가져올 때, 데이터베이스가 클라이언트에 노출되지 않도록 실행되는 API 계층 필요
  Next.js에서는 Route Handler를 사용하여 API 엔드포인트를 생성할 수 있다.

### Database queries
풀스택 어플리케이션을 만드려면, 데이터베이스와 상호작용한는 로직이 필요할 것이다. Postgres와 같은 관계형 데이터베이스의 경우, SQL이나 Prisma와 같은 ORM을 사용할 수 있다.
- API 엔드포인트를 만들 때 데이터베이스와 상호작용하는 로직을 작성해야 한다.
- 서버에서 데이터를 불러오는 React 서버 컴포넌트를 사용할 때는, API 계층을 건너 뛰고 데이터베이스를 클라이언트에 노출할 위험 없이 데이터베이스를 직접 쿼리할 수 있다.

### Using Server Components to fetch data
기본적으로 Next.js 어플리케이션은 React 서버 컴포넌트를 사용한다. 서버 컴포넌트로 데이터를 패칭하는 것은 새로운 접근 방식이며, 몇가지 이점을 가진다.
- 서버 컴포넌트는 프로미스를 지원하여 데이터 패칭과 같은 비동기 작업을 위해 간단한 솔루션을 제공한다. `useEffect`, `useState`나 데이터 패칭 라이브러리를 사용하지 않고도 `async/await` 구문을 사용할 수 있다.
- 서버 컴포넌트는 서버에서 실행되어 비용이 많이 드는 데이터 패칭 및 로직을 서버에 보관하고 결과만 클라이언트로 전송할 수 있다.
- 서버 컴포넌트는 서버에서 실행되기 때문에 API 레이어 없이 직접 데이터베이스에 접근할 수 있다.

### Using SQL
이 튜토리얼은 Vercel Postgres SDK와 SQL을 사용하여 데이터베이스 쿼리를 작성한다. SQL은 관계형 DB 쿼리의 업계 표준이며, 이에 대한 기본적인 지식을 다른 도구에도 적용할 수 있다. 또한, 특정 데이터를 가져오고 조작할 수 있으며 Vercel Postgres SDK는 SQL 인젝션에 대해서 보호 기능을 제공한다.

## Fetching data for the dashboard overview page
페이지를 비동기 컴포넌트(async component)로 만들어 `await`를 통해 데이터를 가져올 수 있다. 이러한 방법을 사용해서 페이지의 여러 컴포넌트에 데이터를 불러올 수 있는데, 주의해야할 점이 있다.
- `async/await` 구문은 blocking하여 실행되기 때문에 request waterfall을 생성한다.
- 기본적으로 Next.js는 성능 개선을 위해 경로를 미리 렌더링하고 이를 정적 렌더링이라고 부른다. 정적 렌더링을 사용할 경우, 데이터가 변경되어도 대시보드에 반영되지 않는다.

## What are request waterfalls?
데이터 패칭 요청의 경우 각 요청은 이전 요청이 데이터를 반환한 이후에 순차적으로 시작된다. Waterfall은 이전 요청의 완료 여부에 따라 달라지는 네트워크 요청을 의미한다.
이러한 패턴이 항상 나쁜 것은 아니다. 먼저 사용자의 ID를 가져온 후 ID를 기반으로 해당 사용자의 정보를 가져오기 위해서는 먼저 ID를 요청해야 하기 때문에 waterfall 방식 또한 필요하다. 하지만 의도하지 않은 채 사용할 경우 성능에 영향을 줄 수 있다.

## Parallel data fetching
일반적으로 Waterfall을 피하기 위해서 모든 데이터 요청을 동시에 병렬로 시작한다.

자바스크립트에서는 `Promise.all()` 혹은 `Promise.allSettled()` 함수를 사용하여 모든 프로미스를 동시에 시작할 수 있다. 실제로 프로젝트의 `fetchCardData()` 함수에서 `Promise.all()`을 사용하는 것을 확인할 수 있다.

이는 다음과 같은 장점을 가진다.
- 모든 데이터 패칭을 동시에 실행하여 성능을 향상시킬 수 있다.
- 모든 라이브러리와 프레임워크에서 사용할 수 있는 패턴이다.

하지만 이러한 패턴을 사용할 때, 하나의 데이터 요청이 다른 모든 데이터 요청보다 매우 느리면 다른 모든 데이터 요청 또한 지연된다.