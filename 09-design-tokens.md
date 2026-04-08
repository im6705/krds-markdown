---
title: "디자인 토큰 (Design Tokens)"
source_pages: "205-216"
---

# 디자인 토큰 (Design Tokens)

디자인 시스템의 스타일 가이드 요소인 색상, 글자, 간격의 스타일 속성을 변수로 정의하여 코드화한 것이다.

---

## 1. 디자인 토큰이란?

디자인 토큰은 디자인 시스템에서 반복적으로 사용되는 디자인 속성(색상, 글자, 간격, 그림자 등)을 효율적으로 관리하기 위한 추상화된 값을 변수로 정의한 코드이다.

### 토큰을 사용하는 이유

| 이유 | 설명 |
|---|---|
| **일관성 유지** | 핵심 디자인 요소를 코드화하여 관리, 기본적인 일관성 유지 |
| **협업 강화** | 디자인 속성을 공통 언어로 변환, 디자이너와 개발자가 같은 방식으로 이해/적용 |
| **접근성 및 표준화 지원** | 명도 대비 관련 요소를 표준화한 토큰으로 접근성 기준 쉽게 준수 |

### 기관별 적용

- **표준형 스타일 기관**: 디자인 토큰 요소를 그대로 사용
- **확장형 스타일 기관**: 자유도 있는 디자인 적용 가능하지만, KRDS 스타일 가이드와 적용된 토큰을 기반으로 관리

---

## 2. KRDS 디자인 토큰 흐름

```
디자인 툴 → JSON으로 추출 → CSS 변수로 변환 → HTML에 적용
```

- 디자인 툴 또는 CSS에서 토큰 값을 수정하면 HTML에 바로 반영 가능

### 토큰 변환 파이프라인 상세

```
[Figma Variables]
    │
    ▼ (Figma Tokens Plugin / REST API로 추출)
[tokens.json]
    │
    ▼ (Style Dictionary / Token Transformer 등 빌드 도구)
[CSS Custom Properties]  ←→  [SCSS Variables]  ←→  [JS/TS Constants]
    │
    ▼ (HTML/CSS에서 var() 함수로 참조)
[최종 UI 렌더링]
```

### JSON 토큰 구조 예시

```json
{
  "color": {
    "primary": {
      "50": { "value": "hsl(215, 85%, 55%)", "type": "color" },
      "60": { "value": "hsl(215, 85%, 45%)", "type": "color" },
      "70": { "value": "hsl(215, 85%, 35%)", "type": "color" }
    },
    "gray": {
      "0": { "value": "hsl(210, 20%, 100%)", "type": "color" },
      "5": { "value": "hsl(210, 20%, 97%)", "type": "color" },
      "10": { "value": "hsl(210, 20%, 93%)", "type": "color" },
      "90": { "value": "hsl(210, 20%, 15%)", "type": "color" },
      "95": { "value": "hsl(210, 20%, 8%)", "type": "color" },
      "100": { "value": "hsl(210, 20%, 3%)", "type": "color" }
    }
  },
  "spacing": {
    "gap-1": { "value": "4px", "type": "spacing" },
    "gap-2": { "value": "8px", "type": "spacing" },
    "gap-3": { "value": "12px", "type": "spacing" },
    "gap-4": { "value": "16px", "type": "spacing" },
    "gap-5": { "value": "20px", "type": "spacing" },
    "gap-6": { "value": "24px", "type": "spacing" },
    "gap-8": { "value": "32px", "type": "spacing" }
  },
  "radius": {
    "sm": { "value": "4px", "type": "borderRadius" },
    "md": { "value": "8px", "type": "borderRadius" },
    "lg": { "value": "12px", "type": "borderRadius" },
    "full": { "value": "9999px", "type": "borderRadius" }
  }
}
```

---

## 3. 디자인 토큰 3-Level 체계

| Level | 이름 | 설명 | 직접 사용 | 예시 |
|---|---|---|---|---|
| **Level 1** | Primitive Token | 기본적인 디자인 속성 정의 (color, typo, space, radius). 다른 토큰의 참고 요소 | **직접 사용하지 않음** | `primary-50`, `gray-5`, `number-4` |
| **Level 2** | Semantic Token | 특정 맥락에서 의미를 가지는 속성. Primitive tokens를 참조하여 정의. 상태/역할 표현 | **사용** (디자인 툴에서 정의) | `color-icon-primary`, `color-border-gray-light` |
| **Level 3** | Component Token | 특정 UI 컴포넌트에 직접 적용되는 구체적 디자인 속성. Semantic tokens를 참조 | **사용** (코드에서 정의) | `--namespace-component--theme-type-size-modifier` |

