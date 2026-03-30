---
title: "선명한 화면 모드 및 다크 모드 (Display Mode)"
source_pages: "192-204"
---

# 선명한 화면 모드 및 다크 모드 (Display Mode)

KRDS는 기본 모드(light), 선명한 화면 모드(high-contrast), 다크 모드(dark) 등 다양한 디스플레이 모드를 지원한다. 각 모드는 디자인 토큰 계층을 통해 구현되며, 사용자의 접근성 요구와 환경 설정에 맞게 전환할 수 있다.

---

## 1. 디스플레이 모드 개요

| 모드 | 설명 | 대상 사용자 |
|---|---|---|
| **기본 모드 (light)** | 밝은 배경 + 어두운 전경의 표준 색상 모드 | 일반 사용자 |
| **선명한 화면 모드 (high-contrast)** | 어두운 배경 + 밝은 전경, 명도 대비를 극대화한 모드 | 시각 기능 제약 사용자, 저조도 환경 |
| **다크 모드 (dark)** | 어두운 배경 + 밝은 전경, 눈의 피로를 줄인 모드 | 저조도 환경 선호 사용자 |

---

## 2. 핵심 스타일 요소

선명한 화면 모드를 표현하는 5가지 핵심 스타일 요소:

1. **색상**: 색상 팔레트에 정의된 규칙에 따라 선명한 화면 모드 팔레트로 일괄 전환
2. **대비**: 기본 모드보다 높은 대비 콘텐츠 제공
3. **상태**: 요소의 표현 방식 변경으로 더 잘 인지되도록
4. **계층 구조**: 어두운 배경에서 레이어가 점차 밝아지도록 표현
5. **시각 보조**: 대화형 요소에 부가적인 시각 단서 제공

---

## 3. 명도 대비 기준

| 콘텐츠 구분 | 기본 모드 | 선명한 화면 모드 | 다크 모드 |
|---|---|---|---|
| **본문 텍스트** | 7:1 이상 | **15:1 이상** | 7:1 이상 |
| **헤딩, 레이블 등의 텍스트** | 4.5:1 이상 | **7:1 이상** | 4.5:1 이상 |
| **아이콘, 시각 보조** | 3:1 이상 | **4.5:1 이상** | 3:1 이상 |

---

## 4. 모드별 디자인 토큰 정의

### 기본 모드 토큰 (Light / Default)

```css
:root,
[data-krds-mode="default"] {
  /* 배경 */
  --krds-color-bg-primary: var(--krds-color-gray-0);       /* white */
  --krds-color-bg-secondary: var(--krds-color-gray-5);
  --krds-color-bg-tertiary: var(--krds-color-gray-10);
  --krds-color-bg-inverse: var(--krds-color-gray-90);

  /* Elevation 배경 */
  --krds-color-surface-base: var(--krds-color-gray-0);
  --krds-color-surface-raised: var(--krds-color-gray-5);
  --krds-color-surface-overlay: var(--krds-color-gray-0);
  --krds-color-surface-sunken: var(--krds-color-gray-10);

  /* 텍스트 */
  --krds-color-text-primary: var(--krds-color-gray-90);
  --krds-color-text-secondary: var(--krds-color-gray-60);
  --krds-color-text-tertiary: var(--krds-color-gray-40);
  --krds-color-text-disabled: var(--krds-color-gray-30);
  --krds-color-text-inverse: var(--krds-color-gray-0);
  --krds-color-text-link: var(--krds-color-primary-50);
  --krds-color-text-link-visited: var(--krds-color-primary-70);

  /* 아이콘 */
  --krds-color-icon-primary: var(--krds-color-gray-90);
  --krds-color-icon-secondary: var(--krds-color-gray-50);
  --krds-color-icon-disabled: var(--krds-color-gray-30);
  --krds-color-icon-action: var(--krds-color-primary-50);

  /* 보더 */
  --krds-color-border-default: var(--krds-color-gray-20);
  --krds-color-border-strong: var(--krds-color-gray-50);
  --krds-color-border-focus: var(--krds-color-primary-50);
  --krds-color-border-disabled: var(--krds-color-gray-10);
  --krds-border-width-default: 1px;
  --krds-border-width-strong: 2px;
  --krds-border-width-focus: 2px;

  /* 시스템 색상 */
  --krds-color-danger-icon: var(--krds-color-danger-50);
  --krds-color-danger-text: var(--krds-color-danger-60);
  --krds-color-danger-bg: var(--krds-color-danger-5);
  --krds-color-danger-border: var(--krds-color-danger-10);
  --krds-color-success-icon: var(--krds-color-success-50);
  --krds-color-success-text: var(--krds-color-success-60);
  --krds-color-success-bg: var(--krds-color-success-5);
  --krds-color-warning-icon: var(--krds-color-warning-50);
  --krds-color-warning-text: var(--krds-color-warning-60);
  --krds-color-warning-bg: var(--krds-color-warning-5);
  --krds-color-info-icon: var(--krds-color-information-50);
  --krds-color-info-text: var(--krds-color-information-60);
  --krds-color-info-bg: var(--krds-color-information-5);

  /* 그림자 */
  --krds-shadow-sm: 0 1px 2px rgba(0, 0, 0, 0.05);
  --krds-shadow-md: 0 2px 8px rgba(0, 0, 0, 0.1);
  --krds-shadow-lg: 0 4px 16px rgba(0, 0, 0, 0.12);
}
```

