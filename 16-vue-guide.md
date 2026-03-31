---
title: "Vue에서 KRDS 적용 가이드"
source_url: "https://www.krds.go.kr/html/site/outline/outline_03.html"
---

# Vue에서 KRDS 적용 가이드

Vue 프로젝트에 KRDS(Korea Government Design System) 컴포넌트와 디자인 토큰을 적용하는 실전 가이드이다.

---

## 1. 설치

### NPM 설치

```bash
npm install krds-vue
```

### CSS 및 컴포넌트 Import

```js
// CSS import (main.js 또는 main.ts에서 1회)
import 'krds-vue/dist/index.css';

// 컴포넌트 사용
import { Button } from 'krds-vue';
```

### 폰트 설정

표준형 스타일은 Pretendard GOV 서체를 사용한다. `index.html` 또는 레이아웃 컴포넌트에 CDN을 추가한다.

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

`krds-vue`는 다음 컴포넌트를 제공한다. (Vue Storybook 기준)

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

> **참고**: 컴포넌트의 상세 Props와 사용 예제는 [Vue Storybook](https://www.krds.go.kr/storybook/vue/)에서 확인할 수 있다.

---

## 3. 디자인 토큰 적용

### CSS Variable 기반 토큰 사용

`krds-vue/dist/index.css`를 import하면 KRDS 디자인 토큰이 CSS Custom Properties로 자동 등록된다.

```vue
<template>
  <div class="custom-card">
    <slot />
  </div>
</template>

<style scoped>
.custom-card {
  background-color: var(--krds-color-bg-primary);
  color: var(--krds-color-text-primary);
  border-radius: var(--krds-radius-lg);
  padding: var(--krds-spacing-6);
  border: 1px solid var(--krds-color-border-default);
}
</style>
```

### SCSS에서 토큰 사용

KRDS는 SCSS 사용을 권장하며, 컴포넌트 SCSS 원본도 함께 제공한다.

```vue
<style scoped lang="scss">
.section {
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
}
</style>
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

```vue
<!-- components/KrdsButton.vue -->
<template>
  <Button
    :variant="variant"
    :size="size"
    v-bind="$attrs"
  >
    <slot />
  </Button>
</template>

<script setup>
import { Button } from 'krds-vue';

defineProps({
  variant: {
    type: String,
    default: 'primary'
  },
  size: {
    type: String,
    default: 'medium'
  }
});
</script>
```

### 페이지 레이아웃 래핑

```vue
<!-- layouts/PageLayout.vue -->
<template>
  <SkipLink href="#main-content">본문 바로가기</SkipLink>
  <Masthead />
  <Header :logo="{ src: '/logo.svg', alt: '서비스명' }" />
  <Breadcrumb v-if="breadcrumbItems" :items="breadcrumbItems" />
  <main id="main-content">
    <slot />
  </main>
  <Footer />
</template>

<script setup>
import { Header, Footer, Masthead, SkipLink, Breadcrumb } from 'krds-vue';

defineProps({
  breadcrumbItems: {
    type: Array,
    default: null
  }
});
</script>
```

### 폼 컴포넌트 래핑 (VeeValidate 연동)

```vue
<!-- components/KrdsFormInput.vue -->
<template>
  <TextInput
    :label="label"
    :model-value="value"
    :error="!!errorMessage"
    :error-message="errorMessage"
    @update:model-value="handleChange"
    @blur="handleBlur"
    v-bind="$attrs"
  />
</template>

<script setup>
import { TextInput } from 'krds-vue';
import { useField } from 'vee-validate';

const props = defineProps({
  name: { type: String, required: true },
  label: { type: String, required: true },
  rules: { type: [String, Object], default: '' }
});

const { value, errorMessage, handleChange, handleBlur } = useField(
  props.name,
  props.rules
);
</script>
```

---

## 5. 디스플레이 모드 (선명한 화면 모드) 적용

### Composable 함수

```js
// composables/useKrdsMode.js
import { ref, watch } from 'vue';

const MODES = ['default', 'dark', 'high-contrast'];

export function useKrdsMode() {
  const mode = ref('default');

  watch(mode, (newMode) => {
    document.documentElement.setAttribute('data-krds-mode', newMode);
  }, { immediate: true });

  function cycleMode() {
    const idx = MODES.indexOf(mode.value);
    mode.value = MODES[(idx + 1) % MODES.length];
  }

  return { mode, cycleMode, MODES };
}
```

```vue
<!-- 사용 예시 -->
<template>
  <select v-model="mode">
    <option value="default">기본</option>
    <option value="dark">다크</option>
    <option value="high-contrast">선명한 화면</option>
  </select>
</template>

<script setup>
import { useKrdsMode } from '@/composables/useKrdsMode';
const { mode } = useKrdsMode();
</script>
```

### Pinia Store로 관리

```js
// stores/displayMode.js
import { defineStore } from 'pinia';

export const useDisplayModeStore = defineStore('displayMode', {
  state: () => ({
    mode: 'default'
  }),
  actions: {
    setMode(newMode) {
      this.mode = newMode;
      document.documentElement.setAttribute('data-krds-mode', newMode);
    },
    cycleMode() {
      const modes = ['default', 'dark', 'high-contrast'];
      const idx = modes.indexOf(this.mode);
      this.setMode(modes[(idx + 1) % modes.length]);
    }
  }
});
```

---

## 6. Vue Router와 함께 사용

### 레이아웃 시스템

```js
// router/index.js
import { createRouter, createWebHistory } from 'vue-router';
import PageLayout from '@/layouts/PageLayout.vue';

const routes = [
  {
    path: '/',
    component: PageLayout,
    children: [
      {
        path: '',
        name: 'Home',
        component: () => import('@/views/Home.vue'),
        meta: {
          breadcrumb: [{ label: '홈', to: '/' }]
        }
      },
      {
        path: 'service/:id',
        name: 'ServiceDetail',
        component: () => import('@/views/ServiceDetail.vue'),
        meta: {
          breadcrumb: [
            { label: '홈', to: '/' },
            { label: '서비스 목록', to: '/services' },
            { label: '서비스 상세' }
          ]
        }
      }
    ]
  }
];
```

---

## 7. 디자인 토큰 커스터마이징

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

```js
// main.js에서 import 순서 중요
import 'krds-vue/dist/index.css';          // KRDS 기본 토큰
import './styles/krds-overrides.css';       // 커스텀 오버라이드
import { createApp } from 'vue';
import App from './App.vue';

createApp(App).mount('#app');
```

---

## 8. 참고 링크

| 리소스 | URL |
|---|---|
| KRDS 공식 사이트 | https://www.krds.go.kr |
| Vue Storybook | https://www.krds.go.kr/storybook/vue/ |
| NPM 패키지 | `krds-vue` |
| HTML Component Kit GitHub | https://www.krds.go.kr (리소스 다운로드) |
| 디자인 토큰 목록 | KRDS 사이트 > 디자인 스타일 > 디자인 토큰 |
