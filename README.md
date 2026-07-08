# Cyanotype Prompt Site

GitHub + Vercel 배포용 정적 사이트 패키지입니다.

## 폴더 구조

```txt
cyanotype-prompt-site/
  index.html
  data/
    presets.json
  images/
    thumbnails/
      .gitkeep
  _local_admin/
    admin.html
  .vercelignore
```

## 공개 사이트

Vercel에 배포되는 핵심 파일은 다음입니다.

```txt
index.html
data/presets.json
images/thumbnails/
```

`index.html`은 `data/presets.json`을 불러와 카드 UI를 만들고, 각 카드에서 Midjourney / Nano Banana / GPT Image용 프롬프트를 복사할 수 있게 합니다.

## 썸네일 추가 방법

1. 관리자 도구 또는 JSON에서 프리셋의 `slug`를 확인합니다.
2. 생성한 썸네일 이미지를 `webp`로 저장합니다.
3. 파일명을 slug와 맞춥니다.

예시:

```txt
slug:
atlantic-kelp-taxonomy

이미지 파일:
images/thumbnails/atlantic-kelp-taxonomy.webp
```

4. `data/presets.json`의 `thumbnailImage`에 아래처럼 경로를 넣습니다.

```json
"thumbnailImage": "images/thumbnails/atlantic-kelp-taxonomy.webp"
```

## 로컬 확인 방법

브라우저에서 `index.html`을 더블클릭하면 JSON fetch가 막힐 수 있습니다. 프로젝트 루트에서 아래 명령어로 로컬 서버를 실행하십시오.

```bash
python -m http.server 8000
```

그다음 접속:

```txt
http://localhost:8000
```

관리자 도구:

```txt
http://localhost:8000/_local_admin/admin.html
```

## 관리자 도구

`_local_admin/admin.html`은 배포용이 아니라 로컬 관리 도구입니다.

가능한 작업:

- Copy Thumbnail
- 추천 썸네일 경로 자동 입력
- 이미지 경로 저장
- 수정된 `presets.json` 내보내기

`.vercelignore`에 `_local_admin`이 들어 있어 Vercel 배포에서는 제외됩니다.

## GitHub + Vercel 배포 흐름

1. 이 폴더를 GitHub 저장소에 업로드합니다.
2. Vercel에서 해당 저장소를 Import합니다.
3. Framework Preset은 정적 사이트로 두면 됩니다.
4. 별도 Build Command 없이 배포합니다.
5. 썸네일을 추가하거나 JSON을 수정하면 GitHub에 push합니다.
6. Vercel이 자동으로 다시 배포합니다.

## 썸네일 파일명 체크리스트

`_local_admin/admin.html`에는 썸네일 파일명 체크리스트가 포함되어 있습니다.

관리자 도구에서 확인할 수 있는 항목:

- 전체 프리셋별 필요한 파일명
- 권장 JSON 경로
- 실제 이미지 파일 존재 여부
- 누락 파일만 보기
- 전체 파일명 복사
- 누락 파일명 복사
- 전체 경로 복사

권장 규칙은 다음과 같습니다.

```txt
프리셋 slug = 이미지 파일명
확장자 = .webp
폴더 = images/thumbnails/
JSON 경로 = images/thumbnails/slug.webp
```

예시:

```txt
images/thumbnails/atlantic-kelp-taxonomy.webp
images/thumbnails/moon-jar-indigo-shadow.webp
images/thumbnails/ceramic-cup-cyanotype-mockup.webp
```

## v7 변경사항

관리자 화면의 썸네일 파일명 체크리스트 영역을 개선했습니다.

- 안내 박스와 버튼 사이 여백 확대
- 버튼 줄과 표 사이 여백 확대
- 버튼 간격 확대
- 표 헤더 padding 확대
- 모바일에서 체크리스트 버튼 세로 정렬 개선
- `_local_admin/admin.html`을 직접 열어도 내장 데이터로 관리자 도구가 표시되도록 fallback 추가

다만 이미지 존재 여부 체크는 로컬 파일 접근 방식에 따라 브라우저 제한을 받을 수 있습니다. 가장 안정적인 확인 방법은 프로젝트 루트에서 아래 명령어를 실행하는 것입니다.

```bash
python -m http.server 8000
```

그다음 접속:

```txt
http://localhost:8000/_local_admin/admin.html
```

## v8 변경사항

공개용 `index.html`에도 내장 데이터 fallback을 추가했습니다.

이제 아래 두 경우 모두 프리셋 카드가 표시됩니다.

```txt
Vercel 배포 환경
→ data/presets.json을 불러와 표시

로컬에서 index.html 직접 열기
→ index.html 안에 내장된 fallback 데이터로 표시
```