### 선명한 화면 모드 토큰 (High Contrast)

```css
[data-krds-mode="high-contrast"] {
  /* 배경 - 어두운 배경에서 레이어가 점차 밝아짐 */
  --krds-color-bg-primary: var(--krds-color-gray-100);
  --krds-color-bg-secondary: var(--krds-color-gray-95);
  --krds-color-bg-tertiary: var(--krds-color-gray-90);
  --krds-color-bg-inverse: var(--krds-color-gray-5);

  /* Elevation 배경 - 깊이에 따라 밝기 증가 */
  --krds-color-surface-base: var(--krds-color-gray-100);
  --krds-color-surface-raised: var(--krds-color-gray-95);
  --krds-color-surface-overlay: var(--krds-color-gray-90);
  --krds-color-surface-sunken: var(--krds-color-gray-100);

  /* 텍스트 - 15:1 이상 대비 */
  --krds-color-text-primary: var(--krds-color-gray-0);
  --krds-color-text-secondary: var(--krds-color-gray-20);
  --krds-color-text-tertiary: var(--krds-color-gray-30);
  --krds-color-text-disabled: var(--krds-color-gray-60);
  --krds-color-text-inverse: var(--krds-color-gray-100);
  --krds-color-text-link: var(--krds-color-primary-30);
  --krds-color-text-link-visited: var(--krds-color-primary-20);

  /* 아이콘 */
  --krds-color-icon-primary: var(--krds-color-gray-0);
  --krds-color-icon-secondary: var(--krds-color-gray-30);
  --krds-color-icon-disabled: var(--krds-color-gray-60);
  --krds-color-icon-action: var(--krds-color-primary-30);

  /* 보더 - 두께 강화 */
  --krds-color-border-default: var(--krds-color-gray-70);
  --krds-color-border-strong: var(--krds-color-gray-50);
  --krds-color-border-focus: var(--krds-color-primary-30);
  --krds-color-border-disabled: var(--krds-color-gray-80);
  --krds-border-width-default: 2px;
  --krds-border-width-strong: 3px;
  --krds-border-width-focus: 3px;

  /* 시스템 색상 - 선명한 화면 모드 */
  --krds-color-danger-icon: var(--krds-color-danger-20);
  --krds-color-danger-text: var(--krds-color-danger-20);
  --krds-color-danger-bg: var(--krds-color-danger-95);
  --krds-color-danger-border: var(--krds-color-danger-90);
  --krds-color-success-icon: var(--krds-color-success-20);
  --krds-color-success-text: var(--krds-color-success-20);
  --krds-color-success-bg: var(--krds-color-success-95);
  --krds-color-warning-icon: var(--krds-color-warning-20);
  --krds-color-warning-text: var(--krds-color-warning-20);
  --krds-color-warning-bg: var(--krds-color-warning-95);
  --krds-color-info-icon: var(--krds-color-information-20);
  --krds-color-info-text: var(--krds-color-information-20);
  --krds-color-info-bg: var(--krds-color-information-95);

  /* 그림자 - 고대비에서는 보더로 대체 */
  --krds-shadow-sm: none;
  --krds-shadow-md: none;
  --krds-shadow-lg: none;

  /* 인라인 링크 밑줄 강화 */
  --krds-link-underline-width: 2px;
}
```