### Primitive Token 포함 요소
- 색상 팔레트: primary, secondary, gray의 0-100
- 타이포그래피: 서체, 굵기, 사이즈
- 간격: 기본 수치 4, 8, 16, 20 등

### Primitive Token CSS 변수 정의 예시

```css
:root {
  /* 색상 - Primary */
  --krds-color-primary-5: hsl(215, 85%, 97%);
  --krds-color-primary-10: hsl(215, 85%, 93%);
  --krds-color-primary-20: hsl(215, 85%, 83%);
  --krds-color-primary-30: hsl(215, 85%, 73%);
  --krds-color-primary-40: hsl(215, 85%, 63%);
  --krds-color-primary-50: hsl(215, 85%, 55%);
  --krds-color-primary-60: hsl(215, 85%, 45%);
  --krds-color-primary-70: hsl(215, 85%, 35%);
  --krds-color-primary-80: hsl(215, 85%, 25%);
  --krds-color-primary-90: hsl(215, 85%, 15%);
  --krds-color-primary-95: hsl(215, 85%, 8%);

  /* 색상 - Gray (Blue Gray) */
  --krds-color-gray-0: hsl(210, 20%, 100%);
  --krds-color-gray-5: hsl(210, 20%, 97%);
  --krds-color-gray-10: hsl(210, 20%, 93%);
  --krds-color-gray-20: hsl(210, 20%, 83%);
  --krds-color-gray-30: hsl(210, 20%, 73%);
  --krds-color-gray-40: hsl(210, 20%, 63%);
  --krds-color-gray-50: hsl(210, 20%, 53%);
  --krds-color-gray-60: hsl(210, 20%, 43%);
  --krds-color-gray-70: hsl(210, 20%, 33%);
  --krds-color-gray-80: hsl(210, 20%, 23%);
  --krds-color-gray-90: hsl(210, 20%, 15%);
  --krds-color-gray-95: hsl(210, 20%, 8%);
  --krds-color-gray-100: hsl(210, 20%, 3%);

  /* 간격 (Spacing) */
  --krds-spacing-1: 4px;
  --krds-spacing-2: 8px;
  --krds-spacing-3: 12px;
  --krds-spacing-4: 16px;
  --krds-spacing-5: 20px;
  --krds-spacing-6: 24px;
  --krds-spacing-8: 32px;
  --krds-spacing-10: 40px;
  --krds-spacing-12: 48px;
  --krds-spacing-16: 64px;

  /* 래디어스 (Radius) */
  --krds-radius-sm: 4px;
  --krds-radius-md: 8px;
  --krds-radius-lg: 12px;
  --krds-radius-xl: 16px;
  --krds-radius-full: 9999px;

  /* 타이포그래피 */
  --krds-font-family: 'Pretendard GOV', 'Pretendard', -apple-system, BlinkMacSystemFont, system-ui, sans-serif;
  --krds-font-weight-regular: 400;
  --krds-font-weight-medium: 500;
  --krds-font-weight-semibold: 600;
  --krds-font-weight-bold: 700;
}
```

### Semantic Token 포함 요소
- 배경에 사용되는 색상, 아이콘에 사용되는 색상
- 카드 내부 패딩값, 작은 사이즈 컴포넌트의 래디어스 등

### Semantic Token CSS 변수 정의 예시

