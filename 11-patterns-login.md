---
title: "로그인 서비스 패턴"
source_pages: "977-1043"
---

# 로그인 서비스 패턴

로그인은 사용자의 신원을 확인하는 과정으로 사용자가 서비스에 접근할 수 있도록 하는 수단이다. 사용자에게 개인화된 경험을 제공하거나 사용자의 신분/신원을 인증하고자 하는 경우에 사용하기 적합하다.

---

## 1. 인증 유형

| 유형 | 설명 | 예시 |
|---|---|---|
| **지식 기반 인증** | 사용자만이 유일하게 알고 있는 내용에 기반 | 아이디/패스워드, PIN |
| **소유 기반 인증** | 사용자가 보유하고 있는 품목을 활용 | 토큰, 공동인증, 간편인증, 문자 인증 |
| **생체 기반 인증** | 사용자의 생체 정보, 행동에 기반 | 지문 인증, 페이스 아이디 |
| **다중 요소 인증** | 여러 가지 인증 방식을 결합하여 제공 | 복합 인증 |

---

## 2. 사용자 여정 (7단계 플로)

### 전체 플로

1. **로그인 기능 찾기** - 로그인 수단을 탐색하고 발견하는 과정
2. **로그인 안내** - 로그인 화면으로 전환에 대한 설명을 확인하고 진행 여부를 확정
3. **로그인 방식 확인 및 선택** - 2개 이상의 로그인 방식에 대해 비교하고 선택
4. **로그인 정보 입력** - 인증 방식에 따라 정보를 입력
5. **로그인 완료** - 로그인이 성공적으로 완료되어 화면을 벗어남
6. **서비스 이용** - 로그인 유지 상태에서 서비스를 이용
7. **로그아웃** - 인증을 해제 (자발적 또는 세션 만료로 자동 해제)

---

## 3. 단계별 사용성 가이드라인

### 3.1 로그인 기능 찾기

**구조**: 아이콘 + 레이블

#### 사용성 가이드라인

| # | 가이드라인 | 적용 수준 |
|---|---|---|
| 01 | 로그인 링크는 모든 화면에서 일관된 위치에 주목도 있게 배치한다 | 필수 |
| 02 | 헤더에서 제공되는 로그인 링크는 사용자 아이콘과 레이블로 구성한다 | 권장 |
| 03 | 로그인 링크는 항상 '로그인' 화면으로 연결되어야 한다 | 필수 |

#### 플랫폼 고려 사항
- 화면 너비가 충분하지 않은 경우에도 로그인 링크를 직관적으로 인지할 수 있는 형태로 표현
- 가능한 한 로그인 링크를 숨기지 않고 헤더나 탭바 영역에 상시 배치
- 서비스 이용에 로그인이 중요하지 않다면 메뉴 레이어에서만 제공 가능

#### HTML 구현 예시

```html
<!-- 헤더 로그인 링크 -->
<header class="krds-header" role="banner">
  <nav class="krds-header__utility" aria-label="유틸리티 메뉴">
    <a href="/login" class="krds-login-link">
      <svg class="krds-login-link__icon" aria-hidden="true" width="24" height="24" viewBox="0 0 24 24">
        <path d="M12 12c2.21 0 4-1.79 4-4s-1.79-4-4-4-4 1.79-4 4 1.79 4 4 4zm0 2c-2.67 0-8 1.34-8 4v2h16v-2c0-2.66-5.33-4-8-4z" fill="currentColor"/>
      </svg>
      <span class="krds-login-link__label">로그인</span>
    </a>
  </nav>
</header>
```

```css
.krds-login-link {
  display: inline-flex;
  align-items: center;
  gap: var(--krds-gap-2);
  padding: var(--krds-spacing-2) var(--krds-spacing-3);
  color: var(--krds-color-text-primary);
  text-decoration: none;
  font-size: var(--krds-font-size-body);
  font-family: var(--krds-font-family);
  border-radius: var(--krds-radius-sm);
  transition: background-color 0.2s ease;
}

.krds-login-link:hover {
  background-color: var(--krds-color-bg-secondary);
}

.krds-login-link:focus-visible {
  outline: var(--krds-border-width-focus) solid var(--krds-color-border-focus);
  outline-offset: 2px;
}

.krds-login-link__icon {
  flex-shrink: 0;
  color: var(--krds-color-icon-secondary);
}

/* 모바일: 아이콘만 표시 */
@media (max-width: 768px) {
  .krds-login-link__label {
    /* sr-only: 스크린리더에서는 읽히되 시각적으로 숨김 */
    position: absolute;
    width: 1px;
    height: 1px;
    padding: 0;
    margin: -1px;
    overflow: hidden;
    clip: rect(0, 0, 0, 0);
    white-space: nowrap;
    border-width: 0;
  }
}
```

