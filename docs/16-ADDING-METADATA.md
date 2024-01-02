# 16 Adding Metadata
메타데이터는 SEO와 공유를 위해서 매우 중요하다.
## What is metadata?
웹에서 메타데이터는 페이지를 방문하는 사용자에게는 표시되지 않지만, 웹 페이지에 대한 추가적인 정보를 제공한다. 일반적으로 HTML의 `<head>` 요소 내에 포함되어 백그라운드에서 작동한다. 메타데이터가 제공하는 정보는 웹 페이지의 콘텐츠를 더 잘 이해해야 하는 검색엔진 등의 시스템에 매우 중요하다.

## Why is metadata important?
메타데이터는 검색 엔진과 소셜 미디어 플랫폼이 웹페이지에 더 쉽게 접근하고 이해할 수 있도록 웹페이지의 SEO를 향상시키는 데 중요한 역할을 한다. 적절한 메타데이터는 검색 엔진이 웹페이지를 효과적으로 인덱싱하여 검색 결과에서 웹페이지의 순위를 높이는 데 도움이 된다. 또한 오픈 그래프와 같은 메타데이터는 소셜 미디어에서 링크 공유를 개선하여 사용자를 편리하게 돕는다.

## Types of metadata
다양한 종류의 메타데이터가 있으며, 각각 고유한 용도로 사용된다.

**Title Metadata**: 브라우저 탭에 표시되는 웹페이지의 제목을 담당한다. 검색 엔진이 웹페이지의 내용을 이해하는데 도움이 되므로 SEO에 매우 중요하다.

```html
<title>Page Title</title>
```

**Description Metadata**: 웹페이지 콘텐츠에 대한 간략한 개요를 제공하며, 검색 엔진 결과에 표시되는 경우가 많다.

```html
<meta name="description" content="A brief description of the page content." />
```

**Keyword Metadata**: 웹페이지 콘텐츠와 관련된 키워드를 포함하여 검색 엔진이 페이지의 인덱스를 생성하는데 도움을 준다.

```html
<meta name="keywords" content="keyword1, keyword2, keyword3" />
```

**Open Graph Metadata**: 소셜 미디어 플랫폼에서 공유될 때, 웹페이지가 표시되는 방식을 개선하여 제목, 설명, 미리보기 이미지 등의 정보를 제공한다.

```html
<meta property="og:title" content="Title Here" />
<meta property="og:description" content="Description Here" />
<meta property="og:image" content="image_url_here" />
```

**Favicon Metadata**: 브라우저의 주소 표시줄이나 탭에 표시되는 파비콘을 웹페이지에 연결한다.

```html
<link rel="icon" href="path/to/favicon.ico" />
```

## Adding metadata
Next.js에는 메타데이터를 정의할 수 있는 Metadata API가 있다. 다음과 같은 두 가지 방식으로 사요할 수 있다.
- **Config-based**: 정적 메타데이터 객체 혹은 `layout.js`나 `page.js` 파일에서 동적으로 `generateMetadata` 함수를 export하여 사용
- **File-based**: Next.js에는 메타데이터 용도로 사용되는 특수 파일들이 존재한다.
    - `favicon.ico`, `apple-icon.jpg`, `icon.jpg`: 파비콘 및 아이콘에 사용
    - `opengraph-image.jpg`, `twitter-image.jpg`: 소셜 미디어 이미지에 사용
    - `robots.txt`: 검색 엔진 크롤링에 대한 지침 제공
    - `sitemap.xml`: 웹사이트 구조에 대한 정보 제공
      이러한 파일을 정적 메타데이터로 사용하거나, 프로젝트 내에서 프로그래밍 방식으로 생성할 수 있다. 두 가지 옵션을 모두 사용하여 Next.js는 페이지에 관련된 `<head>` 요소를 자동으로 생성한다.

### Favicon and Open Graph image
public 폴더에 `favicon.ico`와 `opengraph-image.jpg`가 있는 것을 확인할 수 있다. 이 이미지를 `/app` 폴더의 루트로 이동시키면, Next.js가 자동으로 이 파일을 식별하여 파비콘 및 OG 이미지로 사용한다. 개발자 도구에서 `<head>` 요소를 확인하면 이를 확인할 수 있다. 또한, `ImageResponse` 생성자를 사용하면 동적 OG 이미지를 만들 수도 있다.

### Page title and descriptions
`layout.js` 혹은 `page.js` 파일에서 메타데이터 객체를 포함하여 제목, 설명 등의 페이지 정보를 추가할 수도 있다. `layout.js`의 모든 메타데이터는 이를 사용하는 모든 페이지에서 상속된다. Next.js는 자동으로 애플리케이션에 제목과 메타데이터를 추가한다. 특정 페이지에 사용자 정의 제목을 추가하려면 페이지 자체에 메타데이터 객체를 추가하면 된다. 중첩된 페이지의 메타데이터는 상위 페이지의 메타데이터를 재정의한다. 또한 템플릿을 통해서 중복을 줄일 수 있다.