```css
:root {
  /* 배경 색상 */
  --krds-color-bg-primary: var(--krds-color-gray-0);
  --krds-color-bg-secondary: var(--krds-color-gray-5);
  --krds-color-bg-tertiary: var(--krds-color-gray-10);
  --krds-color-bg-inverse: var(--krds-color-gray-90);

  /* 텍스트 색상 */
  --krds-color-text-primary: var(--krds-color-gray-90);
  --krds-color-text-secondary: var(--krds-color-gray-60);
  --krds-color-text-tertiary: var(--krds-color-gray-40);
  --krds-color-text-inverse: var(--krds-color-gray-0);
  --krds-color-text-link: var(--krds-color-primary-50);

  /* 아이콘 색상 */
  --krds-color-icon-primary: var(--krds-color-gray-90);
  --krds-color-icon-secondary: var(--krds-color-gray-50);
  --krds-color-icon-action: var(--krds-color-primary-50);

  /* 보더 색상 */
  --krds-color-border-primary: var(--krds-color-gray-20);
  --krds-color-border-strong: var(--krds-color-gray-50);
  --krds-color-border-focus: var(--krds-color-primary-50);
  --krds-color-border-error: var(--krds-color-danger-50);

  /* 간격 시멘틱 토큰 */
  --krds-gap-1: var(--krds-spacing-1);   /* 4px */
  --krds-gap-2: var(--krds-spacing-2);   /* 8px */
  --krds-gap-3: var(--krds-spacing-3);   /* 12px */
  --krds-gap-4: var(--krds-spacing-4);   /* 16px */
  --krds-gap-6: var(--krds-spacing-6);   /* 24px */
  --krds-gap-8: var(--krds-spacing-8);   /* 32px */

  /* 패딩 시멘틱 토큰 */
  --krds-padding-card: var(--krds-spacing-6);       /* 24px */
  --krds-padding-section: var(--krds-spacing-8);    /* 32px */
  --krds-padding-input-x: var(--krds-spacing-4);    /* 16px */
  --krds-padding-input-y: var(--krds-spacing-3);    /* 12px */
  --krds-padding-button-x: var(--krds-spacing-6);   /* 24px */
  --krds-padding-button-y: var(--krds-spacing-3);   /* 12px */
}
```

### Component Token 유의 사항
- KRDS에서는 **디자인 툴에서 시멘틱 토큰까지만 정의**, 컴포넌트 토큰은 **코드에서 정의**
- 디자인 툴에서 선언된 input, button 등의 토큰은 시멘틱 토큰으로 간주
- 레이아웃형 컴포넌트(헤더, 푸터) 및 외부 플러그인(캐러셀)은 컴포넌트 토큰을 따로 정의하지 않고 시멘틱 토큰 사용

### Component Token CSS 변수 정의 예시

```css
/* 버튼 컴포넌트 토큰 */
.krds-button {
  --krds-button--color-bg: var(--krds-color-primary-50);
  --krds-button--color-text: var(--krds-color-gray-0);
  --krds-button--color-border: transparent;
  --krds-button--padding-x: var(--krds-padding-button-x);
  --krds-button--padding-y: var(--krds-padding-button-y);
  --krds-button--radius: var(--krds-radius-md);
  --krds-button--font-weight: var(--krds-font-weight-semibold);

  background-color: var(--krds-button--color-bg);
  color: var(--krds-button--color-text);
  border: 1px solid var(--krds-button--color-border);
  padding: var(--krds-button--padding-y) var(--krds-button--padding-x);
  border-radius: var(--krds-button--radius);
  font-weight: var(--krds-button--font-weight);
  font-family: var(--krds-font-family);
}

/* 버튼 상태별 토큰 */
.krds-button:hover {
  --krds-button--color-bg: var(--krds-color-primary-60);
}
.krds-button:active {
  --krds-button--color-bg: var(--krds-color-primary-70);
}
.krds-button:disabled {
  --krds-button--color-bg: var(--krds-color-gray-20);
  --krds-button--color-text: var(--krds-color-gray-40);
}

/* 인풋 컴포넌트 토큰 */
.krds-input {
  --krds-input--color-bg: var(--krds-color-bg-primary);
  --krds-input--color-text: var(--krds-color-text-primary);
  --krds-input--color-border: var(--krds-color-border-primary);
  --krds-input--color-border-focus: var(--krds-color-border-focus);
  --krds-input--color-placeholder: var(--krds-color-text-tertiary);
  --krds-input--padding-x: var(--krds-padding-input-x);
  --krds-input--padding-y: var(--krds-padding-input-y);
  --krds-input--radius: var(--krds-radius-md);

  background-color: var(--krds-input--color-bg);
  color: var(--krds-input--color-text);
  border: 1px solid var(--krds-input--color-border);
  padding: var(--krds-input--padding-y) var(--krds-input--padding-x);
  border-radius: var(--krds-input--radius);
  font-family: var(--krds-font-family);
}

.krds-input:focus {
  --krds-input--color-border: var(--krds-input--color-border-focus);
  outline: 2px solid var(--krds-input--color-border-focus);
  outline-offset: -2px;
}

/* 카드 컴포넌트 토큰 */
.krds-card {
  --krds-card--color-bg: var(--krds-color-bg-primary);
  --krds-card--color-border: var(--krds-color-border-primary);
  --krds-card--padding: var(--krds-padding-card);
  --krds-card--radius: var(--krds-radius-lg);
  --krds-card--shadow: 0 1px 3px rgba(0, 0, 0, 0.1), 0 1px 2px rgba(0, 0, 0, 0.06);

  background-color: var(--krds-card--color-bg);
  border: 1px solid var(--krds-card--color-border);
  padding: var(--krds-card--padding);
  border-radius: var(--krds-card--radius);
  box-shadow: var(--krds-card--shadow);
}
```

