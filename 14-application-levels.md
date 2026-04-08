---
title: "적용 수준 전략 (Do/Better/Best)"
source_pages: "36-40, 977-1129"
---

# 적용 수준 전략 (Do/Better/Best)

KRDS 서비스 패턴의 사용성 가이드라인은 3개의 적용 수준(필수/권장/우수)으로 구분된다. 이 문서는 각 수준의 의미, 컴포넌트에 대한 동적 적용 전략, 그리고 구현 아키텍처를 상세히 기술한다.

---

## 1. 적용 수준 정의

| 수준 | 영문 | 설명 | 준수 시 | 미준수 시 | WCAG 대응 |
|---|---|---|---|---|---|
| **필수 (Do)** | Do | 기본적이고 보편적인 사용자 경험 보장 | 기본적인 경험 충족 | 작업 실패 직결, 사용자가 스스로 완료 불가 | 주로 WCAG 2.1 A 수준 |
| **권장 (Better)** | Better | 더 많은 사용자에게 실용적인 경험 제공 | 더 높은 만족도 | 작업 난도 증가, 스스로 해결 불가 | 주로 WCAG 2.1 AA 수준 |
| **우수 (Best)** | Best | 매력적이고 최적의 경험 제공 | 만족도 기하급수적 증가 | 불편/비효율적 절차 경험 | WCAG 2.1 AAA 수준 포함 |

> **주의**: 최상위 수준 준수가 하위 수준 준수를 보장하지 않음. 대부분의 최상위 수준 가이드라인은 하위 수준이 준수된 상태에서 효과를 나타내도록 구성됨.

---

## 2. 적용 수준의 의사결정 기준

### 기관 유형별 권장 적용 수준

| 기관 유형 | 아이덴티티 | 스타일 | 컴포넌트 | 서비스 패턴 최소 수준 |
|---|---|---|---|---|
| **중앙행정기관 (대표)** | 필수 | 필수 | 필수 | **권장 (Better)** |
| **중앙행정기관 (운영서비스)** | 필수 | 필수 | 필수 | **필수 (Do)** |
| **공공기관** | 선택 | 필수 | 필수 | **필수 (Do)** |
| **지방자치단체** | 필수 | 필수 | 필수 | **필수 (Do)** |

### 서비스 특성별 권장 적용 수준

| 서비스 특성 | 권장 최소 수준 | 이유 |
|---|---|---|
| **민원 신청/처리** | 권장 (Better) | 높은 과업 완료율 필수 |
| **정보 제공/열람** | 필수 (Do) | 기본 접근성 보장으로 충분 |
| **복합 신청 (다단계)** | 우수 (Best) | 복잡한 플로에서 최적 경험 필요 |
| **실시간 서비스 (예약 등)** | 우수 (Best) | 시간 제약하의 효율성 중요 |

---

## 3. 동적 적용 수준 전환 아키텍처

### 3.1 설정 기반 접근 (Configuration-Driven)

서비스별로 적용 수준을 설정 파일로 관리하고, 런타임에 해당 수준에 맞는 컴포넌트/기능을 활성화한다.

#### 설정 파일 구조 (JSON)

```json
{
  "service": {
    "id": "welfare-application",
    "name": "복지 급여 신청",
    "type": "submission"
  },
  "complianceLevel": "better",
  "guidelines": {
    "login": {
      "level": "better",
      "features": {
        "do": {
          "consistentLoginPosition": true,
          "loginIconWithLabel": true,
          "directLoginNavigation": true
        },
        "better": {
          "frequentMethodPriority": true,
          "methodClassificationByUser": true,
          "helpAndManualLinks": true,
          "preLoginServiceInfo": true
        },
        "best": {
          "diverseEnvironmentHelp": true,
          "savedLoginMethod": true,
          "contextualGuidance": true
        }
      }
    },
    "application": {
      "level": "better",
      "features": {
        "do": {
          "properListSorting": true,
          "metadataClustering": true,
          "statusBadges": true,
          "officialServiceNames": true,
          "noTitleTruncation": true,
          "externalLinkIndicator": true
        },
        "better": {
          "deadlineInPreview": true,
          "conciseSummaryText": true
        },
        "best": {
          "directActionButton": true
        }
      }
    }
  },
  "accessibility": {
    "wcagLevel": "AA",
    "highContrastSupport": true,
    "screenReaderOptimized": true
  }
}
```

