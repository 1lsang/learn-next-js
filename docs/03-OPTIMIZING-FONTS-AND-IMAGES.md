# 03 Optimizing Fonts and Images
- `next/font`로 커스텀 폰트를 추가하는 방법
- `next/image`로 이미지를 추가하는 방법
- Next.js에서 폰트와 이미지를 최적화하는 방법

## Why optimize fonts?
폰트는 웹사이트의 디자인에서 중요한 역할을 하지만, 프로젝트에서 커스텀 폰트를 불러올 때 성능에 영향을 줄 수 있다. Layout Shift는 Google에서 웹사이트의 성능과 사용자 경험을 평가하는데 사용하는 지표이며, 브라우저가 처음에 fallback 혹은 시스템 폰트로 텍스트를 렌더링하고 커스텀 글꼴로 변경되면서 Layout Shift가 발생한다. 폰트가 교체되면서 텍스트 크기, 간격, 또는 레이아웃이 변경되어 주변요소가 이동하면서 발생하게 되는 것이다.

Next.js는 `next/font` 모듈을 사용하여 글꼴을 자동으로 최적화한다. 빌드 시점에 폰트를 다운로드하고 정적 에셋과 함께 호스팅한다. 즉, 어플리케이션을 방문할 때 성능에 영향을 줄 수 있는 추가 네트워크 요청이 없다.

## Adding a primary font
`/app/ui` 폴더에 `fonts.ts` 파일을 추가하고 Google 폰트를 불러올 수 있다.

```typescript
import { Inter } from 'next/font/google';
 
export const inter = Inter({ subsets: ['latin'] });
```

위와 같이 폰트를 불러오고, 다음과 같이 클래스에 추가해주는 것으로 폰트를 설정할 수 있다.

```tsx
import { inter } from '@/app/ui/fonts';
 
export default function RootLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      {/* antialiased 클래스는 폰트를  */}
      <body className={`${inter.className} antialiased`}>{children}</body>
    </html>
  );
}
```

## Why optimize images?

Next.js는 `/public` 폴더에 이미지와 같은 정적 에셋을 저장하여 제공할 수 있다. 이를 HTML에서 다음과 같이 사용할 수 있다.

```html
<img
  src="/hero.png"
  alt="Hero image"
/>
```

하지만 이렇게 사용할 경우 다음과 같은 부분들을 직접 설정해야 한다.
- 이미지가 다양한 화면 크기에서 반응하는지 확인
- 다양한 디바이스에 맞는 이미지 크기를 지정
- 이미지가 로드될 때 Layout Shift가 발생 방지
- 사용자 뷰포트 외부에 있는 이미지를 지연 로드

이미지 최적화를 수동으로 구현하는 대신 `next/image` 컴포넌트를 사용하여 이미지를 자동으로 최적화할 수 있다.

## The `<Image>` component
`<Image>` 컴포넌트는 HTML `<img>` 태그의 확장으로 다음과 같은 이미지 최적화 기능을 제공한다.
- 이미지가 로드될 때 Layout Shift의 발생 방지
- 뷰포트가 작은 기기에 큰 이미지가 전송되지 않도록 이미지 크기 조정.
- 이미지 지연 로딩(이미지가 뷰포트에 들어갈 때 이미지 로드).
- 브라우저에서 WebP 및 AVIF와 같은 최신 형식을 지원하는 경우 해당 이미지를 제공

## Adding the desktop hero image
`/public` 폴더에 있는 두 이미지를 디바이스에 따라 표시하는 예제
Layout shift를 피하기 위해서 `width`와 `height`는 소스 이미지와 동일한 비율로 설정하는 것이 좋다.
