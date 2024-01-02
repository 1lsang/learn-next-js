# 15 Adding Authentication

## What is authentication?
인증(authentication)은 웹 어플리케이션의 시스템에서 사용자가 본인이 맞는지 확인하는 방법이다. 보안 웹사이트에서는 사용자의 신원을 확인하기 위해 여러 가지 방법을 사용한다. 사용자의 이름과 비밀번호를 입력한 후 인증 코드를 디바이스로 보내거나 Google OTP와 같은 외부 앱을 사용할 수 있다. 누군가 내 비밀번호를 알아내더라도 고유한 토큰 없이 계정에 접근할 수 없으므로 이러한 2단계(2FA) 인증은 보안을 강화하는데 도움이 된다.

## Authentication vs Authorization
인증과 권환 부여는 각각 다른 역할을 한다.
- 인증(Authentication): 사용자의 신원을 확인한다. 사용자 이름, 비밀번호 등의 정보를 통해 신원을 증명하는 단계이다.
- 권한 부여(Authorization): 인증의 다음 단계로, 신원이 확인되면 권한 부여를 통해 어플리케이션의 어떤 부분을 사용할 수 있는지 결정한다.
  인증은 사용자가 누구인지 확인하는 단계, 권한 부여는 어플리케이션에서 할 수 있는 작업을 결정한다.

## NextAuth.js
어플리케이션에 인증을 추가하기 위해 NextAtuth.js를 사용할 수 있다. NextAuth.js는 세션 관리, 로그인 및 로그아웃 등 인증과 관련된 복잡한 부분을 추상화하여 프로세스를 간소화한다. 이러한 부분들은 직접 구현할 경우, 시간이 많이 걸리고 오류가 발생하기 쉽다. NextAuth.js는 Next.js 어플리케이션에서 인증을 위한 통합 솔루션을 제공한다.

## Setting up NextAuth.js
NextAuth.js를 설치하고(Next 14와의 호환성을 고려해 beta로 설치), 다음 명령어를 사용하여 어플리케이션의 비밀 키를 생성한다. 해당 비밀키는 `.env` 파일에 `AUTH_SECRET` 변수로 추가한다.

```bash
openssl rand -base64 32
```

### Adding the pages option
프로젝트의 루트에서 `authConfig` 객체를 내보내는 `auth.config.ts` 파일을 만든다. 이 객체에는 NextAuth.js에 대한 구성 옵션이 포함된다.

먼저 `pages` 옵션을 사용하여 사용자 정의 로그인, 로그아웃, 오류 페이지의 경로를 지정할 수 있고, 페이지 옵션을 추가하면 NextAuth.js의 기본 페이지가 아닌 사용자 정의 로그인 페이지로 리디렉션된다.

## Protecting your routes with Next.js Middleware
다음으로 경로를 보호하는 로직을 추가한다. 사용자가 로그인하지 않은 경우 대시보드 페이지에 접근할 수 없도록한다.

```ts
import type { NextAuthConfig } from 'next-auth';
 
export const authConfig = {
  pages: {
    signIn: '/login',
  },
  callbacks: {
    authorized({ auth, request: { nextUrl } }) {
      const isLoggedIn = !!auth?.user;
      const isOnDashboard = nextUrl.pathname.startsWith('/dashboard');
      if (isOnDashboard) {
        if (isLoggedIn) return true;
        return false; // Redirect unauthenticated users to login page
      } else if (isLoggedIn) {
        return Response.redirect(new URL('/dashboard', nextUrl));
      }
      return true;
    },
  },
  providers: [], // Add providers with an empty array for now
} satisfies NextAuthConfig;
```

`authorized` 콜백은 미들웨어를 통해 요청이 페이지에 접근할 수 있는 권한이 있는지 확인하는데 사용한다. 요청이 완료되지 전에 호출되며, `auth`와 `request` 속성을 가진 객체를 받는다.

`providers` 옵션은 다양한 로그인 옵션을 나열하는 배열이다.

authConfig 객체를 미들웨어 파일로 가져오기 위해서 root에 `middleware.ts` 파일을 만든다.

```ts
import NextAuth from 'next-auth';
import { authConfig } from './auth.config';
 
export default NextAuth(authConfig).auth;
 
export const config = {
  matcher: ['/((?!api|_next/static|_next/image|.*\\.png$).*)'],
};
```

`authConfig` 객체를 사용하여 NextAuth.js를 초기화하여 `auth` 속성을 export한다. 또한, 미들웨어의 `matcher` 옵션을 사용하여 특정 경로에서 실행되도록 지정하고 있다. 미들웨어가 인증을 확인할 때까지 보호되어 있는 경로가 렌더링되지 않아 보안과 성능이 모두 향상된다.

### Password hashing
비밀번호는 데이터베이스에 저장하기 전, 해싱을 해야 한다. 해싱은 비밀번호를 길이가 같은 무작위 문자열로 변환하여 사용자의 데이터가 노출되어도 보안을 유지할 수 있는 추가적인 계층이 된다.
기존 데이터베이스에 `bcrypt` 패키지를 통해 비밀번호를 해싱해두었으므로 해당 패키지를 사요해야 하는데, 이는 Node.js의 API에 의존하여 Next.js의 미들웨어에서 사용할 수 없다. 따라서 `bcrypt` 패키지를 위해 별도의 파일을 만들어야 한다.

`auth.ts`를 만들어 `authConfig` 객체를 spreads 한다.
```ts
import NextAuth from 'next-auth';
import { authConfig } from './auth.config';
 
export const { auth, signIn, signOut } = NextAuth({
  ...authConfig,
});
```

### Adding the Credential provider
NextAuth.js에 `providers` 옵션을 추가한다. Google, Github 등 다양한 로그인 옵션을 나열하는데, Credential을 추가하면 `Credential` provider를 사용할 수 있다. 일반적으로는 OAuth와 같은 provider나 이메일 provider를 사용하는 것이 좋다.

### Adding the sign in functionality
`authorize` 함수를 사용하여 인증 로직을 처리할 수 있다. 서버 작업처럼 `zod`를 사용하여 이메일과 비밀번호의 유효성을 검사하고 데이터베이스에 사용자가 존재하는지 확인하면 된다.
Credential 검증 이후에는 데이터베이스에서 사용자를 받아오고, `bcrypt.compare`를 통해 비밀번호가 맞는지 확인한다. 일치한다면 `user`를 아니라면 `null`을 반환한다.

### Updating the login form
인증 로직을 로그인 form과 연결하기 위해 `actions.ts` 파일에 `authenticate` 함수를 만들고 `auth.ts`에서 로그인 함수를 가져온다.

`'CredentialsSignin'` 오류가 발생할 경우, 적절한 오류 메시지를 표시하는 것이 좋다. 마지막으로 `login-form.tsx` 컴포넌트에서 React의 `useFormState`를 사용하여 서버 액션을 호출하고 form 오류를 처리하고, `useFormStatus` 훅을 사용하여 form의 보류 중인 상태를 처리할 수 있다.

## Adding the logout functionality
로그아웃 기능은 `<SideNav>` 컴포넌트 내에 `signOut` 함수를 가져와 form에 연결해주면 된다. 

