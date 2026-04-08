---
title: "React에서 KRDS 적용 가이드"
source_url: "https://www.krds.go.kr/html/site/outline/outline_03.html"
---

# React에서 KRDS 적용 가이드

React 프로젝트에 KRDS(Korea Government Design System) 컴포넌트와 디자인 토큰을 적용하는 실전 가이드이다.

---

## 1. 설치

### NPM 설치

```bash
npm install krds-react
```

### CSS 및 컴포넌트 Import

```jsx
// CSS import (프로젝트 엔트리 파일에서 1회)
import 'krds-react/dist/index.css';

// 컴포넌트 사용
import { Button } from 'krds-react';
```

### 폰트 설정

표준형 스타일은 Pretendard GOV 서체를 사용한다. `public/index.html` 또는 레이아웃 컴포넌트에 CDN을 추가한다.

```html
<link
  rel="stylesheet"
  as="style"
  crossorigin
  href="https://cdn.jsdelivr.net/gh/orioncactus/pretendard@v1.3.9/dist/web/static/pretendard-gov-dynamic-subset.min.css"
/>
```

---

## 2. 제공 컴포넌트 목록

`krds-react`는 다음 컴포넌트를 제공한다. (React Storybook 기준)

| 범주 | 컴포넌트 |
|---|---|
| **아이덴티티** | `Masthead`, `Header`, `Footer` |
| **탐색** | `SkipLink`, `MainMenu`, `Breadcrumb`, `SideNavigation`, `InPageNavigation`, `Pagination` |
| **레이아웃/표현** | `Accordion`, `StructuredList`, `CriticalAlert`, `Calendar`, `Disclosure`, `Modal`, `Badge`, `Image`, `Carousel`, `Tab`, `Table`, `TextList` |
| **액션** | `Link`, `Button` |
| **선택** | `Radio`, `Checkbox`, `Select`, `Tag`, `ToggleSwitch` |
| **피드백** | `StepIndicator`, `Spinner` |
| **도움** | `HelpPanel`, `TutorialPanel`, `ContextualHelp`, `CoachMark`, `Tooltip` |
| **입력** | `DateInput`, `TextInput`, `Textarea`, `FileUpload` |
| **설정** | `LanguageSwitcher`, `Resize` |