### 다크 모드 토큰 (Dark)

```css
[data-krds-mode="dark"] {
  /* 배경 */
  --krds-color-bg-primary: var(--krds-color-gray-95);
  --krds-color-bg-secondary: var(--krds-color-gray-90);
  --krds-color-bg-tertiary: var(--krds-color-gray-80);
  --krds-color-bg-inverse: var(--krds-color-gray-5);

  /* Elevation 배경 */
  --krds-color-surface-base: var(--krds-color-gray-95);
  --krds-color-surface-raised: var(--krds-color-gray-90);
  --krds-color-surface-overlay: var(--krds-color-gray-85);
  --krds-color-surface-sunken: var(--krds-color-gray-100);

  /* 텍스트 */
  --krds-color-text-primary: var(--krds-color-gray-5);
  --krds-color-text-secondary: var(--krds-color-gray-30);
  --krds-color-text-tertiary: var(--krds-color-gray-50);
  --krds-color-text-disabled: var(--krds-color-gray-60);
  --krds-color-text-inverse: var(--krds-color-gray-90);
  --krds-color-text-link: var(--krds-color-primary-40);
  --krds-color-text-link-visited: var(--krds-color-primary-30);

  /* 아이콘 */
  --krds-color-icon-primary: var(--krds-color-gray-5);
  --krds-color-icon-secondary: var(--krds-color-gray-40);
  --krds-color-icon-disabled: var(--krds-color-gray-60);
  --krds-color-icon-action: var(--krds-color-primary-40);

  /* 보더 */
  --krds-color-border-default: var(--krds-color-gray-70);
  --krds-color-border-strong: var(--krds-color-gray-50);
  --krds-color-border-focus: var(--krds-color-primary-40);
  --krds-color-border-disabled: var(--krds-color-gray-80);
  --krds-border-width-default: 1px;
  --krds-border-width-strong: 2px;
  --krds-border-width-focus: 2px;

  /* 시스템 색상 - 다크 모드 */
  --krds-color-danger-icon: var(--krds-color-danger-30);
  --krds-color-danger-text: var(--krds-color-danger-30);
  --krds-color-danger-bg: var(--krds-color-danger-90);
  --krds-color-danger-border: var(--krds-color-danger-80);
  --krds-color-success-icon: var(--krds-color-success-30);
  --krds-color-success-text: var(--krds-color-success-30);
  --krds-color-success-bg: var(--krds-color-success-90);
  --krds-color-warning-icon: var(--krds-color-warning-30);
  --krds-color-warning-text: var(--krds-color-warning-30);
  --krds-color-warning-bg: var(--krds-color-warning-90);
  --krds-color-info-icon: var(--krds-color-information-30);
  --krds-color-info-text: var(--krds-color-information-30);
  --krds-color-info-bg: var(--krds-color-information-90);

  /* 그림자 - 다크 모드에서는 더 강한 그림자 */
  --krds-shadow-sm: 0 1px 3px rgba(0, 0, 0, 0.3);
  --krds-shadow-md: 0 2px 8px rgba(0, 0, 0, 0.4);
  --krds-shadow-lg: 0 4px 16px rgba(0, 0, 0, 0.5);
}
```

---

## 5. 모드 전환 CSS 구현

### OS 설정 자동 감지