#### JavaScript 설정 로더

```javascript
/**
 * KRDS 적용 수준 관리자
 * 서비스 설정에 따라 Do/Better/Best 수준의 기능을 동적으로 활성화/비활성화한다.
 */
class KRDSComplianceManager {
  static LEVELS = {
    DO: 'do',
    BETTER: 'better',
    BEST: 'best'
  };

  static LEVEL_ORDER = ['do', 'better', 'best'];

  constructor(configUrl) {
    this.config = null;
    this.currentLevel = KRDSComplianceManager.LEVELS.DO;
    this.configUrl = configUrl;
  }

  /** 설정 파일 로드 */
  async loadConfig(configUrl) {
    const url = configUrl || this.configUrl;
    try {
      const response = await fetch(url);
      this.config = await response.json();
      this.currentLevel = this.config.complianceLevel || 'do';
      return this.config;
    } catch (error) {
      console.error('KRDS 설정 로드 실패:', error);
      this.currentLevel = 'do';
      return null;
    }
  }

  /** 현재 수준에서 특정 기능이 활성화되어야 하는지 확인 */
  isFeatureEnabled(pattern, featureName) {
    if (!this.config?.guidelines?.[pattern]) return false;

    const levelIndex = KRDSComplianceManager.LEVEL_ORDER.indexOf(this.currentLevel);

    // 현재 수준 이하의 모든 기능을 활성화
    for (let i = 0; i <= levelIndex; i++) {
      const level = KRDSComplianceManager.LEVEL_ORDER[i];
      const features = this.config.guidelines[pattern]?.features?.[level];
      if (features && features[featureName] === true) {
        return true;
      }
    }
    return false;
  }

  /** 특정 패턴의 현재 수준에서 활성화된 모든 기능 목록 반환 */
  getEnabledFeatures(pattern) {
    if (!this.config?.guidelines?.[pattern]) return [];

    const levelIndex = KRDSComplianceManager.LEVEL_ORDER.indexOf(this.currentLevel);
    const enabled = {};

    for (let i = 0; i <= levelIndex; i++) {
      const level = KRDSComplianceManager.LEVEL_ORDER[i];
      const features = this.config.guidelines[pattern]?.features?.[level];
      if (features) {
        Object.entries(features).forEach(([key, value]) => {
          if (value === true) enabled[key] = { level, enabled: true };
        });
      }
    }
    return enabled;
  }

  /** 적용 수준 동적 변경 */
  setLevel(level) {
    if (!KRDSComplianceManager.LEVEL_ORDER.includes(level)) {
      throw new Error(`유효하지 않은 적용 수준: ${level}`);
    }
    this.currentLevel = level;
    document.documentElement.setAttribute('data-krds-level', level);

    // 수준 변경 이벤트 발행
    window.dispatchEvent(new CustomEvent('krds-level-change', {
      detail: { level, config: this.config }
    }));
  }

  /** 현재 적용 수준 조회 */
  getLevel() {
    return this.currentLevel;
  }

  /** 수준 간 비교 (현재 수준이 target 수준 이상인지 확인) */
  meetsLevel(targetLevel) {
    const currentIndex = KRDSComplianceManager.LEVEL_ORDER.indexOf(this.currentLevel);
    const targetIndex = KRDSComplianceManager.LEVEL_ORDER.indexOf(targetLevel);
    return currentIndex >= targetIndex;
  }

  /** 컴플라이언스 리포트 생성 */
  generateReport(pattern) {
    const guidelines = this.config?.guidelines?.[pattern];
    if (!guidelines) return null;

    const report = {
      pattern,
      currentLevel: this.currentLevel,
      targetLevel: guidelines.level,
      meetsTarget: this.meetsLevel(guidelines.level),
      features: {}
    };

    KRDSComplianceManager.LEVEL_ORDER.forEach(level => {
      const features = guidelines.features?.[level] || {};
      report.features[level] = {
        total: Object.keys(features).length,
        enabled: Object.values(features).filter(v => v === true).length,
        required: level === 'do',
        items: features
      };
    });

    return report;
  }
}
```