---

## 4. 디자인 토큰 표기법

### 시멘틱 토큰 표기

큰 개념에서 세부 요소로 좁혀가는 방식으로 구조화한다.

구조: `네임스페이스-테마-카테고리-속성-변형상태`

예시:
```
--krds-color-bg-primary        → 네임스페이스(krds) - 카테고리(color) - 속성(bg) - 변형(primary)
--krds-color-text-secondary    → 네임스페이스(krds) - 카테고리(color) - 속성(text) - 변형(secondary)
--krds-color-border-focus      → 네임스페이스(krds) - 카테고리(color) - 속성(border) - 변형(focus)
--krds-spacing-4               → 네임스페이스(krds) - 카테고리(spacing) - 값(4)
```

### 컴포넌트 토큰 표기

```
--namespace-component--theme-type-size-modifier
```

#### 규칙
- 컴포넌트 명을 다른 속성보다 **먼저 표기**
- 컴포넌트명 뒤에 **대시 2개 (--)** 를 붙여 다른 속성과 구분
- 새로운 속성 추가 시 **CSS에서 사용하는 속성명** 차용
- padding/margin 등 좌/우 또는 상/하 값이 동일한 경우:
  - `-x` (좌우 동일)
  - `-y` (상하 동일)
  - 예: `--namespace-component--type-padding-x`

#### 컴포넌트 토큰 표기 예시 상세

```css
/* 버튼 컴포넌트 */
--krds-button--color-bg              /* 배경 색상 */
--krds-button--color-text            /* 텍스트 색상 */
--krds-button--padding-x             /* 좌우 패딩 */
--krds-button--padding-y             /* 상하 패딩 */
--krds-button--radius                /* 모서리 래디어스 */
--krds-button--sm-padding-x          /* 작은 버튼 좌우 패딩 */
--krds-button--lg-padding-x          /* 큰 버튼 좌우 패딩 */

/* 인풋 컴포넌트 */
--krds-input--color-bg               /* 배경 색상 */
--krds-input--color-border           /* 보더 색상 */
--krds-input--color-border-focus     /* 포커스 보더 색상 */
--krds-input--color-border-error     /* 에러 보더 색상 */
--krds-input--padding-x              /* 좌우 패딩 */
--krds-input--padding-y              /* 상하 패딩 */

/* 카드 컴포넌트 */
--krds-card--color-bg                /* 배경 색상 */
--krds-card--padding                 /* 내부 패딩 */
--krds-card--shadow                  /* 그림자 */
--krds-card--radius                  /* 모서리 래디어스 */
```

#### SCSS 배열 → @each 문으로 CSS variable 출력

```scss
// SCSS에서 컴포넌트 토큰 관리 예시
$krds-button-tokens: (
  'color-bg': var(--krds-color-primary-50),
  'color-text': var(--krds-color-gray-0),
  'color-border': transparent,
  'padding-x': var(--krds-padding-button-x),
  'padding-y': var(--krds-padding-button-y),
  'radius': var(--krds-radius-md),
  'font-weight': var(--krds-font-weight-semibold),
);

.krds-button {
  @each $property, $value in $krds-button-tokens {
    --krds-button--#{$property}: #{$value};
  }

  background-color: var(--krds-button--color-bg);
  color: var(--krds-button--color-text);
  border: 1px solid var(--krds-button--color-border);
  padding: var(--krds-button--padding-y) var(--krds-button--padding-x);
  border-radius: var(--krds-button--radius);
  font-weight: var(--krds-button--font-weight);
}
```

---

## 5. HTML에서 디자인 토큰 적용

### 기본 HTML 구조에서 토큰 사용

```html
<!DOCTYPE html>
<html lang="ko">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>KRDS 디자인 토큰 적용 예시</title>
  <!-- KRDS 토큰 CSS 로드 -->
  <link rel="stylesheet" href="/css/krds-tokens.css">
  <link rel="stylesheet" href="/css/krds-components.css">
</head>
<body>
  <!-- 카드 컴포넌트에 토큰 적용 -->
  <div class="krds-card">
    <h2 style="color: var(--krds-color-text-primary);">제목</h2>
    <p style="color: var(--krds-color-text-secondary); margin-top: var(--krds-gap-2);">
      본문 텍스트
    </p>
    <button class="krds-button" style="margin-top: var(--krds-gap-4);">
      액션 버튼
    </button>
  </div>
</body>
</html>
```