```css
/* OS 다크 모드 감지 - 사용자가 명시적으로 모드를 설정하지 않은 경우 */
@media (prefers-color-scheme: dark) {
  :root:not([data-krds-mode]) {
    --krds-color-bg-primary: var(--krds-color-gray-95);
    --krds-color-bg-secondary: var(--krds-color-gray-90);
    --krds-color-text-primary: var(--krds-color-gray-5);
    --krds-color-text-secondary: var(--krds-color-gray-30);
    --krds-color-border-default: var(--krds-color-gray-70);
    /* ... 다크 모드 토큰 전체 적용 */
  }
}

/* OS 고대비 모드 감지 */
@media (prefers-contrast: high) {
  :root:not([data-krds-mode]) {
    --krds-color-bg-primary: var(--krds-color-gray-100);
    --krds-color-text-primary: var(--krds-color-gray-0);
    --krds-border-width-default: 2px;
    --krds-border-width-focus: 3px;
    /* ... 선명한 화면 모드 토큰 전체 적용 */
  }
}

/* OS 저대비(reduced contrast) 감지 */
@media (prefers-contrast: less) {
  :root:not([data-krds-mode]) {
    /* 대비를 살짝 낮춘 부드러운 모드 */
    --krds-color-text-primary: var(--krds-color-gray-70);
    --krds-color-text-secondary: var(--krds-color-gray-50);
  }
}
```

### 컴포넌트별 모드 적용 예시

```css
/* 버튼 - 모드에 따른 자동 스타일 전환 */
.krds-button {
  background-color: var(--krds-color-primary-50);
  color: var(--krds-color-text-inverse);
  border: var(--krds-border-width-default) solid transparent;
  border-radius: var(--krds-radius-md);
}

[data-krds-mode="high-contrast"] .krds-button {
  border-color: var(--krds-color-primary-30);
  border-width: var(--krds-border-width-strong);
  text-decoration: underline;
}

[data-krds-mode="dark"] .krds-button {
  background-color: var(--krds-color-primary-40);
}

/* 카드 - 모드별 자동 전환 */
.krds-card {
  background-color: var(--krds-color-surface-raised);
  border: var(--krds-border-width-default) solid var(--krds-color-border-default);
  box-shadow: var(--krds-shadow-md);
}

[data-krds-mode="high-contrast"] .krds-card {
  box-shadow: none;
  border-width: var(--krds-border-width-strong);
}

/* 인풋 - 모드별 자동 전환 */
.krds-input {
  background-color: var(--krds-color-bg-primary);
  color: var(--krds-color-text-primary);
  border: var(--krds-border-width-default) solid var(--krds-color-border-default);
}

[data-krds-mode="high-contrast"] .krds-input {
  border-width: var(--krds-border-width-strong);
}

.krds-input:focus {
  border-color: var(--krds-color-border-focus);
  outline: var(--krds-border-width-focus) solid var(--krds-color-border-focus);
  outline-offset: -1px;
}
```

---

## 6. JavaScript 모드 전환 구현

### 모드 관리 클래스