### 3.2 CSS 클래스 기반 수준 적용

```css
/* data-krds-level 속성으로 수준별 스타일 분기 */

/* 필수 (Do) - 기본 스타일, 항상 적용 */
.krds-login-link {
  display: flex;
  align-items: center;
  gap: var(--krds-gap-2);
}

.krds-login-link__icon {
  display: inline-flex;
}

.krds-login-link__label {
  font-size: var(--krds-font-size-body);
}

/* 권장 (Better) - 추가 기능 활성화 */
[data-krds-level="better"] .krds-login-method-priority,
[data-krds-level="best"] .krds-login-method-priority {
  display: block;
}

[data-krds-level="do"] .krds-login-method-priority {
  display: none;
}

/* 우수 (Best) - 고급 기능 활성화 */
[data-krds-level="best"] .krds-login-saved-method {
  display: block;
}

[data-krds-level="do"] .krds-login-saved-method,
[data-krds-level="better"] .krds-login-saved-method {
  display: none;
}

/* 도움말 정보 - Better 이상에서만 표시 */
[data-krds-level="better"] .krds-help-panel,
[data-krds-level="best"] .krds-help-panel {
  display: block;
}

[data-krds-level="do"] .krds-help-panel {
  display: none;
}

/* 신청 서비스 - 액션 버튼 바로가기 (Best에서만) */
[data-krds-level="best"] .krds-service-list__direct-action {
  display: inline-flex;
}

[data-krds-level="do"] .krds-service-list__direct-action,
[data-krds-level="better"] .krds-service-list__direct-action {
  display: none;
}
```

### 3.3 컴포넌트에서 수준 기반 조건부 렌더링

#### React 컴포넌트 예시

```jsx
import React, { createContext, useContext, useState, useEffect } from 'react';

// 적용 수준 Context
const ComplianceLevelContext = createContext('do');

/** 적용 수준 Provider */
export function KRDSComplianceProvider({ configUrl, children }) {
  const [level, setLevel] = useState('do');
  const [config, setConfig] = useState(null);

  useEffect(() => {
    fetch(configUrl)
      .then(res => res.json())
      .then(data => {
        setConfig(data);
        setLevel(data.complianceLevel || 'do');
        document.documentElement.setAttribute('data-krds-level', data.complianceLevel || 'do');
      })
      .catch(() => setLevel('do'));
  }, [configUrl]);

  return (
    <ComplianceLevelContext.Provider value={{ level, setLevel, config }}>
      {children}
    </ComplianceLevelContext.Provider>
  );
}

/** 적용 수준 Hook */
export function useComplianceLevel() {
  return useContext(ComplianceLevelContext);
}

/** 적용 수준에 따라 조건부 렌더링하는 컴포넌트 */
export function RequiresLevel({ minimum, children, fallback = null }) {
  const { level } = useComplianceLevel();
  const levels = ['do', 'better', 'best'];
  const currentIndex = levels.indexOf(level);
  const requiredIndex = levels.indexOf(minimum);

  if (currentIndex >= requiredIndex) {
    return <>{children}</>;
  }
  return fallback;
}

/** 로그인 패턴 컴포넌트 - 수준별 기능 분기 */
function LoginPattern() {
  const { level } = useComplianceLevel();

  return (
    <div className="krds-login">
      {/* Do (필수): 일관된 위치의 로그인 링크 */}
      <div className="krds-login__header">
        <a href="/login" className="krds-login-link">
          <UserIcon aria-hidden="true" />
          <span>로그인</span>
        </a>
      </div>

      {/* Better (권장): 빈번하게 사용되는 로그인 방식 우선 표시 */}
      <RequiresLevel minimum="better">
        <div className="krds-login__frequent-methods">
          <h3>자주 사용하는 로그인 방식</h3>
          <LoginMethodList prioritized />
        </div>
      </RequiresLevel>

      {/* Better (권장): 도움말 링크 */}
      <RequiresLevel minimum="better">
        <div className="krds-login__help">
          <a href="/login/help">로그인 도움말</a>
          <a href="/login/manual">이용 매뉴얼</a>
        </div>
      </RequiresLevel>

      {/* Best (우수): 저장된 로그인 방식으로 빠른 접근 */}
      <RequiresLevel minimum="best">
        <SavedLoginMethodPanel />
      </RequiresLevel>

      {/* Best (우수): 다양한 이용 환경 고려 도움말 */}
      <RequiresLevel minimum="best">
        <ContextualHelpPanel />
      </RequiresLevel>
    </div>
  );
}
```