### JavaScript에서 토큰 값 프로그래밍적 접근

```javascript
// CSS 변수 값 읽기
function getTokenValue(tokenName) {
  return getComputedStyle(document.documentElement)
    .getPropertyValue(tokenName)
    .trim();
}

// 사용 예시
const primaryColor = getTokenValue('--krds-color-primary-50');
const spacing = getTokenValue('--krds-gap-4');
console.log(primaryColor); // "hsl(215, 85%, 55%)"
console.log(spacing);      // "16px"

// CSS 변수 값 동적 변경
function setTokenValue(tokenName, value) {
  document.documentElement.style.setProperty(tokenName, value);
}

// 런타임에 토큰 변경 예시
setTokenValue('--krds-color-primary-50', 'hsl(220, 80%, 50%)');
```

### JavaScript에서 토큰 관리 유틸리티 클래스

```javascript
/**
 * KRDS 디자인 토큰 관리 유틸리티
 */
class KRDSTokenManager {
  constructor() {
    this.root = document.documentElement;
    this.tokens = {};
    this._loadTokens();
  }

  /** 현재 적용된 모든 KRDS 토큰을 로드 */
  _loadTokens() {
    const styles = getComputedStyle(this.root);
    const sheets = document.styleSheets;

    for (const sheet of sheets) {
      try {
        for (const rule of sheet.cssRules) {
          if (rule.selectorText === ':root') {
            for (const prop of rule.style) {
              if (prop.startsWith('--krds-')) {
                this.tokens[prop] = styles.getPropertyValue(prop).trim();
              }
            }
          }
        }
      } catch (e) {
        // cross-origin stylesheet 무시
      }
    }
  }

  /** 토큰 값 조회 */
  get(tokenName) {
    const name = tokenName.startsWith('--') ? tokenName : `--${tokenName}`;
    return getComputedStyle(this.root).getPropertyValue(name).trim();
  }

  /** 토큰 값 설정 */
  set(tokenName, value) {
    const name = tokenName.startsWith('--') ? tokenName : `--${tokenName}`;
    this.root.style.setProperty(name, value);
    this.tokens[name] = value;
  }

  /** 여러 토큰 일괄 설정 (테마 전환 등) */
  setMultiple(tokenMap) {
    Object.entries(tokenMap).forEach(([name, value]) => {
      this.set(name, value);
    });
  }

  /** 특정 카테고리의 토큰 목록 조회 */
  getByCategory(category) {
    return Object.entries(this.tokens)
      .filter(([key]) => key.includes(`-${category}-`))
      .reduce((acc, [key, value]) => ({ ...acc, [key]: value }), {});
  }

  /** 토큰 초기화 (인라인 스타일 제거, CSS 파일 기본값 복원) */
  reset(tokenName) {
    const name = tokenName.startsWith('--') ? tokenName : `--${tokenName}`;
    this.root.style.removeProperty(name);
  }
}

// 사용 예시
const tokenManager = new KRDSTokenManager();
console.log(tokenManager.get('--krds-color-primary-50'));
console.log(tokenManager.getByCategory('spacing'));
```

---

## 6. 모드 전환

### 선명한 화면 모드 (mode)

- 기본 모드와 선명한 화면 모드 간 토큰 적용이 되어 있음
- 선명한 화면 모드의 **고대비 명도 기준** 준수
- 전환되는 요소: **보더 값**, **색상**

#### 모드별 시멘틱 토큰 매핑

