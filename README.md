# HODU - 반응형 랜딩 페이지

## 📋 프로젝트 개요

**HODU**는 고양이 테마의 반응형 랜딩 페이지입니다. 데스크탑과 모바일 환경까지 사용자 경험을 제공합니다.

## 📊 프로젝트 구조

```
landing-page/
├── index.html          # 메인 HTML 파일
├── css/
│   ├── reset.css       # CSS 리셋
│   └── index.css       # 메인 스타일시트
├── img/                # 이미지 에셋
│   ├── logo.svg
│   ├── cat-subscribe.png
│   └── ...
└── README.md           # 프로젝트 문서
```

## 🎯 주요 기능

### 반응형 디자인

- **데스크탑 (768px 이상)**
- **모바일 (768px 이하)**
- **CSS Flexbox 활용**

## 🛠 기술 스택

### Frontend

- **HTML5**: 시맨틱 마크업
- **CSS3**:
  - CSS Grid & Flexbox
  - CSS Variables (Custom Properties)
  - Media Queries
  - CSS Reset

### 디자인 시스템

- **CSS 변수**: 일관된 색상 및 크기 관리
- **반응형 타이포그래피**: 뷰포트에 따른 폰트 크기 조정
- **모던 UI/UX**: 그림자, 둥근 모서리

## 🎨 디자인 시스템

### 색상 팔레트

```css
:root {
  --color-bg-main: #263140; /* 메인 배경색 */
  --color-btn-main: #d98c5f; /* 버튼 색상 */
  --color-white: #fff; /* 흰색 */
  --color-black: #000; /* 검은색 */
}
```

### 타이포그래피

- **제목 (H2, H3)**: 24px (모바일), 48px (데스크탑)
- **본문**: 16px
- **버튼 텍스트**: 16px

### 간격 시스템

- **메뉴 간격**: 60px
- **섹션 간격**: 80px
- **요소 간격**: 30px

## 🔧 개발 과정

### 1. 기획 및 디자인

- **레이아웃 구조 설계**
- **반응형 브레이크포인트 정의**: 768px 기준
- **컴포넌트 설계**: 버튼, 네비게이션 같은 UI 요소들을 미리 정의해서 일관성 있고 재사용 가능하게 UI 요소 정의

### 2. HTML 구조화

- **시맨틱 마크업**: header, main, section, footer, dialog 활용
- **접근성 고려**: alt 텍스트, 적절한 제목 구조화
- **SEO 최적화**: 메타 태그, 제목 구조화

### 3. 모바일 메뉴 구현 - 주요 어려움과 해결방법

#### 🚨 처음에 겪은 문제들

```
❌ 텍스트가 안 보여요!
❌ 화면 크기에 따라 위치가 달라져요!
```

#### 🔍 왜 이런 문제가 생겼을까?

**문제 1: 오른쪽 붙이기가 생각보다 복잡했어요**

```css
/* 처음 시도 */
.mobile-menu-dialog[open] {
  right: 0; /* 이거만으로는 안 됐어요 */
}
```

→ `right: 0`만 설정해도 다른 CSS 규칙들이 덮어써서 제대로 적용되지 않았어요.

**문제 2: 텍스트가 숨겨져 있었어요**

```css
/* 헤더의 모든 nav와 button */
header nav,
header button {
  text-indent: -9999px; /* "텍스트를 화면 밖으로 밀어내!" */
}
```

→ 이 스타일이 모바일 메뉴의 텍스트까지 숨겨버렸어요.

**문제 3: 위치가 화면 크기에 따라 달라졌어요**

```css
left: 50px; /* 왼쪽에서 50px 떨어뜨려 */
```

→ 큰 화면에서는 괜찮지만, 작은 화면에서는 위치가 이상해졌어요.

#### ✅ 어떻게 해결했을까?

**해결책 1: 텍스트 보이게 하기**

```css
/* 모바일 메뉴의 nav만 따로 처리 */
.mobile-menu-dialog nav {
  text-indent: 0; /* "텍스트 보여줘!" */
}

/* 모바일 메뉴의 버튼도 따로 처리 */
.mobile-menu-dialog .mobile-menu-btn {
  text-indent: 0;
  background-image: none; /* 배경 이미지 제거 */
}
```

**해결책 2: 화면 크기와 상관없이 일정하게**

```css
:root {
  --menu-width: 80%; /* 화면의 80% */
  --menu-max-width: 350px; /* 최대 350px */
}

.mobile-menu-dialog[open] {
  width: var(--menu-width);
  max-width: var(--menu-max-width);
  right: 0; /* 항상 오른쪽 끝 */
}
```

