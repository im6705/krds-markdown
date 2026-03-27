---
title: "타이포그래피 (Typography)"
source_pages: "102-125"
---

# 타이포그래피 (Typography)

타이포그래피는 정보를 효과적으로 전달하고 일관된 사용자 경험을 제공하는 데 필수적인 요소이다. 글꼴, 크기, 두께, 간격, 계층 구조를 정의하여 가독성을 높이고, 내용을 중요도에 따라 시각적으로 표현한다.

---

## 1. 서체 (Typeface)

### 표준형 스타일
- **Pretendard GOV** 서체를 국문/영문 모두 기본 사용
- Pretendard를 기반으로 공공기관 접근성/가독성에 초점

### 확장형 스타일
- **가독성이 높은 고딕 계열 서체** 권장
- 자체 개발 폰트가 고딕 계열이 아니거나 가독성이 낮으면 디스플레이/배너에서만 제한 사용
- 추천 고딕 계열 폰트: **노토 산스, 나눔 고딕, 스포카 한 산스**

---

## 2. 글자 두께 (Font Weight)

표준형 스타일은 **regular(400)**와 **bold(700)** 두 가지 두께를 사용한다.

| 두께 | font-weight 값 | 용도 |
|---|---|---|
| **Regular** | 400 | 본문 텍스트, 기본 가독성 보장 |
| **Bold** | 700 | 강조가 필요한 제목, 중요한 내용 |

### 이상적인 글자 두께 권장
- 이상적인 텍스트 두께: **regular(400), medium(500), bold(700)**
- 두께 사용은 **최대 4가지** 정도로 제한
- light/thin: 배경 구분 어려움
- extra bold: 시각적 피로 유발

---

## 3. 줄 간격 (Line Height)

- **최소 150% 이상**으로 설정
- 시각장애/난독증 사용자의 읽기 편의 향상
- px 단위가 아닌 **em 또는 %** 등 상대적 단위 사용

---

## 4. 기본 사이즈

- 본문 폰트 크기: **최소 16px 이상**
- 표준형 스타일 (Pretendard GOV): **17px**을 기본 크기로 사용 (다른 서체에 비해 상대적으로 작으므로)

### Figma Vertical Trim
- 표준형 스타일은 **"Standard"**로 설정
- Standard는 CSS의 line-height와 구조적으로 유사하여 레이아웃 오류 감소

---

## 5. 표현 단위 (rem)

개발 시 **rem 단위**로 변환해 사용한다. rem 기본값은 10px 또는 62.5%로 설정.

| % 기준 | px 기준 |
|---|---|
| 100% | 16px (시스템 기본) |
| **62.5%** | **10px** (권장 기본값) |
| 50% | 8px |

---

## 6. 글자 스케일 (Type Scale)

display, heading, body 구조를 사용하며, heading과 body 간 글자 크기 차이는 **1.25~1.5배** 권장.

### Display

배너 등 마케팅 용도. 가장 큰 텍스트.

| Style | Size (PC) | Size (Mobile) | Font Weight | Line Height | Letter Spacing |
|---|---|---|---|---|---|
| large | 60px | 44px | 700 | 150% | 1px |
| medium | 44px | 32px | 700 | 150% | 1px |
| small | 36px | 28px | 700 | 150% | 1px |

### Heading

페이지/모듈 타이틀. h1~h5 계층 설정.

| Style | Size (PC) | Size (Mobile) | Font Weight | Line Height | Letter Spacing |
|---|---|---|---|---|---|
| xlarge | 40px | 28px | 700 | 150% | 1px |
| large | 32px | 24px | 700 | 150% | 1px |
| medium | 24px | 22px | 700 | 150% | 0px |
| small | 19px | 19px | 700 | 150% | 0px |
| xsmall | 17px | 17px | 700 | 150% | 0px |
| xxsmall | 15px | 15px | 700 | 150% | 0px |

### Heading 계층별 사용

| 구조 | 범위 | 사용 |
|---|---|---|
| h1 | xlarge-large | 페이지/섹션의 가장 중요한 제목 |
| h2 | large-medium | 주요 섹션 구분, 부제목 역할 |
| h3 | medium-small | 세부 섹션 제목 |
| h4 | small-xsmall | 본문 비슷한 크기의 제목, 세부 콘텐츠 설명 |
| h5 | xsmall-xxsmall | 가장 작은 제목, 부차적 정보/보조 설명 |

### Body

본문과 기본 콘텐츠에 적용.