```css
/* 기본 모드 (default) */
:root,
[data-krds-mode="default"] {
  --krds-color-bg-primary: var(--krds-color-gray-0);
  --krds-color-bg-secondary: var(--krds-color-gray-5);
  --krds-color-bg-tertiary: var(--krds-color-gray-10);
  --krds-color-text-primary: var(--krds-color-gray-90);
  --krds-color-text-secondary: var(--krds-color-gray-60);
  --krds-color-border-primary: var(--krds-color-gray-20);
  --krds-color-border-strong: var(--krds-color-gray-50);
}

/* 선명한 화면 모드 (high-contrast) */
[data-krds-mode="high-contrast"] {
  --krds-color-bg-primary: var(--krds-color-gray-100);
  --krds-color-bg-secondary: var(--krds-color-gray-95);
  --krds-color-bg-tertiary: var(--krds-color-gray-90);
  --krds-color-text-primary: var(--krds-color-gray-0);
  --krds-color-text-secondary: var(--krds-color-gray-20);
  --krds-color-border-primary: var(--krds-color-gray-70);
  --krds-color-border-strong: var(--krds-color-gray-50);

  /* 보더 두께 강화 */
  --krds-border-width-default: 2px;
  --krds-border-width-focus: 3px;
}

/* OS 설정 연동 (prefers-contrast) */
@media (prefers-contrast: high) {
  :root:not([data-krds-mode]) {
    --krds-color-bg-primary: var(--krds-color-gray-100);
    --krds-color-bg-secondary: var(--krds-color-gray-95);
    --krds-color-text-primary: var(--krds-color-gray-0);
    --krds-color-text-secondary: var(--krds-color-gray-20);
  }
}

/* OS 다크 모드 감지 */
@media (prefers-color-scheme: dark) {
  :root:not([data-krds-mode]) {
    --krds-color-bg-primary: var(--krds-color-gray-95);
    --krds-color-bg-secondary: var(--krds-color-gray-90);
    --krds-color-bg-tertiary: var(--krds-color-gray-80);
    --krds-color-text-primary: var(--krds-color-gray-5);
    --krds-color-text-secondary: var(--krds-color-gray-30);
    --krds-color-border-primary: var(--krds-color-gray-70);
  }
}
```

#### JavaScript 모드 전환 구현

```javascript
/**
 * KRDS 화면 모드 전환 관리
 */
class KRDSModeManager {
  static MODES = ['default', 'high-contrast'];
  static STORAGE_KEY = 'krds-display-mode';

  constructor() {
    this.currentMode = this._loadMode();
    this._applyMode(this.currentMode);
    this._watchOSPreference();
  }

  /** 저장된 모드 로드 또는 OS 설정 확인 */
  _loadMode() {
    const saved = localStorage.getItem(KRDSModeManager.STORAGE_KEY);
    if (saved && KRDSModeManager.MODES.includes(saved)) return saved;

    // OS 설정 감지
    if (window.matchMedia('(prefers-contrast: high)').matches) {
      return 'high-contrast';
    }
    return 'default';
  }

  /** DOM에 모드 적용 */
  _applyMode(mode) {
    document.documentElement.setAttribute('data-krds-mode', mode);
    this.currentMode = mode;
  }

  /** OS 설정 변경 감지 */
  _watchOSPreference() {
    window.matchMedia('(prefers-contrast: high)')
      .addEventListener('change', (e) => {
        if (!localStorage.getItem(KRDSModeManager.STORAGE_KEY)) {
          this._applyMode(e.matches ? 'high-contrast' : 'default');
        }
      });
  }

  /** 모드 변경 */
  setMode(mode) {
    if (!KRDSModeManager.MODES.includes(mode)) {
      throw new Error(`Invalid mode: ${mode}. Use: ${KRDSModeManager.MODES.join(', ')}`);
    }
    this._applyMode(mode);
    localStorage.setItem(KRDSModeManager.STORAGE_KEY, mode);
  }

  /** 모드 토글 */
  toggle() {
    const nextIndex = (KRDSModeManager.MODES.indexOf(this.currentMode) + 1) % KRDSModeManager.MODES.length;
    this.setMode(KRDSModeManager.MODES[nextIndex]);
  }

  /** 현재 모드 조회 */
  getMode() {
    return this.currentMode;
  }
}

// 사용 예시
const modeManager = new KRDSModeManager();

// 모드 전환 버튼 이벤트
document.querySelector('#mode-toggle').addEventListener('click', () => {
  modeManager.toggle();
});
```

### 반응형 모드 (responsive)

디바이스 사이즈에 맞게 변할 수 있는 모드. large와 small 사이즈 대응.

전환되는 요소:
1. **글자 사이즈**
2. **간격**
3. **카드 패딩값**

#### 반응형 토큰 CSS 구현

```css
:root {
  /* Large (기본) */
  --krds-font-size-heading-1: 32px;
  --krds-font-size-heading-2: 24px;
  --krds-font-size-heading-3: 20px;
  --krds-font-size-body: 16px;
  --krds-font-size-caption: 14px;
  --krds-padding-card: 24px;
  --krds-gap-section: 32px;
}

/* Small (모바일) - 768px 이하 */
@media (max-width: 768px) {
  :root {
    --krds-font-size-heading-1: 24px;
    --krds-font-size-heading-2: 20px;
    --krds-font-size-heading-3: 18px;
    --krds-font-size-body: 15px;
    --krds-font-size-caption: 13px;
    --krds-padding-card: 16px;
    --krds-gap-section: 24px;
  }
}
```