---

### 3.2 로그인 방식 확인 및 선택

**구조** (10개 요소):
1. 부제목 - 특정 기관/서비스의 로그인 화면임을 안내
2. 제목 - 이용 목적을 선택하도록 유도
3. 설명 - 로그인 목적 선택에 대한 이해를 돕는 텍스트
4. 방식 선택 목록 - 사용자 유형/목적에 따라 화면 분리
5. 방식 선택 탭 - 통합 로그인과 기존 서비스 로그인 분리
6. 섹션 제목 - 인증 후 이용 가능한 서비스 범위 식별
7. 방식 선택 목록 - 서비스 범위에 따른 로그인 방식 선택
8. 디스클로저 - 로그인 방식에 대한 세부 정보/참고 사항
9. 로그인 관련 도움말 - 질문 및 문제 해결 링크
10. 회원 가입 링크 - 계정 생성 화면 연결

#### 사용성 가이드라인

| # | 가이드라인 | 적용 수준 |
|---|---|---|
| 01 | 빈번하게 사용되는 로그인 방식에 우선적으로 접근할 수 있는 수단을 제공한다 | 권장 |
| 02 | 동일한 수준의 로그인 방식을 탭으로 구분하여 선택하게 하지 않는다 | 필수 |
| 03 | 다양한 로그인 방식에 대한 분류 체계에 사용자의 관점과 언어를 반영한다 | 권장 |
| 04 | 로그인 방식에 따라 서비스 이용 절차가 달라지는 경우, 사전에 안내를 제공한다 | 필수 |
| 05 | 로그인 방식과 입력 항목에 대한 직관적인 안내를 제공한다 | 필수 |
| 06 | 도움말 정보, 매뉴얼 링크를 제공한다 | 권장 |
| 07 | 도움말 정보는 사용자의 다양한 이용 환경을 고려하여 작성한다 | 우수 |

#### 로그인 폼 HTML 구현