| Style | Size (PC) | Size (Mobile) | Font Weight | Line Height | Letter Spacing |
|---|---|---|---|---|---|
| large-bold | 19px | 28px | 700 | 150% | 0px |
| medium-bold | 17px | 24px | 700 | 150% | 0px |
| small-bold | 15px | 22px | 700 | 150% | 0px |
| xsmall-bold | 13px | 19px | 700 | 150% | 0px |
| large | 19px | 19px | 400 | 150% | 0px |
| **medium*** | **17px** | **17px** | **400** | **150%** | **0px** |
| small | 15px | 15px | 400 | 150% | 0px |
| xsmall | 13px | 13px | 400 | 150% | 0px |

> *medium은 기본 본문 크기

### Body 계층별 사용

| 범위 | 사용 |
|---|---|
| large | 써머리 또는 중요한 정보 전달 |
| medium | 본문 표준 크기, 전체적인 UI 요소 |
| small | 덜 중요한 정보, 부가적인 설명 (가독성 주의) |
| xsmall | 주석, 보조 설명, 부가 정보 (가독성 떨어질 수 있으므로 신중하게 사용) |

---

## 7. 특정 역할의 시멘틱 토큰

### Navigation

사이트 내 이정표 역할 컴포넌트 (Header, Main menu, Side menu, In page navigation)에서 사용.

| Style | Size (PC) | Size (Mobile) | Font Weight | Line Height | Letter Spacing |
|---|---|---|---|---|---|
| title-large | 24px | 22px | 700 | 150% | 0px |
| title-small | 19px | 17px | 700 | 150% | 0px |
| depth-medium-bold | 17px | 17px | 700 | 150% | 0px |
| depth-medium | 17px | 17px | 400 | 150% | 0px |
| depth-small-bold | 15px | 15px | 700 | 150% | 0px |
| depth-small | 15px | 15px | 400 | 150% | 0px |

### Label

컴포넌트 내 label, placeholder 등 (Button, Input, Checkbox, Radio button)에서 사용.

| Style | Size (PC) | Size (Mobile) | Font Weight | Line Height | Letter Spacing |
|---|---|---|---|---|---|
| large | 19px | 19px | 400 | 150% | 0px |
| medium | 17px | 17px | 400 | 150% | 0px |
| small | 15px | 15px | 400 | 150% | 0px |
| xsmall | 13px | 13px | 400 | 150% | 0px |

---

## 8. 접근성 (WCAG 명도 대비)

### 기본 모드

| 등급 | 큰 텍스트 | 일반 텍스트 | 대비율 |
|---|---|---|---|
| **AA** (최소) | 3:1 (매직넘버 40) | 4.5:1 (매직넘버 50) | 최소 |
| **AAA** (강화) | 4.5:1 (매직넘버 50) | 7:1 (매직넘버 70) | 강화 |

### 텍스트 크기 기준

| 구분 | pt 단위 | px 단위 |
|---|---|---|
| **큰 텍스트** | 18pt 이상 또는 14pt 이상 + 굵게 | 24px 이상 또는 18.66px 이상 + 굵게 |
| **일반 텍스트** | 18pt 이하 또는 14pt 미만 | 24px 이하 또는 18.66px 미만 |

> 단위 변환: **1pt = 1.333px** (14pt = 18.66px, 18pt = 24px)

### 선명한 화면 모드 명도 대비

| 요소 | 대비 비율 |
|---|---|
| 본문 텍스트 | **15:1** |
| 헤딩, 레이블 등 텍스트와 아이콘 | **7:1** |
| 시각적 보조 수단 | **4.5:1** |

---

## 9. 사용 가이드 (모범 사례 / 피해야 할 사례)

### 줄 간격
- **모범 사례**: 150% 이상 설정
- **피해야 할 사례**: 줄 간격이 좁아 시각적 피로 유발

### 타이틀-본문 구분
- **모범 사례**: 타이틀 글자 크기를 크게 하거나, 같은 크기일 경우 두께로 구분
- **피해야 할 사례**: 타이틀과 본문의 시각적 위계가 구분되지 않음

### 밑줄
- **모범 사례**: 밑줄은 텍스트 링크에만 사용. 강조는 색상이나 굵기로 표현
- **피해야 할 사례**: 강조 부분에 밑줄 사용

### 자체 개발 폰트
- **모범 사례**: 고딕 계열이 아닌 폰트는 디스플레이/배너에만 사용
- **피해야 할 사례**: 비고딕 계열 폰트를 본문에 사용
