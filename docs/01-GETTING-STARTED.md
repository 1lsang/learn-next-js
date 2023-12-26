# 01 Getting Started

기존 코드베이스를 활용하여 Next.js를 학습한다. 이 코드베이스의 대부분은 이미 작성되어 있던 코드이다.

## Directory
- `/app`: 모든 routes, 컴포넌트, 로직을 포함하고 있는 디렉토리
    - `/app/lib`: 재사용할 수 있는 유틸리티 함수나 데이터 패칭 함수를 포함하고 있는 디렉토리
    - `/app/ui`: UI 컴포넌트를 포함하고 있는 디렉토리
- `/public`: 이미지와 같은 정적 에셋 파일들을 포함하고 있는 디렉토리
- `/scripts`: 데이터 베이스를 채우는 Seeding 스크립트를 포함하는 디렉토리
- **Config Files**: `next.config.js`와 같은 Next.js의 설정 파일들

## Placeholder data
UI를 구성할 때, Placeholder 데이터가 필요하다. DB나 API가 사용불가능하다면 다음과 같은 방법을 사용한다.
- JSON 형식 혹은 자바스크립트 객체로 Placeholder 데이터를 사용한다.
- mockAPI와 같은 써드파티 서비스를 사용한다.
  이 프로젝트에서는 `/app/lib/placeholder-data.js`에 있는 Placeholder 데이터를 사용한다.