✅ 텍스트가 잘 보여요!
✅ 화면 크기와 상관없이 일정해요!

```

### 4. 모바일 메뉴 오버레이 구현 - 추가 문제와 해결

#### 🚨 메뉴 모달창 배경

```

❌ 오버레이가 안 보여요!
❌ CSS 선택자가 작동하지 않아요!
❌ dialog 뒤에 흰색 배경이 안 생겨요!

````

#### 🔍 문제 원인 분석

**문제: CSS 선택자와 HTML 구조 불일치**

```html
<!-- HTML 구조 -->
<div class="mobile-menu-overlay"></div>
<!-- 오버레이가 먼저 -->
<dialog class="mobile-menu-dialog" open><!-- dialog가 나중에 --></dialog>
````

```css
/* 처음 시도한 CSS */
.mobile-menu-dialog[open] ~ .mobile-menu-overlay {
  display: block; /* 작동하지 않음 */
}
```

**원인**:

- `~` 선택자는 **형제 요소 중 뒤에 오는 요소**를 선택하는데
- HTML에서는 오버레이가 dialog **앞에** 있어서 선택되지 않았음

#### ✅ 해결방법

**해결책 1: 올바른 CSS 선택자 사용**

```css
.mobile-menu-overlay {
  display: none;
  position: fixed;
  top: 0;
  left: 0;
  width: 100vw;
  height: 100vh;
  background-color: white;
  z-index: 1999;
}

/* dialog가 열려있을 때 오버레이 보이기 */
.mobile-menu-dialog[open] ~ .mobile-menu-overlay {
  display: block !important;
}
```

**해결책 2: 더 강력한 선택자 추가**

```css
/* body:has() 선택자로 더 강력하게 */
body:has(.mobile-menu-dialog[open]) .mobile-menu-overlay {
  display: block !important;
}
```

#### 🎯 추가 학습점

**1. CSS 선택자 방향이 중요해요**

- `~` (형제 선택자): 뒤에 오는 형제 요소 선택
- `+` (인접 형제 선택자): 바로 뒤에 오는 형제 요소 선택
- `:has()`: 특정 조건을 만족하는 부모 요소 선택

**2. body:has() 선택자란?**

**간단한 개념:**
"body 안에 특정 요소가 있으면, 다른 요소에 스타일을 적용해줘"

**실제 예시:**

```css
/* "body 안에 열린 모바일 메뉴가 있으면, 오버레이를 보여줘" */
body:has(.mobile-menu-dialog[open]) .mobile-menu-overlay {
  display: block;
}
```

**왜 이걸 썼을까?**

- ❌ **기존 방법**: HTML 순서에 의존적 (오버레이가 dialog 뒤에 있어야 함)
- ✅ **body:has()**: HTML 순서 상관없이 작동 (어디에 있든 상관없음)

**발표용 한 줄 요약:**

> "body:has()는 부모가 특정 자식을 가지고 있는지 확인해서 스타일을 적용하는 강력한 CSS 선택자입니다"

**추가 발견:**

```css
/* !important 없이도 충분히 작동! */
body:has(.mobile-menu-dialog[open]) .mobile-menu-overlay {
  display: block; /* !important 없어도 OK */
}
```

**왜 !important가 필요 없었을까?**

- `body:has()` 선택자는 **매우 강력한 선택자**입니다
- 다른 CSS 규칙들을 쉽게 덮어쓸 수 있어요
- 불필요한 `!important` 사용을 줄일 수 있어요

#### 🎉 최종 결과

```
✅ 오버레이가 dialog 뒤에 제대로 보여요!
✅ 전체 화면을 흰색으로 덮어요!
✅ CSS 선택자가 정상 작동해요!
✅ 모바일 메뉴가 완벽하게 구현되었어요!
```

### 5. 모바일 이미지 슬라이드 구현 방법

#### 🎯 발표용 한 줄 요약

> "모바일에서 이미지들이 가로로 스크롤되는 슬라이드 효과를 CSS로 구현했습니다"

#### 📱 어떻게 만들었나요?

**1. 기본 아이디어**

```
여러 이미지를 가로로 나열하고 → 모바일에서는 가로 스크롤로 슬라이드
```

**2. HTML 구조**

```html
<div class="img-wrapper">
  <img src="./img/img_1.jpg" alt="캣타워 호두" />
  <img src="./img/img_2.jpg" alt="호두 뒷모습" />
  <img src="./img/img_3.jpg" alt="호두 앞모습" />