```html
<div class="krds-login-page">
  <!-- 부제목 -->
  <p class="krds-login-page__subtitle">대한민국 정부서비스</p>
  <!-- 제목 -->
  <h1 class="krds-login-page__title">로그인</h1>
  <!-- 설명 -->
  <p class="krds-login-page__description">
    이용 목적에 따라 로그인 방식을 선택해 주세요.
  </p>

  <!-- 로그인 방식 선택 -->
  <div class="krds-login-methods" role="group" aria-label="로그인 방식 선택">
    <!-- 섹션 제목 -->
    <h2 class="krds-login-methods__section-title">간편 인증</h2>

    <ul class="krds-login-methods__list">
      <li>
        <button class="krds-login-method" data-method="kakao">
          <img src="/icons/kakao.svg" alt="" class="krds-login-method__icon" />
          <span class="krds-login-method__name">카카오 인증</span>
          <span class="krds-login-method__description">카카오톡 앱에서 인증</span>
        </button>
      </li>
      <li>
        <button class="krds-login-method" data-method="pass">
          <img src="/icons/pass.svg" alt="" class="krds-login-method__icon" />
          <span class="krds-login-method__name">PASS 인증</span>
          <span class="krds-login-method__description">통신사 PASS 앱에서 인증</span>
        </button>
      </li>
      <li>
        <button class="krds-login-method" data-method="certificate">
          <img src="/icons/cert.svg" alt="" class="krds-login-method__icon" />
          <span class="krds-login-method__name">공동인증서</span>
          <span class="krds-login-method__description">공동인증서로 로그인</span>
        </button>
      </li>
    </ul>

    <h2 class="krds-login-methods__section-title">아이디/비밀번호</h2>

    <!-- 아이디/비밀번호 로그인 폼 -->
    <form class="krds-login-form" id="loginForm" novalidate>
      <div class="krds-form-field">
        <label for="userId" class="krds-form-field__label">아이디</label>
        <input
          type="text"
          id="userId"
          name="userId"
          class="krds-input"
          autocomplete="username"
          required
          aria-describedby="userId-error"
          placeholder="아이디를 입력해 주세요"
        />
        <p id="userId-error" class="krds-form-field__error" role="alert" aria-live="polite" hidden>
          아이디를 입력해 주세요.
        </p>
      </div>

      <div class="krds-form-field">
        <label for="userPw" class="krds-form-field__label">비밀번호</label>
        <div class="krds-input-group">
          <input
            type="password"
            id="userPw"
            name="userPw"
            class="krds-input"
            autocomplete="current-password"
            required
            aria-describedby="userPw-error"
            placeholder="비밀번호를 입력해 주세요"
          />
          <button
            type="button"
            class="krds-input-group__toggle"
            aria-label="비밀번호 표시"
            data-toggle-password="userPw"
          >
            <svg aria-hidden="true" width="20" height="20"><!-- 눈 아이콘 --></svg>
          </button>
        </div>
        <p id="userPw-error" class="krds-form-field__error" role="alert" aria-live="polite" hidden>
          비밀번호를 입력해 주세요.
        </p>
      </div>

      <div class="krds-form-field krds-form-field--inline">
        <label class="krds-checkbox">
          <input type="checkbox" name="rememberMe" class="krds-checkbox__input" />
          <span class="krds-checkbox__label">아이디 저장</span>
        </label>
      </div>

      <button type="submit" class="krds-button krds-button--primary krds-button--full">
        로그인
      </button>
    </form>

    <!-- 도움말 링크 -->
    <div class="krds-login-help">
      <a href="/find-id" class="krds-login-help__link">아이디 찾기</a>
      <span class="krds-login-help__divider" aria-hidden="true">|</span>
      <a href="/find-pw" class="krds-login-help__link">비밀번호 찾기</a>
      <span class="krds-login-help__divider" aria-hidden="true">|</span>
      <a href="/signup" class="krds-login-help__link">회원 가입</a>
    </div>

    <!-- 디스클로저: 로그인 방식 안내 -->
    <details class="krds-disclosure">
      <summary class="krds-disclosure__summary">로그인 방식 안내</summary>
      <div class="krds-disclosure__content">
        <p>간편 인증은 앱을 통한 본인 확인 방식입니다. 공동인증서는 별도 설치가 필요합니다.</p>
      </div>
    </details>
  </div>
</div>
```

#### 로그인 폼 CSS 스타일

```css
.krds-login-page {
  max-width: 480px;
  margin: 0 auto;
  padding: var(--krds-spacing-8) var(--krds-spacing-4);
}

.krds-login-page__subtitle {
  font-size: var(--krds-font-size-caption);
  color: var(--krds-color-text-secondary);
  margin-bottom: var(--krds-gap-1);
}

.krds-login-page__title {
  font-size: var(--krds-font-size-heading-1);
  font-weight: var(--krds-font-weight-bold);
  color: var(--krds-color-text-primary);
  margin-bottom: var(--krds-gap-2);
}

.krds-login-page__description {
  font-size: var(--krds-font-size-body);
  color: var(--krds-color-text-secondary);
  margin-bottom: var(--krds-gap-6);
}

/* 로그인 방식 선택 버튼 */
.krds-login-methods__list {
  list-style: none;
  padding: 0;
  display: flex;
  flex-direction: column;
  gap: var(--krds-gap-2);
  margin-bottom: var(--krds-gap-6);
}

.krds-login-method {
  display: flex;
  align-items: center;
  gap: var(--krds-gap-3);
  width: 100%;
  padding: var(--krds-spacing-4);
  border: var(--krds-border-width-default) solid var(--krds-color-border-default);
  border-radius: var(--krds-radius-md);
  background-color: var(--krds-color-bg-primary);
  cursor: pointer;
  text-align: left;
  transition: border-color 0.2s ease, background-color 0.2s ease;
  font-family: var(--krds-font-family);
}

.krds-login-method:hover {
  border-color: var(--krds-color-primary-50);
  background-color: var(--krds-color-bg-secondary);
}

.krds-login-method:focus-visible {
  outline: var(--krds-border-width-focus) solid var(--krds-color-border-focus);
  outline-offset: 2px;
}

/* 폼 필드 */
.krds-form-field {
  margin-bottom: var(--krds-gap-4);
}

.krds-form-field__label {
  display: block;
  font-size: var(--krds-font-size-body);
  font-weight: var(--krds-font-weight-medium);
  color: var(--krds-color-text-primary);
  margin-bottom: var(--krds-gap-1);
}

.krds-form-field__error {
  font-size: var(--krds-font-size-caption);
  color: var(--krds-color-danger-text);
  margin-top: var(--krds-gap-1);
  display: flex;
  align-items: center;
  gap: var(--krds-gap-1);
}

.krds-form-field__error::before {
  content: '';
  display: inline-block;
  width: 16px;
  height: 16px;
  background: url("data:image/svg+xml,...") no-repeat center;
  flex-shrink: 0;
}

/* 에러 상태 인풋 */
.krds-input[aria-invalid="true"],
.krds-input.is-error {
  border-color: var(--krds-color-border-error);
}

.krds-input[aria-invalid="true"]:focus,
.krds-input.is-error:focus {
  outline-color: var(--krds-color-danger-50);
}

/* 비밀번호 토글 버튼 */
.krds-input-group {
  position: relative;
}

.krds-input-group .krds-input {
  width: 100%;
  padding-right: 48px;
}

.krds-input-group__toggle {
  position: absolute;
  right: 0;
  top: 0;
  bottom: 0;
  width: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  background: none;
  border: none;
  cursor: pointer;
  color: var(--krds-color-icon-secondary);
}

/* 도움말 링크 영역 */
.krds-login-help {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: var(--krds-gap-3);
  margin-top: var(--krds-gap-4);
  font-size: var(--krds-font-size-caption);
}

.krds-login-help__link {
  color: var(--krds-color-text-secondary);
  text-decoration: none;
}

.krds-login-help__link:hover {
  color: var(--krds-color-text-link);
  text-decoration: underline;
}

.krds-login-help__divider {
  color: var(--krds-color-border-default);
}
```