### Figma에서 모드 전환

Figma에서 제공하는 라이브러리(local variable)를 통해 모드 전환이 가능하다.

---

## 7. 빌드 파이프라인 통합

### Style Dictionary를 활용한 토큰 빌드

[Style Dictionary](https://amzn.github.io/style-dictionary/)를 사용하여 JSON 토큰을 다양한 플랫폼용 코드로 변환할 수 있다.

#### 프로젝트 구조

```
krds-tokens/
├── tokens/
│   ├── color/
│   │   ├── primitive.json      # Level 1: Primitive 색상
│   │   ├── semantic.json       # Level 2: Semantic 색상
│   │   └── high-contrast.json  # 선명한 화면 모드 색상
│   ├── spacing/
│   │   ├── primitive.json
│   │   └── semantic.json
│   ├── typography/
│   │   ├── primitive.json
│   │   └── semantic.json
│   └── radius/
│       └── primitive.json
├── config.js                   # Style Dictionary 설정
├── package.json
└── dist/                       # 빌드 출력
    ├── css/
    │   ├── krds-tokens.css
    │   └── krds-tokens-high-contrast.css
    ├── scss/
    │   ├── _krds-tokens.scss
    │   └── _krds-tokens-high-contrast.scss
    └── js/
        ├── krds-tokens.js
        └── krds-tokens.d.ts
```

#### Style Dictionary 설정 (config.js)

```javascript
const StyleDictionary = require('style-dictionary');

// 커스텀 트랜스폼: KRDS 네이밍 규칙 적용
StyleDictionary.registerTransform({
  name: 'name/krds',
  type: 'name',
  transformer: (token) => {
    return `krds-${token.path.join('-')}`;
  }
});

module.exports = {
  source: ['tokens/**/*.json'],
  platforms: {
    css: {
      transformGroup: 'css',
      transforms: ['name/krds', 'color/hsl-4', 'size/rem'],
      buildPath: 'dist/css/',
      files: [{
        destination: 'krds-tokens.css',
        format: 'css/variables',
        options: {
          selector: ':root',
          outputReferences: true
        }
      }]
    },
    scss: {
      transformGroup: 'scss',
      transforms: ['name/krds', 'color/hsl-4', 'size/rem'],
      buildPath: 'dist/scss/',
      files: [{
        destination: '_krds-tokens.scss',
        format: 'scss/variables',
        options: { outputReferences: true }
      }]
    },
    js: {
      transformGroup: 'js',
      transforms: ['name/krds'],
      buildPath: 'dist/js/',
      files: [
        {
          destination: 'krds-tokens.js',
          format: 'javascript/es6'
        },
        {
          destination: 'krds-tokens.d.ts',
          format: 'typescript/es6-declarations'
        }
      ]
    }
  }
};
```

#### package.json 빌드 스크립트

```json
{
  "name": "@krds/tokens",
  "version": "1.0.0",
  "description": "KRDS 디자인 토큰",
  "main": "dist/js/krds-tokens.js",
  "types": "dist/js/krds-tokens.d.ts",
  "files": ["dist/"],
  "scripts": {
    "build": "style-dictionary build --config config.js",
    "build:watch": "chokidar 'tokens/**/*.json' -c 'npm run build'",
    "clean": "rimraf dist",
    "prepublishOnly": "npm run clean && npm run build"
  },
  "devDependencies": {
    "style-dictionary": "^3.9.0",
    "chokidar-cli": "^3.0.0",
    "rimraf": "^5.0.0"
  }
}
```

### 토큰 버전 관리

#### 시맨틱 버저닝 (SemVer) 전략

| 변경 유형 | 버전 증가 | 예시 |
|---|---|---|
| **Primitive 토큰 값 미세 조정** | PATCH (1.0.x) | gray-50의 L값 53% → 52% |
| **새로운 Semantic 토큰 추가** | MINOR (1.x.0) | `--krds-color-bg-highlight` 추가 |
| **기존 토큰 이름 변경/삭제** | MAJOR (x.0.0) | `--krds-gap-4` → `--krds-space-4` 변경 |
| **모드 추가** | MINOR (1.x.0) | 다크 모드 토큰 세트 추가 |

#### 변경 기록 (CHANGELOG) 관리

```markdown
## [1.2.0] - 2025-01-15
### Added
- 다크 모드 시멘틱 토큰 세트 추가
- `--krds-color-bg-overlay` 토큰 추가

### Changed
- `--krds-color-primary-50` L값 55% → 53% 조정 (WCAG AA 대비 개선)

### Deprecated
- `--krds-gap-sm` → `--krds-gap-2` 로 마이그레이션 권장
```

### 멀티 마이크로프론트엔드 환경에서의 토큰 배포

#### 배포 아키텍처

```
@krds/tokens (npm 패키지)
    │
    ├── Shell App (메인 프레임)
    │   └── @krds/tokens@1.2.0  ──→ :root에 CSS 변수 주입
    │
    ├── Micro-Frontend A
    │   └── @krds/tokens@1.2.0  ──→ Shadow DOM 또는 scoped CSS
    │
    └── Micro-Frontend B
        └── @krds/tokens@1.2.0  ──→ Shadow DOM 또는 scoped CSS
```

#### Shell App에서 전역 토큰 주입

```javascript
// shell-app/src/main.js
import '@krds/tokens/dist/css/krds-tokens.css';

// 모드 관리는 Shell App이 담당
const modeManager = new KRDSModeManager();

// Micro-Frontend에 모드 변경 이벤트 전달
window.addEventListener('krds-mode-change', (e) => {
  modeManager.setMode(e.detail.mode);
});
```

#### Micro-Frontend에서 토큰 소비

```javascript
// micro-frontend-a/src/main.js

// 방법 1: Shell App이 주입한 CSS 변수 직접 참조 (권장)
// 컴포넌트 CSS에서 var(--krds-color-primary-50) 사용

// 방법 2: JS 상수로 import (빌드 시 토큰 값 고정)
import { KrdsColorPrimary50 } from '@krds/tokens/dist/js/krds-tokens';

// 방법 3: Shadow DOM 환경에서 토큰 주입
class KRDSMicroComponent extends HTMLElement {
  constructor() {
    super();
    this.attachShadow({ mode: 'open' });
    // Shadow DOM 내부에 토큰 CSS 주입
    const tokenSheet = new CSSStyleSheet();
    tokenSheet.replaceSync(document.querySelector('style#krds-tokens').textContent);
    this.shadowRoot.adoptedStyleSheets = [tokenSheet];
  }
}
```

#### 토큰 업데이트 워크플로

```
1. 디자이너가 Figma에서 토큰 수정
   │
2. Figma Tokens Plugin으로 JSON 추출 → Git 커밋
   │
3. CI/CD 파이프라인 실행
   │
   ├── a. Style Dictionary 빌드
   ├── b. 빌드 결과물 검증 (CSS 유효성, 토큰 개수)
   ├── c. 시각적 회귀 테스트 (Chromatic 등)
   └── d. npm 패키지 버전 업 & 배포
   │
4. 각 마이크로프론트엔드에서 @krds/tokens 버전 업데이트
   │
5. 릴리스 검증 및 모니터링
```

#### Webpack/Vite 통합 설정

```javascript
// vite.config.js
import { defineConfig } from 'vite';

export default defineConfig({
  css: {
    preprocessorOptions: {
      scss: {
        // KRDS 토큰 SCSS 변수를 모든 SCSS 파일에 자동 주입
        additionalData: `@use '@krds/tokens/dist/scss/krds-tokens' as *;`
      }
    }
  },
  resolve: {
    alias: {
      '@krds/tokens': '/node_modules/@krds/tokens'
    }
  }
});
```

```javascript
// webpack.config.js
const path = require('path');

module.exports = {
  module: {
    rules: [
      {
        test: /\.scss$/,
        use: [
          'style-loader',
          'css-loader',
          {
            loader: 'sass-loader',
            options: {
              sassOptions: {
                includePaths: [
                  path.resolve(__dirname, 'node_modules/@krds/tokens/dist/scss')
                ]
              },
              additionalData: `@use 'krds-tokens' as *;`
            }
          }
        ]
      }
    ]
  }
};
```

### 토큰 의존성 관리

| 의존성 시나리오 | 해결 방법 |
|---|---|
| **버전 불일치** | Shell App과 Micro-Frontend가 동일 major 버전 사용 강제 (peerDependencies) |
| **토큰 이름 변경** | MAJOR 버전에서만 허용, deprecated 경고 1 minor 버전 유지 |
| **새로운 토큰 추가** | MINOR 버전 업, 하위 호환 보장 |
| **런타임 토큰 충돌** | CSS 변수의 cascading 특성 활용, Shell App의 :root 선언이 기본값 역할 |

```json
{
  "name": "@my-org/micro-frontend-a",
  "peerDependencies": {
    "@krds/tokens": "^1.0.0"
  }
}
```