```javascript
/**
 * KRDS 디스플레이 모드 관리자
 * 기본 모드, 선명한 화면 모드, 다크 모드 간 전환을 관리한다.
 */
class KRDSDisplayModeManager {
  static MODES = {
    DEFAULT: 'default',
    HIGH_CONTRAST: 'high-contrast',
    DARK: 'dark'
  };

  static STORAGE_KEY = 'krds-display-mode';
  static DATA_ATTR = 'data-krds-mode';

  constructor(options = {}) {
    this.root = options.root || document.documentElement;
    this.respectOS = options.respectOS !== false;  // 기본: OS 설정 존중
    this.onChange = options.onChange || null;        // 모드 변경 콜백

    this.currentMode = this._resolveInitialMode();
    this._applyMode(this.currentMode);

    if (this.respectOS) {
      this._watchOSPreferences();
    }
  }

  /** 초기 모드 결정: 저장값 > OS 설정 > 기본값 */
  _resolveInitialMode() {
    // 1. 사용자가 명시적으로 설정한 값
    const saved = localStorage.getItem(KRDSDisplayModeManager.STORAGE_KEY);
    if (saved && Object.values(KRDSDisplayModeManager.MODES).includes(saved)) {
      return saved;
    }

    // 2. OS 설정 감지
    if (this.respectOS) {
      if (window.matchMedia('(prefers-contrast: high)').matches) {
        return KRDSDisplayModeManager.MODES.HIGH_CONTRAST;
      }
      if (window.matchMedia('(prefers-color-scheme: dark)').matches) {
        return KRDSDisplayModeManager.MODES.DARK;
      }
    }

    // 3. 기본값
    return KRDSDisplayModeManager.MODES.DEFAULT;
  }

  /** DOM에 모드 적용 */
  _applyMode(mode) {
    const oldMode = this.currentMode;
    this.root.setAttribute(KRDSDisplayModeManager.DATA_ATTR, mode);
    this.currentMode = mode;

    // 프레임 요소에도 모드 전파
    this._propagateToFrames(mode);

    // 콜백 실행
    if (this.onChange && oldMode !== mode) {
      this.onChange({ oldMode, newMode: mode });
    }

    // 커스텀 이벤트 발행
    window.dispatchEvent(new CustomEvent('krds-mode-change', {
      detail: { oldMode, newMode: mode }
    }));
  }

  /** iframe 등 프레임 요소에 모드 전파 */
  _propagateToFrames(mode) {
    const frames = document.querySelectorAll('iframe[data-krds-frame]');
    frames.forEach(frame => {
      try {
        frame.contentDocument?.documentElement?.setAttribute(
          KRDSDisplayModeManager.DATA_ATTR, mode
        );
      } catch (e) {
        // cross-origin frame은 postMessage 사용
        frame.contentWindow?.postMessage(
          { type: 'krds-mode-change', mode }, '*'
        );
      }
    });
  }

  /** OS 설정 변경 실시간 감지 */
  _watchOSPreferences() {
    // 다크 모드 변경 감지
    window.matchMedia('(prefers-color-scheme: dark)')
      .addEventListener('change', (e) => {
        if (!localStorage.getItem(KRDSDisplayModeManager.STORAGE_KEY)) {
          this._applyMode(e.matches
            ? KRDSDisplayModeManager.MODES.DARK
            : KRDSDisplayModeManager.MODES.DEFAULT
          );
        }
      });

    // 고대비 모드 변경 감지
    window.matchMedia('(prefers-contrast: high)')
      .addEventListener('change', (e) => {
        if (!localStorage.getItem(KRDSDisplayModeManager.STORAGE_KEY)) {
          this._applyMode(e.matches
            ? KRDSDisplayModeManager.MODES.HIGH_CONTRAST
            : KRDSDisplayModeManager.MODES.DEFAULT
          );
        }
      });
  }

  /** 모드 설정 */
  setMode(mode) {
    if (!Object.values(KRDSDisplayModeManager.MODES).includes(mode)) {
      throw new Error(`유효하지 않은 모드: ${mode}`);
    }
    this._applyMode(mode);
    localStorage.setItem(KRDSDisplayModeManager.STORAGE_KEY, mode);
  }

  /** 현재 모드 조회 */
  getMode() {
    return this.currentMode;
  }

  /** 사용자 설정 초기화 (OS 설정으로 복귀) */
  resetToOSDefault() {
    localStorage.removeItem(KRDSDisplayModeManager.STORAGE_KEY);
    this._applyMode(this._resolveInitialMode());
  }

  /** 모드 순환 전환 (default → dark → high-contrast → default) */
  cycleMode() {
    const modes = Object.values(KRDSDisplayModeManager.MODES);
    const currentIndex = modes.indexOf(this.currentMode);
    const nextIndex = (currentIndex + 1) % modes.length;
    this.setMode(modes[nextIndex]);
  }
}
```

### 모드 전환 UI 컴포넌트 HTML