---

### 3.3 JavaScript 폼 검증 및 상태 관리

#### 로그인 폼 검증 구현

```javascript
/**
 * KRDS 로그인 폼 관리자
 * 클라이언트 사이드 폼 검증, 상태 관리, 에러 처리를 담당한다.
 */
class KRDSLoginForm {
  constructor(formElement, options = {}) {
    this.form = typeof formElement === 'string'
      ? document.querySelector(formElement)
      : formElement;

    this.options = {
      validateOnBlur: true,          // blur 이벤트에서 검증
      validateOnInput: false,        // input 이벤트에서 검증 (에러 상태에서만)
      showErrorOnSubmit: true,       // submit 시 에러 표시
      rememberIdKey: 'krds-login-remember-id',
      onSuccess: null,               // 로그인 성공 콜백
      onError: null,                 // 로그인 에러 콜백
      loginEndpoint: '/api/auth/login',
      ...options
    };

    this.state = {
      isSubmitting: false,
      errors: {},
      touched: {}
    };

    this.validators = {
      userId: [
        { test: (v) => !!v.trim(), message: '아이디를 입력해 주세요.' },
        { test: (v) => v.length >= 4, message: '아이디는 4자 이상이어야 합니다.' }
      ],
      userPw: [
        { test: (v) => !!v.trim(), message: '비밀번호를 입력해 주세요.' },
        { test: (v) => v.length >= 8, message: '비밀번호는 8자 이상이어야 합니다.' }
      ]
    };

    this._init();
  }

  /** 초기화 */
  _init() {
    // 이벤트 바인딩
    this.form.addEventListener('submit', (e) => this._handleSubmit(e));

    // blur 검증
    if (this.options.validateOnBlur) {
      this.form.querySelectorAll('.krds-input').forEach(input => {
        input.addEventListener('blur', (e) => this._validateField(e.target));
        // 에러 상태에서 input 이벤트로 실시간 검증
        input.addEventListener('input', (e) => {
          if (this.state.errors[e.target.name]) {
            this._validateField(e.target);
          }
        });
      });
    }

    // 비밀번호 표시/숨기기 토글
    this.form.querySelectorAll('[data-toggle-password]').forEach(btn => {
      btn.addEventListener('click', (e) => this._togglePasswordVisibility(e));
    });

    // 저장된 아이디 복원
    this._restoreRememberedId();
  }

  /** 필드 검증 */
  _validateField(input) {
    const { name, value } = input;
    const validators = this.validators[name];
    if (!validators) return true;

    this.state.touched[name] = true;

    for (const validator of validators) {
      if (!validator.test(value)) {
        this._showError(name, validator.message);
        return false;
      }
    }

    this._clearError(name);
    return true;
  }

  /** 전체 폼 검증 */
  _validateAll() {
    let isValid = true;
    const inputs = this.form.querySelectorAll('.krds-input[required]');

    inputs.forEach(input => {
      if (!this._validateField(input)) {
        isValid = false;
      }
    });

    return isValid;
  }

  /** 에러 메시지 표시 */
  _showError(fieldName, message) {
    this.state.errors[fieldName] = message;

    const input = this.form.querySelector(`[name="${fieldName}"]`);
    const errorEl = this.form.querySelector(`#${fieldName}-error`);

    if (input) {
      input.setAttribute('aria-invalid', 'true');
      input.classList.add('is-error');
    }

    if (errorEl) {
      errorEl.textContent = message;
      errorEl.hidden = false;
    }
  }

  /** 에러 메시지 제거 */
  _clearError(fieldName) {
    delete this.state.errors[fieldName];

    const input = this.form.querySelector(`[name="${fieldName}"]`);
    const errorEl = this.form.querySelector(`#${fieldName}-error`);

    if (input) {
      input.removeAttribute('aria-invalid');
      input.classList.remove('is-error');
    }

    if (errorEl) {
      errorEl.textContent = '';
      errorEl.hidden = true;
    }
  }

  /** 폼 제출 처리 */
  async _handleSubmit(event) {
    event.preventDefault();

    if (this.state.isSubmitting) return;

    // 전체 검증
    if (!this._validateAll()) {
      // 첫 번째 에러 필드에 포커스
      const firstError = this.form.querySelector('[aria-invalid="true"]');
      if (firstError) firstError.focus();
      return;
    }

    // 로딩 상태 설정
    this.state.isSubmitting = true;
    this._setLoadingState(true);

    try {
      const formData = new FormData(this.form);
      const data = Object.fromEntries(formData);

      // 아이디 저장 처리
      if (data.rememberMe) {
        localStorage.setItem(this.options.rememberIdKey, data.userId);
      } else {
        localStorage.removeItem(this.options.rememberIdKey);
      }

      // API 호출
      const response = await fetch(this.options.loginEndpoint, {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          userId: data.userId,
          password: data.userPw
        })
      });

      if (!response.ok) {
        const errorData = await response.json().catch(() => ({}));
        throw new LoginError(
          errorData.message || '로그인에 실패했습니다.',
          response.status,
          errorData
        );
      }

      const result = await response.json();

      // 성공 콜백
      if (this.options.onSuccess) {
        this.options.onSuccess(result);
      }
    } catch (error) {
      this._handleLoginError(error);
    } finally {
      this.state.isSubmitting = false;
      this._setLoadingState(false);
    }
  }

  /** 로그인 에러 처리 */
  _handleLoginError(error) {
    let message = '로그인에 실패했습니다. 다시 시도해 주세요.';

    if (error instanceof LoginError) {
      switch (error.status) {
        case 401:
          message = '아이디 또는 비밀번호가 올바르지 않습니다.';
          break;
        case 403:
          message = '계정이 잠겼습니다. 관리자에게 문의해 주세요.';
          break;
        case 429:
          message = '로그인 시도가 너무 많습니다. 잠시 후 다시 시도해 주세요.';
          break;
        case 500:
          message = '서버 오류가 발생했습니다. 잠시 후 다시 시도해 주세요.';
          break;
      }
    }

    // 에러 알림 표시
    this._showGlobalError(message);

    // 에러 콜백
    if (this.options.onError) {
      this.options.onError(error);
    }
  }

  /** 전역 에러 메시지 표시 */
  _showGlobalError(message) {
    let alertEl = this.form.querySelector('.krds-login-form__alert');
    if (!alertEl) {
      alertEl = document.createElement('div');
      alertEl.className = 'krds-login-form__alert';
      alertEl.setAttribute('role', 'alert');
      alertEl.setAttribute('aria-live', 'assertive');
      this.form.prepend(alertEl);
    }

    alertEl.innerHTML = `
      <svg aria-hidden="true" width="20" height="20" class="krds-login-form__alert-icon">
        <path d="M10 0C4.48 0 0 4.48 0 10s4.48 10 10 10 10-4.48 10-10S15.52 0 10 0zm1 15H9v-2h2v2zm0-4H9V5h2v6z" fill="currentColor"/>
      </svg>
      <span>${message}</span>
    `;
    alertEl.hidden = false;

    // 에러 메시지에 포커스 이동 (스크린리더 접근성)
    alertEl.focus();
  }

  /** 로딩 상태 설정 */
  _setLoadingState(isLoading) {
    const submitBtn = this.form.querySelector('[type="submit"]');
    if (!submitBtn) return;

    if (isLoading) {
      submitBtn.disabled = true;
      submitBtn.setAttribute('aria-busy', 'true');
      submitBtn.dataset.originalText = submitBtn.textContent;
      submitBtn.innerHTML = `
        <span class="krds-spinner krds-spinner--sm" aria-hidden="true"></span>
        <span>로그인 중...</span>
      `;
    } else {
      submitBtn.disabled = false;
      submitBtn.removeAttribute('aria-busy');
      submitBtn.textContent = submitBtn.dataset.originalText || '로그인';
    }
  }

  /** 비밀번호 표시/숨기기 토글 */
  _togglePasswordVisibility(event) {
    const btn = event.currentTarget;
    const targetId = btn.dataset.togglePassword;
    const input = document.getElementById(targetId);

    if (!input) return;

    const isVisible = input.type === 'text';
    input.type = isVisible ? 'password' : 'text';
    btn.setAttribute('aria-label', isVisible ? '비밀번호 표시' : '비밀번호 숨기기');
    btn.setAttribute('aria-pressed', !isVisible);
  }

  /** 저장된 아이디 복원 */
  _restoreRememberedId() {
    const savedId = localStorage.getItem(this.options.rememberIdKey);
    if (savedId) {
      const userIdInput = this.form.querySelector('[name="userId"]');
      const rememberCheckbox = this.form.querySelector('[name="rememberMe"]');
      if (userIdInput) userIdInput.value = savedId;
      if (rememberCheckbox) rememberCheckbox.checked = true;
    }
  }
}