#### Vanilla JavaScript 예시

```javascript
/**
 * 적용 수준에 따른 컴포넌트 초기화
 */
class KRDSComponentInitializer {
  constructor(complianceManager) {
    this.manager = complianceManager;
    this.components = new Map();
  }

  /** 컴포넌트 등록 */
  register(name, { do: doInit, better: betterInit, best: bestInit }) {
    this.components.set(name, { do: doInit, better: betterInit, best: bestInit });
  }

  /** 현재 적용 수준에 따라 모든 컴포넌트 초기화 */
  initializeAll() {
    const levels = ['do', 'better', 'best'];
    const currentIndex = levels.indexOf(this.manager.getLevel());

    this.components.forEach((handlers, name) => {
      for (let i = 0; i <= currentIndex; i++) {
        const handler = handlers[levels[i]];
        if (handler) {
          try {
            handler();
          } catch (e) {
            console.error(`[KRDS] ${name} ${levels[i]} 초기화 실패:`, e);
          }
        }
      }
    });
  }
}

// 사용 예시
const manager = new KRDSComplianceManager('/config/welfare-app.json');
const initializer = new KRDSComponentInitializer(manager);

// 로그인 컴포넌트 등록
initializer.register('login', {
  do: () => {
    // 필수: 일관된 위치에 로그인 링크 배치
    document.querySelector('.krds-login-link').style.display = 'flex';
    // 필수: 아이콘 + 레이블 구성
    document.querySelector('.krds-login-link__icon').style.display = 'inline-flex';
    document.querySelector('.krds-login-link__label').style.display = 'inline';
  },
  better: () => {
    // 권장: 빈번한 로그인 방식 우선 배치
    document.querySelector('.krds-login-method-priority')?.classList.remove('hidden');
    // 권장: 도움말/매뉴얼 링크
    document.querySelector('.krds-login-help')?.classList.remove('hidden');
  },
  best: () => {
    // 우수: 저장된 로그인 방식
    document.querySelector('.krds-login-saved-method')?.classList.remove('hidden');
    // 우수: 맥락적 도움말
    document.querySelector('.krds-contextual-help')?.classList.remove('hidden');
  }
});

// 신청 서비스 컴포넌트 등록
initializer.register('application', {
  do: () => {
    // 필수: 적절한 정렬, 메타데이터, 상태 배지
    document.querySelectorAll('.krds-service-badge').forEach(el => el.style.display = 'inline-flex');
    document.querySelectorAll('.krds-service-metadata').forEach(el => el.style.display = 'flex');
  },
  better: () => {
    // 권장: 신청 가능 기한 미리보기
    document.querySelectorAll('.krds-service-deadline').forEach(el => el.classList.remove('hidden'));
  },
  best: () => {
    // 우수: 상세 화면을 거치지 않는 바로가기 버튼
    document.querySelectorAll('.krds-service-direct-action').forEach(el => el.classList.remove('hidden'));
  }
});

// 설정 로드 후 초기화
manager.loadConfig().then(() => {
  initializer.initializeAll();
});

// 수준 변경 시 재초기화
window.addEventListener('krds-level-change', () => {
  initializer.initializeAll();
});
```

---

## 4. 적용 수준별 가이드라인 예시

### 4.1 로그인 패턴 적용 수준