```html
<!-- 모드 전환 버튼 그룹 -->
<div class="krds-mode-switcher" role="radiogroup" aria-label="화면 모드 설정">
  <button
    class="krds-mode-switcher__option"
    role="radio"
    aria-checked="true"
    data-mode="default"
    aria-label="기본 모드"
  >
    <svg aria-hidden="true" width="20" height="20"><!-- 태양 아이콘 --></svg>
    <span>기본</span>
  </button>
  <button
    class="krds-mode-switcher__option"
    role="radio"
    aria-checked="false"
    data-mode="dark"
    aria-label="다크 모드"
  >
    <svg aria-hidden="true" width="20" height="20"><!-- 달 아이콘 --></svg>
    <span>다크</span>
  </button>
  <button
    class="krds-mode-switcher__option"
    role="radio"
    aria-checked="false"
    data-mode="high-contrast"
    aria-label="선명한 화면 모드"
  >
    <svg aria-hidden="true" width="20" height="20"><!-- 대비 아이콘 --></svg>
    <span>선명</span>
  </button>
</div>

<script>
  const modeManager = new KRDSDisplayModeManager({
    respectOS: true,
    onChange: ({ newMode }) => {
      // UI 상태 업데이트
      document.querySelectorAll('.krds-mode-switcher__option').forEach(btn => {
        const isActive = btn.dataset.mode === newMode;
        btn.setAttribute('aria-checked', isActive);
        btn.classList.toggle('is-active', isActive);
      });
    }
  });

  // 버튼 이벤트 바인딩
  document.querySelectorAll('.krds-mode-switcher__option').forEach(btn => {
    btn.addEventListener('click', () => {
      modeManager.setMode(btn.dataset.mode);
    });
  });
</script>
```

---

## 7. 상태 표현

- 텍스트/아이콘 아래 영역의 **배경색**은 모드에 상관없이 일관된 색상 상태 규칙 사용
- 텍스트/아이콘/컴포넌트의 **윤곽선에 색이 사용**된 경우, 선명한 화면 모드에서는 **일반 모드와 반대 방식**으로 정의된 상태 규칙 사용

---

## 8. 계층 구조

- 어두운 배경에서는 면의 깊이에 따라 레이어 배경면이 **점차 밝아지도록** 표현
- 선명한 화면 모드에서는 기본 모드보다 시각적으로 계층 구조가 **더 명확하게 드러남**

### Elevation별 모드 토큰 매핑

| Elevation | 기본 모드 | 다크 모드 | 선명한 화면 모드 |
|---|---|---|---|
| **-1 (Sunken)** | gray-10 | gray-100 | gray-100 |
| **0 (Base)** | gray-0 | gray-95 | gray-100 |
| **+1 (Raised)** | gray-5 | gray-90 | gray-95 |
| **+2 (Overlay)** | gray-0 | gray-85 | gray-90 |

---

## 9. 시각 보조

콘텐츠의 특성과 이용 맥락을 고려하여 특정 요소에 시각 보조를 제공한다.

| 영역 | 시각 보조 방법 |
|---|---|
| **헤더** | 유틸리티 링크에 윤곽선/배경색 표시 |
| **푸터** | 모든 링크에 밑줄 표시 |
| **액션/선택/입력** | 버튼, 입력, 선택 컴포넌트의 외곽선/링크 밑줄을 기본 모드보다 굵게 제공. 명도 대비 향상. 모든 인라인 링크에 밑줄 표시 |

> **주의**: 모든 대화형 요소에 동일한 시각 보조를 제공하면 시각적 복잡도가 증가하거나 강조 수준이 부적절하게 변경될 수 있다.

---

## 10. 사용성 가이드라인

### 디자인 토큰 규칙 준수
- 디자인 토큰에 정의된 규칙에 따라 모든 스타일 체계가 선명한 화면 모드로 전환되도록 한다

### 흰 배경 사용 유의
- **모범 사례**: 어두운 화면에 넓은 면적의 흰 배경을 피하고, 색상 팔레트에서 조금 더 어두운 색상 사용
- **피해야 할 사례**: 넓은 면적의 흰 배경 요소 사용 (눈부심 유발, 지나친 강조)