/** 로그인 에러 클래스 */
class LoginError extends Error {
  constructor(message, status, data) {
    super(message);
    this.name = 'LoginError';
    this.status = status;
    this.data = data;
  }
}

// 사용 예시
document.addEventListener('DOMContentLoaded', () => {
  const loginForm = new KRDSLoginForm('#loginForm', {
    loginEndpoint: '/api/auth/login',
    onSuccess: (result) => {
      // 로그인 성공 시 리다이렉트
      const redirectUrl = new URLSearchParams(window.location.search).get('redirect') || '/';
      window.location.href = redirectUrl;
    },
    onError: (error) => {
      console.error('로그인 실패:', error);
    }
  });
});
```

#### 세션 관리 및 로그아웃

```javascript
/**
 * KRDS 세션 관리자
 * 로그인 상태 유지, 세션 만료 처리, 로그아웃을 관리한다.
 */
class KRDSSessionManager {
  constructor(options = {}) {
    this.options = {
      sessionCheckInterval: 60000,   // 1분마다 세션 확인
      sessionCheckEndpoint: '/api/auth/session',
      logoutEndpoint: '/api/auth/logout',
      warningBeforeExpiry: 300000,   // 만료 5분 전 경고
      onSessionExpired: null,
      onSessionWarning: null,
      ...options
    };

    this.isAuthenticated = false;
    this.sessionTimer = null;
    this.warningTimer = null;

    this._init();
  }