| # | 가이드라인 | Do | Better | Best |
|---|---|---|---|---|
| 01 | 로그인 링크를 모든 화면에서 일관된 위치에 배치 | **필수** | - | - |
| 02 | 아이콘과 레이블로 로그인 링크 구성 | - | **권장** | - |
| 03 | 로그인 링크는 항상 로그인 화면으로 연결 | **필수** | - | - |
| 04 | 빈번한 로그인 방식에 우선 접근 수단 제공 | - | **권장** | - |
| 05 | 동일 수준 로그인 방식을 탭으로 구분하지 않음 | **필수** | - | - |
| 06 | 사용자 관점의 분류 체계 반영 | - | **권장** | - |
| 07 | 서비스 이용 절차 차이 사전 안내 | **필수** | - | - |
| 08 | 직관적 안내 제공 | **필수** | - | - |
| 09 | 도움말/매뉴얼 링크 제공 | - | **권장** | - |
| 10 | 다양한 이용 환경 고려한 도움말 | - | - | **우수** |

### 4.2 신청 서비스 패턴 적용 수준

| # | 가이드라인 | Do | Better | Best |
|---|---|---|---|---|
| 01 | 서비스 특성에 따른 적절한 목록 정렬 방식 | **필수** | - | - |
| 02 | 목록의 메타데이터 군집화 | **필수** | - | - |
| 03 | 신청 상태 배지 필수 제공 | **필수** | - | - |
| 04 | 미리보기에 신청 가능 기한 정보 | - | **권장** | - |
| 05 | 공식 서비스 명칭 사용 | **필수** | - | - |
| 06 | 제목에 말줄임표 미사용 | **필수** | - | - |
| 07 | 미리보기 텍스트 간결 작성 | - | **권장** | - |
| 08 | 바로가기 액션 버튼 제공 | - | - | **우수** |
| 09 | 외부 링크 시각적 단서 | **필수** | - | - |

---

## 5. 적용 수준 전환을 위한 시스템 설정

### 서버사이드 설정 관리

```javascript
// Express.js 미들웨어 예시
function krdsComplianceMiddleware(req, res, next) {
  // 기관별 설정 조회
  const orgConfig = getOrganizationConfig(req.organizationId);

  // 서비스별 적용 수준 결정
  const serviceType = req.params.serviceType;
  const complianceLevel = determineComplianceLevel(orgConfig, serviceType);

  // 요청 컨텍스트에 적용 수준 설정
  req.krdsLevel = complianceLevel;

  // HTML 응답에 data 속성 주입
  res.locals.krdsLevel = complianceLevel;
  next();
}

function determineComplianceLevel(orgConfig, serviceType) {
  // 기관 유형 기반 기본 수준
  const orgLevel = orgConfig.defaultLevel || 'do';

  // 서비스 유형에 따른 수준 오버라이드
  const serviceOverrides = {
    'submission': 'better',       // 민원 신청: 최소 Better
    'inquiry': 'do',              // 조회: Do
    'reservation': 'best',        // 예약: Best 권장
    'complex-submission': 'best'  // 복합 신청: Best 권장
  };

  const serviceLevel = serviceOverrides[serviceType] || orgLevel;

  // 더 높은 수준 적용
  const levels = ['do', 'better', 'best'];
  return levels[Math.max(levels.indexOf(orgLevel), levels.indexOf(serviceLevel))];
}
```

### HTML 템플릿에 적용 수준 반영