</div>
```

**3. CSS 코드 (핵심 부분)**

```css
.section-middle .img-wrapper {
  display: flex;
  flex-direction: row;
  gap: 72px; /* 이미지 간격 */
}

/* 모바일에서 슬라이드 효과 */
@media (max-width: 768px) {
  .section-middle .img-wrapper {
    justify-content: flex-start;
    overflow-x: auto; /* 가로 스크롤 활성화 */
    scroll-snap-type: x mandatory; /* 스크롤 스냅 효과 */
    padding: 20px;
    width: 100%;
  }

  .section-middle .img-wrapper img {
    flex-shrink: 0; /* 이미지 크기 고정 */
    scroll-snap-align: center; /* 스크롤 시 중앙 정렬 */
  }
}
```

#### 🎨 핵심 CSS 속성 설명

**1. `overflow-x: auto`**

- 가로 방향으로 내용이 넘칠 때 스크롤바 생성
- 사용자가 손가락으로 좌우 스크롤 가능

**2. `scroll-snap-type: x mandatory`**

- 스크롤할 때 이미지가 중앙에 딱 맞춰서 정렬됨
- 부드러운 슬라이드 효과 제공

#### 🔍 `x mandatory` 뜻 설명

**`x` = 가로 방향**

- x축(가로)으로만 스크롤 스냅 적용
- y축(세로) 스크롤에는 영향 없음

**`mandatory` = 강제 적용**

- 스크롤을 멈추면 **반드시** 가장 가까운 스냅 포인트로 이동
- 사용자가 어디서 스크롤을 멈춰도 자동으로 정렬됨

**다른 옵션들과 비교:**

```css
scroll-snap-type: x mandatory; /* 강제로 가로 스냅 */
scroll-snap-type: x proximity; /* 가까우면만 스냅 (선택적) */
scroll-snap-type: y mandatory; /* 세로 방향 강제 스냅 */
scroll-snap-type: none; /* 스냅 효과 없음 */
```

**발표용 한 줄 요약:**

```
"x는 가로 방향, mandatory는 강제 적용이라는 뜻으로, 가로로 스크롤할 때 반드시 이미지에 딱 맞춰서 정렬되도록 하는 설정입니다"
```

**4. `scroll-snap-align: center`**

- 스크롤 시 이미지가 중앙에 정렬됨

#### 🎨 스크롤바 숨기기

**문제: 스크롤바가 보여요!**

```
❌ 가로 스크롤할 때 스크롤바가 보임 (어색함)
❌ 모바일에서 스크롤바가 UI를 방해함
```

**해결방법:**

```css
/* 웹킷 기반 브라우저 (Chrome, Safari) */
.section-middle .img-wrapper::-webkit-scrollbar {
  display: none; /* 스크롤바 숨기기 */
}

/* IE, Edge */
.section-middle .img-wrapper {
  -ms-overflow-style: none; /* IE/Edge 스크롤바 숨기기 */
}

/* Firefox */
.section-middle .img-wrapper {
  scrollbar-width: none; /* Firefox 스크롤바 숨기기 */
}
```

**왜 이걸 썼을까?**

- ✅ **깔끔한 UI**: 스크롤바 없이 깔끔한 디자인
- ✅ **모바일 친화적**: 터치 스크롤에 최적화
- ✅ **크로스 브라우저**: 모든 브라우저에서 스크롤바 숨김

**발표용 한 줄 요약:**

> "-webkit-scrollbar는 웹킷 기반 브라우저에서 스크롤바를 숨기는 CSS 속성으로, 깔끔한 UI를 위해 사용했습니다"

#### 🤔 스크롤 스냅 효과란?

> "스크롤을 멈추면 자동으로 가장 가까운 요소에 딱 맞춰서 정렬되는 효과"

**발표용 한 줄 요약:**

> "스크롤 스냅은 사용자가 스크롤을 멈추면 자동으로 가장 가까운 이미지에 딱 맞춰서 정렬해주는 CSS 기능입니다"

**왜 이걸 썼을까?**

- ✅ **사용자 경험 향상**: 이미지가 중간에 걸쳐서 보이지 않음
- ✅ **직관적인 네비게이션**: 한 번에 하나의 이미지만 깔끔하게 보임
- ✅ **모바일 친화적**: 터치 스크롤에 최적화됨

#### 🎉 발표용 마무리

```
"모바일 사용자가 손가락으로 좌우 스크롤하면
이미지들이 부드럽게 슬라이드되는 효과를
CSS overflow와 scroll-snap 속성으로 구현했습니다!"
```