  /** 초기화 */
  _init() {
    this._startSessionCheck();

    // 페이지 가시성 변경 감지
    document.addEventListener('visibilitychange', () => {
      if (document.visibilityState === 'visible') {
        this._checkSession();
      }
    });
  }

  /** 세션 상태 주기적 확인 */
  _startSessionCheck() {
    this._checkSession();
    this.sessionTimer = setInterval(
      () => this._checkSession(),
      this.options.sessionCheckInterval
    );
  }

  /** 세션 확인 API 호출 */
  async _checkSession() {
    try {
      const response = await fetch(this.options.sessionCheckEndpoint, {
        credentials: 'include'
      });

      if (!response.ok) {
        this._handleSessionExpired();
        return;
      }

      const data = await response.json();
      this.isAuthenticated = data.authenticated;

      // 세션 만료 임박 경고
      if (data.expiresIn && data.expiresIn <= this.options.warningBeforeExpiry) {
        this._showExpiryWarning(data.expiresIn);
      }
    } catch (error) {
      console.error('세션 확인 실패:', error);
    }
  }

  /** 세션 만료 임박 경고 */
  _showExpiryWarning(expiresIn) {
    const minutes = Math.ceil(expiresIn / 60000);

    if (this.options.onSessionWarning) {
      this.options.onSessionWarning(minutes);
      return;
    }

    // 기본 경고 모달
    const shouldExtend = confirm(
      `세션이 ${minutes}분 후 만료됩니다. 연장하시겠습니까?`
    );

    if (shouldExtend) {
      this.extendSession();
    }
  }