> **참고**: 컴포넌트의 상세 Props와 사용 예제는 [React Storybook](https://www.krds.go.kr/storybook/react/)에서 확인할 수 있다.

---

## 3. 디자인 토큰 적용

### CSS Variable 기반 토큰 사용

`krds-react/dist/index.css`를 import하면 KRDS 디자인 토큰이 CSS Custom Properties로 자동 등록된다.

```jsx
// 인라인 스타일에서 토큰 참조
function CustomCard({ children }) {
  return (
    <div style={{
      backgroundColor: 'var(--krds-color-bg-primary)',
      color: 'var(--krds-color-text-primary)',
      borderRadius: 'var(--krds-radius-lg)',
      padding: 'var(--krds-spacing-6)',
      border: '1px solid var(--krds-color-border-default)'
    }}>
      {children}
    </div>
  );
}
```

### styled-components / Emotion에서 토큰 사용

```jsx
import styled from 'styled-components';

const StyledSection = styled.section`
  background-color: var(--krds-color-bg-secondary);
  padding: var(--krds-spacing-8);
  border-radius: var(--krds-radius-lg);

  h2 {
    color: var(--krds-color-text-primary);
    font-size: var(--krds-font-size-heading-medium);
    font-weight: 700;
    line-height: 150%;
  }

  p {
    color: var(--krds-color-text-secondary);
    font-size: var(--krds-font-size-body-medium);
    line-height: 150%;
  }
`;
```

### Tailwind CSS와 함께 사용

```js
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      colors: {
        'krds-primary': 'var(--krds-color-primary-50)',
        'krds-bg': 'var(--krds-color-bg-primary)',
        'krds-text': 'var(--krds-color-text-primary)',
        'krds-border': 'var(--krds-color-border-default)',
      },
      borderRadius: {
        'krds-sm': 'var(--krds-radius-sm)',
        'krds-md': 'var(--krds-radius-md)',
        'krds-lg': 'var(--krds-radius-lg)',
      },
    },
  },
};
```

---

## 4. 컴포넌트 래핑 패턴

### 기본 래핑 예시: Button

```jsx
import { Button } from 'krds-react';

// 프로젝트 공통 버튼으로 래핑
export function KrdsButton({ children, variant = 'primary', size = 'medium', ...props }) {
  return (
    <Button
      variant={variant}
      size={size}
      {...props}
    >
      {children}
    </Button>
  );
}
```

### 페이지 레이아웃 래핑

```jsx
import { Header, Footer, Masthead, SkipLink, Breadcrumb } from 'krds-react';

export function PageLayout({ children, breadcrumbItems }) {
  return (
    <>
      <SkipLink href="#main-content">본문 바로가기</SkipLink>
      <Masthead />
      <Header
        logo={{ src: '/logo.svg', alt: '서비스명' }}
        // ... header props
      />
      {breadcrumbItems && <Breadcrumb items={breadcrumbItems} />}
      <main id="main-content">
        {children}
      </main>
      <Footer
        // ... footer props
      />
    </>
  );
}
```

### 폼 컴포넌트 래핑 (React Hook Form 연동)

```jsx
import { TextInput } from 'krds-react';
import { useFormContext } from 'react-hook-form';

export function KrdsFormInput({ name, label, rules, ...props }) {
  const { register, formState: { errors } } = useFormContext();
  const error = errors[name];

  return (
    <TextInput
      label={label}
      {...register(name, rules)}
      error={!!error}
      errorMessage={error?.message}
      {...props}
    />
  );
}
```

---

## 5. 디스플레이 모드 (선명한 화면 모드) 적용

```jsx
import { useState, useEffect } from 'react';

const MODES = ['default', 'dark', 'high-contrast'];

export function useKrdsMode() {
  const [mode, setMode] = useState('default');

  useEffect(() => {
    document.documentElement.setAttribute('data-krds-mode', mode);
  }, [mode]);

  const cycleMode = () => {
    setMode(prev => {
      const idx = MODES.indexOf(prev);
      return MODES[(idx + 1) % MODES.length];
    });
  };

  return { mode, setMode, cycleMode };
}
```

```jsx
// 사용 예시
function App() {
  const { mode, setMode } = useKrdsMode();

  return (
    <div>
      <select value={mode} onChange={(e) => setMode(e.target.value)}>
        <option value="default">기본</option>
        <option value="dark">다크</option>
        <option value="high-contrast">선명한 화면</option>
      </select>
      {/* ... */}
    </div>
  );
}
```

---

## 6. 디자인 토큰 커스터마이징

원본 토큰을 직접 수정하지 않고, 프로젝트 CSS에서 CSS Variable을 재선언한다.

```css
/* src/styles/krds-overrides.css */
:root {
  /* 기관 고유 Primary 색상으로 변경 */
  --krds-color-light-primary-50: #1a5ccc;

  /* 확장형 스타일: 기관 고유 래디어스 */
  --krds-radius-md: 8px;
  --krds-radius-lg: 12px;
}
```

```jsx
// main.jsx 또는 App.jsx에서 순서 중요
import 'krds-react/dist/index.css';       // KRDS 기본 토큰
import './styles/krds-overrides.css';      // 커스텀 오버라이드
```

---

## 7. 참고 링크

| 리소스 | URL |
|---|---|
| KRDS 공식 사이트 | https://www.krds.go.kr |
| React Storybook | https://www.krds.go.kr/storybook/react/ |
| NPM 패키지 | `krds-react` |
| HTML Component Kit GitHub | https://www.krds.go.kr (리소스 다운로드) |
| 디자인 토큰 목록 | KRDS 사이트 > 디자인 스타일 > 디자인 토큰 |