운영 배포에서는 여전히 `data/presets.json`이 기준 데이터입니다.  
다만 로컬 확인이나 fetch 차단 상황에서도 빈 화면이 나오지 않도록 보완했습니다.

## v9 변경사항

공개용 사용자 화면에서 개발자용 안내 문구를 제거했습니다.

제거된 노출 문구:

- `50개의 프리셋 표시 중 / 전체 50개`
- `프리셋은 내장 데이터로 먼저 표시되고...`
- `data/presets.json` 관련 사용자 노출 설명

관리자 도구에는 관리에 필요한 정보가 유지됩니다.

## v10 변경사항

34번 프리셋의 Midjourney 프롬프트가 정책 위반으로 오탐될 수 있어 더 중립적인 표현으로 수정했습니다.

수정 방향:

- `Fashion designer` 제거
- `silk pieces` 제거
- `pinned` 제거
- `--style raw` 제거
- 인물·신체로 오해될 수 있는 결과를 막기 위해 `--no people, body parts, lingerie, swimwear` 추가

변경된 34번 프리셋명:

```txt
Cyanotype Fabric Sample Board
```

## v11 변경사항

34번 프리셋이 Midjourney에서 계속 차단되어 더 안전한 구조로 다시 수정했습니다.

수정 방향:

- `--no` 파라미터 전체 제거
- `people`, `body parts`, `lingerie`, `swimwear` 등 부정 프롬프트 단어 제거
- `fashion`, `textile`, `fabric realism` 등 오탐 가능성이 있는 맥락 축소
- 종이 샘플 카드 / 시아노타입 테스트 그리드로 변경

변경된 34번 프리셋명:

```txt
Cyanotype Sample Card Grid
```

새 Midjourney 프롬프트:

```txt
Indigo cyanotype sample card grid, blue handmade paper rectangles, white botanical sun-print marks, archival layout, clean studio lighting, matte paper texture, Prussian blue and cobalt tones --ar 4:5 --s 80
```

## v12 변경사항

34번 프리셋의 Midjourney 프롬프트를 실제 테스트에서 잘 동작한 짧은 버전으로 업데이트했습니다.

적용된 프롬프트:

```txt
Indigo cyanotype sample card grid, blue handmade paper rectangles, white botanical sun-print marks, archival layout, matte paper texture --ar 4:5
```

## v13 변경사항

썸네일 표시 로직을 수정했습니다.

기존 문제:

```txt
images/thumbnails/slug.webp 파일은 있음
→ 관리자 체크리스트에서는 Found
→ 하지만 data/presets.json의 thumbnailImage 값이 비어 있으면
→ 공개 index.html에서는 썸네일이 표시되지 않음
```

수정 후:

```txt
thumbnailImage 값이 있으면 해당 경로 사용
thumbnailImage 값이 비어 있으면 images/thumbnails/slug.webp 자동 시도
이미지가 있으면 표시
이미지가 없으면 기존 파란 플레이스홀더 표시
```

따라서 이제 `data/presets.json`에 썸네일 경로를 일일이 입력하지 않아도, 파일명이 slug와 같으면 썸네일이 자동으로 표시됩니다.

## v14 변경사항

관리자 화면에서도 썸네일 카드 이미지가 보이도록 경로 문제를 수정했습니다.

문제 원인:

```txt
공개 index.html 위치:
/
이미지 경로:
images/thumbnails/slug.webp
→ 정상

관리자 admin.html 위치:
/_local_admin/
이미지 경로:
images/thumbnails/slug.webp
→ /_local_admin/images/thumbnails/slug.webp 로 잘못 해석됨
```

수정 후 관리자 카드에서는 이미지 경로를 자동으로 아래처럼 변환합니다.

```txt
../images/thumbnails/slug.webp
```

따라서 관리자 체크리스트에서 `Found`로 뜨는 이미지는 관리자 카드 썸네일에도 표시됩니다.

## v15 변경사항

루트의 `og-image.png`를 사용할 수 있도록 `index.html`에 OG / Twitter 카드 메타태그를 추가했습니다.

추가된 대표 태그:

```html
<meta property="og:title" content="Cyanotype Prompt Library">
<meta property="og:description" content="50 Commercial Blue Presets for Image Models">
<meta property="og:image" content="/og-image.png">
<meta property="og:image:width" content="1280">
<meta property="og:image:height" content="630">
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:image" content="/og-image.png">
```

배포 후 공유 미리보기가 바로 갱신되지 않으면, 각 플랫폼의 캐시 때문에 시간이 걸릴 수 있습니다.
운영 도메인이 확정된 뒤에는 필요에 따라 `/og-image.png`를 `https://도메인/og-image.png` 형태의 절대 URL로 바꿔도 됩니다.