```html
<!DOCTYPE html>
<html lang="ko" data-krds-level="<%= krdsLevel %>">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>디지털 정부서비스</title>
  <link rel="stylesheet" href="/css/krds-tokens.css">
  <link rel="stylesheet" href="/css/krds-components.css">
</head>
<body>
  <header class="krds-header">
    <!-- Do 수준: 기본 로그인 링크 (항상 렌더링) -->
    <a href="/login" class="krds-login-link">
      <svg class="krds-login-link__icon" aria-hidden="true">...</svg>
      <span class="krds-login-link__label">로그인</span>
    </a>
  </header>

  <main>
    <!-- 서비스 목록 -->
    <ul class="krds-service-list">
      <li class="krds-service-list__item">
        <h3 class="krds-service-list__title">
          <a href="/service/welfare-001">복지 급여 신청</a>
        </h3>

        <!-- Do 수준: 상태 배지 (필수) -->
        <span class="krds-badge krds-badge--status-open">접수 중</span>

        <!-- Do 수준: 메타데이터 (필수) -->
        <div class="krds-service-list__metadata">
          <span>분야: 복지</span>
          <span>대상: 전체</span>
        </div>

        <!-- Better 수준: 신청 기한 (권장) -->
        <p class="krds-service-list__deadline" data-level="better">
          신청 기한: 2025-12-31
        </p>

        <!-- Best 수준: 바로가기 액션 버튼 (우수) -->
        <button class="krds-service-list__direct-action" data-level="best">
          바로 신청하기
        </button>
      </li>
    </ul>
  </main>

  <script>
    // 적용 수준에 따라 해당 수준 이하의 요소만 표시
    const level = document.documentElement.getAttribute('data-krds-level');
    const levels = ['do', 'better', 'best'];
    const currentIndex = levels.indexOf(level);

    document.querySelectorAll('[data-level]').forEach(el => {
      const elementLevel = el.getAttribute('data-level');
      const elementIndex = levels.indexOf(elementLevel);
      if (elementIndex > currentIndex) {
        el.style.display = 'none';
      }
    });
  </script>
</body>
</html>
```

---

## 6. 컴플라이언스 점검 및 리포트

### 자동화된 컴플라이언스 점검 도구

```javascript
/**
 * KRDS 적용 수준 자동 점검 도구
 * DOM을 분석하여 현재 페이지가 설정된 적용 수준을 충족하는지 확인한다.
 */
class KRDSComplianceChecker {
  constructor(targetLevel = 'do') {
    this.targetLevel = targetLevel;
    this.results = [];
  }

  /** 로그인 패턴 점검 */
  checkLoginPattern() {
    const checks = {
      do: [
        {
          name: '로그인 링크 일관된 위치',
          check: () => !!document.querySelector('header .krds-login-link'),
          severity: 'error'
        },
        {
          name: '로그인 링크가 로그인 화면으로 연결',
          check: () => {
            const link = document.querySelector('.krds-login-link');
            return link && (link.href.includes('/login') || link.getAttribute('href') === '/login');
          },
          severity: 'error'
        }
      ],
      better: [
        {
          name: '아이콘과 레이블 구성',
          check: () => {
            const link = document.querySelector('.krds-login-link');
            return link &&
              !!link.querySelector('.krds-login-link__icon') &&
              !!link.querySelector('.krds-login-link__label');
          },
          severity: 'warning'
        },
        {
          name: '도움말 링크 제공',
          check: () => !!document.querySelector('.krds-login-help'),
          severity: 'warning'
        }
      ],
      best: [
        {
          name: '저장된 로그인 방식 제공',
          check: () => !!document.querySelector('.krds-login-saved-method'),
          severity: 'info'
        }
      ]
    };

    return this._runChecks('login', checks);
  }

  /** 점검 실행 */
  _runChecks(pattern, checks) {
    const levels = ['do', 'better', 'best'];
    const targetIndex = levels.indexOf(this.targetLevel);
    const results = [];

    for (let i = 0; i <= targetIndex; i++) {
      const level = levels[i];
      const levelChecks = checks[level] || [];

      levelChecks.forEach(({ name, check, severity }) => {
        const passed = check();
        results.push({
          pattern,
          level,
          name,
          passed,
          severity: passed ? 'pass' : severity
        });
      });
    }

    return results;
  }

  /** 전체 점검 결과 리포트 생성 */
  generateReport() {
    const loginResults = this.checkLoginPattern();
    const allResults = [...loginResults];

    const passed = allResults.filter(r => r.passed).length;
    const total = allResults.length;
    const errors = allResults.filter(r => r.severity === 'error').length;

    return {
      targetLevel: this.targetLevel,
      summary: {
        total,
        passed,
        failed: total - passed,
        errors,
        score: total > 0 ? Math.round((passed / total) * 100) : 0
      },
      details: allResults
    };
  }
}

// 사용 예시
const checker = new KRDSComplianceChecker('better');
const report = checker.generateReport();
console.log(`컴플라이언스 점수: ${report.summary.score}%`);
console.log(`에러: ${report.summary.errors}건`);
```