### 이미지 호환성
- **모범 사례**: 일반 모드와 선명한 화면 모드에 잘 호환되는 이미지 사용
- 래스터 이미지(JPEG, PNG, GIF): 두 모드에 대응 가능하도록 **별도 버전 준비**
- 벡터 이미지(SVG): 디자인 토큰의 색상 규칙에 따라 적용되도록 구현
- **피해야 할 사례**: 모드 전환 시 이미지가 식별 불가

#### 이미지 모드 대응 구현 예시

```html
<!-- picture 요소를 활용한 모드별 이미지 제공 -->
<picture>
  <source
    media="(prefers-color-scheme: dark)"
    srcset="/images/hero-dark.webp"
  />
  <source
    media="(prefers-contrast: high)"
    srcset="/images/hero-high-contrast.webp"
  />
  <img src="/images/hero-light.webp" alt="서비스 소개 이미지" />
</picture>
```

```css
/* CSS를 활용한 SVG 아이콘 모드 대응 */
.krds-icon {
  color: var(--krds-color-icon-primary);
  /* currentColor를 사용하는 SVG는 자동으로 모드에 대응 */
}

/* 배경 이미지 모드 대응 */
.krds-hero {
  background-image: url('/images/hero-light.webp');
}

[data-krds-mode="dark"] .krds-hero,
[data-krds-mode="high-contrast"] .krds-hero {
  background-image: url('/images/hero-dark.webp');
}
```

### 모드 일관성 유지
- **모범 사례**: 한 서비스 내에서 모든 화면/요소에 일관된 모드 적용. 채널/플랫폼 간 전환 시에도 유지
- **피해야 할 사례**: 화면 이동 시 모드가 일관되지 않음

### 프레임 요소 스타일
- 프레임으로 포함된 문서는 부모 화면의 스타일 속성을 **상속하지 못함**
- 프레임 요소가 선명한 화면 모드를 지원한다면, 부모 화면 전환 시 프레임도 함께 전환되도록 구현

#### iframe 모드 동기화 구현

```javascript
// 부모 페이지: iframe에 모드 변경 알림
window.addEventListener('krds-mode-change', (e) => {
  document.querySelectorAll('iframe[data-krds-frame]').forEach(frame => {
    frame.contentWindow.postMessage({
      type: 'krds-mode-change',
      mode: e.detail.newMode
    }, '*');
  });
});

// iframe 내부: 부모로부터 모드 변경 수신
window.addEventListener('message', (e) => {
  if (e.data?.type === 'krds-mode-change') {
    document.documentElement.setAttribute('data-krds-mode', e.data.mode);
  }
});
```

---

## 11. 접근성 가이드라인

### 명도 대비 충족
- 전경과 배경 간 명도 대비 **최소 4.5:1 이상**, 가능하면 **7:1 이상**
- WCAG 2.1 Contrast (Minimum) (AA)
- WCAG 2.1 Contrast (Enhanced) (AAA)

### 콘텐츠/기능 손실 방지
- 선명한 화면 모드 전환 시 콘텐츠나 기능이 손실되지 않도록 함
- WCAG 2.1 Contrast (Minimum) (AA)
- WCAG 2.1 Contrast (Enhanced) (AAA)
- WCAG 2.1 Non-text Contrast (AA)

### 모드 전환 접근성 체크리스트

| 항목 | 확인 내용 | WCAG 기준 |
|---|---|---|
| 키보드 접근 | 모드 전환 컨트롤이 키보드로 접근 가능한가 | 2.1.1 Keyboard (A) |
| 포커스 표시 | 모드 전환 후에도 포커스 표시가 명확한가 | 2.4.7 Focus Visible (AA) |
| 상태 전달 | 현재 모드가 스크린리더에 전달되는가 | 4.1.2 Name, Role, Value (A) |
| 대비 유지 | 모든 모드에서 WCAG AA 대비 기준을 충족하는가 | 1.4.3 Contrast (Minimum) (AA) |
| 콘텐츠 보존 | 모드 전환 시 콘텐츠가 손실되지 않는가 | 1.3.1 Info and Relationships (A) |