  /** 세션 연장 */
  async extendSession() {
    try {
      await fetch(this.options.sessionCheckEndpoint, {
        method: 'POST',
        credentials: 'include'
      });
    } catch (error) {
      console.error('세션 연장 실패:', error);
    }
  }

  /** 세션 만료 처리 */
  _handleSessionExpired() {
    this.isAuthenticated = false;
    clearInterval(this.sessionTimer);

    if (this.options.onSessionExpired) {
      this.options.onSessionExpired();
    } else {
      // 기본: 로그인 페이지로 리다이렉트
      const currentUrl = encodeURIComponent(window.location.href);
      window.location.href = `/login?redirect=${currentUrl}&expired=true`;
    }
  }

  /** 로그아웃 */
  async logout() {
    try {
      await fetch(this.options.logoutEndpoint, {
        method: 'POST',
        credentials: 'include'
      });
    } catch (error) {
      console.error('로그아웃 API 호출 실패:', error);
    } finally {
      this.isAuthenticated = false;
      clearInterval(this.sessionTimer);
      window.location.href = '/';
    }
  }
}

// 사용 예시
const sessionManager = new KRDSSessionManager({
  onSessionWarning: (minutes) => {
    // KRDS 모달 컴포넌트로 경고 표시
    const modal = document.querySelector('#session-warning-modal');
    modal.querySelector('.krds-modal__body').textContent =
      `세션이 ${minutes}분 후 만료됩니다. 연장하시겠습니까?`;
    modal.hidden = false;
  },
  onSessionExpired: () => {
    const currentUrl = encodeURIComponent(window.location.href);
    window.location.href = `/login?redirect=${currentUrl}&expired=true`;
  }
});

// 로그아웃 버튼 이벤트
document.querySelector('#logoutBtn')?.addEventListener('click', () => {
  sessionManager.logout();
});
```

---

### 3.4 로그아웃

- 사용자가 자발적으로 수행하거나 세션 유지 시간 만료로 인해 자동으로 해제될 수 있음

---

## 4. 접근성 가이드라인

### 로그인 폼 접근성 체크리스트

| 항목 | 구현 방법 | WCAG 기준 |
|---|---|---|
| **레이블 연결** | `<label for="...">` 사용하여 입력 필드와 레이블 연결 | 1.3.1 Info and Relationships (A) |
| **에러 식별** | `aria-invalid="true"`, `aria-describedby` 로 에러 필드 식별 | 3.3.1 Error Identification (A) |
| **에러 제안** | 에러 메시지에 수정 방법 포함 | 3.3.3 Error Suggestion (AA) |
| **실시간 에러 알림** | `role="alert"`, `aria-live="polite"` 사용 | 4.1.3 Status Messages (AA) |
| **키보드 탐색** | Tab 키로 모든 입력 요소 접근 가능 | 2.1.1 Keyboard (A) |
| **포커스 관리** | 에러 시 첫 번째 에러 필드에 포커스 이동 | 2.4.3 Focus Order (A) |
| **포커스 표시** | `focus-visible` 스타일로 현재 포커스 위치 표시 | 2.4.7 Focus Visible (AA) |
| **자동완성** | `autocomplete="username"`, `autocomplete="current-password"` | 1.3.5 Identify Input Purpose (AA) |
| **비밀번호 표시** | 토글 버튼에 `aria-label`, `aria-pressed` 사용 | 4.1.2 Name, Role, Value (A) |
| **로딩 상태** | `aria-busy="true"` 로 제출 중 상태 전달 | 4.1.3 Status Messages (AA) |
