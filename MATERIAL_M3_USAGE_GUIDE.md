# Material 3 / Material Web 적용 가이드

이 문서는 `USDT 안전거래 정책 분석` 정적 페이지에서 Material Design 3, Material Web, Material Symbols를 가져다 쓰는 규칙을 정리한 상세 사용 가이드다. 목적은 “예쁜 버튼을 한 번 테스트하는 것”이 아니라, 정책 분석 문서와 이후 모바일 전환 화면에서 같은 스타일 규칙을 반복 적용할 수 있게 하는 것이다.

## 기준 소스

- Material Design 3: https://m3.material.io/
- M3 Web 개발 가이드: https://m3.material.io/develop/web
- Material Web: https://material-web.dev/
- Material Web Quick Start: https://material-web.dev/about/quick-start/
- Material Web Components: https://material-web.dev/components/
- Material Symbols: https://m3.material.io/styles/icons
- Material Symbols 적용 가이드: https://m3.material.io/styles/icons/applying-icons

## 현재 프로젝트 적용 상태

현재 프로젝트는 빌드 도구 없이 `index.html`, `styles.css`, `app.js`만으로 동작하는 정적 페이지다. 따라서 npm 패키지를 직접 import하는 방식보다, 브라우저가 바로 읽을 수 있는 번들 파일을 `vendor/`에 보관하는 방식을 우선 사용한다.

```text
index.html
styles.css
app.js
vendor/
  mermaid.min.js
  lucide.min.js
  material-web/
    all.bundle.mjs
  material-symbols/
    material-symbols-rounded.css
    material-symbols-rounded.ttf
```

`index.html`에는 다음 리소스가 연결되어 있다.

```html
<link rel="stylesheet" href="./vendor/material-symbols/material-symbols-rounded.css" />
<script type="module" src="./vendor/material-web/all.bundle.mjs"></script>
```

`styles.css`에는 우리 문서용 M3 토큰과 Material Web 시스템 토큰이 함께 선언되어 있다.

```css
:root {
  --m3-primary: #0b3b7d;
  --m3-primary-container: #d7e3ff;
  --m3-secondary: #25685e;
  --m3-surface-container: #f1f4f6;
  --m3-radius-full: 999px;

  --md-sys-color-primary: var(--m3-primary);
  --md-sys-color-primary-container: var(--m3-primary-container);
  --md-sys-color-secondary: var(--m3-secondary);
  --md-sys-color-surface-container: var(--m3-surface-container);
  --md-sys-shape-corner-full: var(--m3-radius-full);
}
```

## 어떤 라이브러리를 언제 쓰는가

| 범위 | 사용 목적 | 프로젝트 적용 방식 |
| --- | --- | --- |
| Material Design 3 | 색상, 타이포그래피, shape, motion, 컴포넌트 의미를 정하는 원칙 | `styles.css`의 `--m3-*`, `--md-sys-*` 토큰으로 반영 |
| Material Web | 버튼, FAB, 체크박스, 탭 등 실제 웹 컴포넌트 | `vendor/material-web/all.bundle.mjs`를 로드하고 `<md-*>` 태그 사용 |
| Material Symbols | 공식 아이콘 세트 | `vendor/material-symbols/material-symbols-rounded.css`와 `<span class="material-symbols-rounded">icon_name</span>` 사용 |
| Lucide | 기존 문서 내부 아이콘 유지 | 기존 `icon("...")` 함수 사용. 신규 M3 테스트 영역과 좌측 메뉴는 Material Symbols 우선 |
| Mermaid | 시퀀스 다이어그램 렌더링 | Material과 무관하게 기존 유지 |

## Material Web 컴포넌트 사용 범위

Material Web 홈페이지에서 안내하는 주요 컴포넌트는 아래 목적별로 사용할 수 있다.

| 컴포넌트 | 태그 예시 | 목적 | 이 문서에서의 권장 사용처 |
| --- | --- | --- | --- |
| Buttons | `<md-filled-button>`, `<md-outlined-button>` | 주요/보조 액션 | 매칭 확정, ARS 동의, 증빙 등록, 상세 보기 |
| FAB | `<md-fab>` | 화면의 대표 단일 액션 | 운영자 메시지 열기, 새 문의 작성, 모바일 하단 주요 액션 |
| Icon Buttons | `<md-icon-button>` | 아이콘만 있는 좁은 액션 | 검색 이동, 닫기, 새로고침, 첨부, 설정 |
| Checkbox | `<md-checkbox>` | 복수 선택 또는 동의 체크 | 약관 항목 확인, 이체확인증 제출 전 확인 체크 |
| Radio | `<md-radio>` | 단일 선택 | 매수/매도 역할 선택, 상태 필터 하나만 선택 |
| Switch | `<md-switch>` | 켜기/끄기 설정 | 알림 수신, 운영자 자동 배정, 보안 옵션 |
| Tabs | `<md-tabs>`, `<md-primary-tab>` | 같은 문맥의 보기 전환 | 고객/운영자, 매수자/매도자 채널 전환 |
| Text field | `<md-outlined-text-field>` | 입력 | 검색, 메시지 작성, 증빙 메모 |
| Select | `<md-outlined-select>` | 옵션 선택 | 상태, 거래금액대, 분쟁 유형, 담당자 필터 |
| Menu | `<md-menu>` | 임시 옵션 목록 | 더보기, 정렬, 빠른 상태 변경 |
| Dialog | `<md-dialog>` | 강한 확인/경고 | 매칭확정, 취소불가, 분쟁전환 확인 |
| Lists | `<md-list>`, `<md-list-item>` | 목록형 정보 | 공지, 정책 리스트, 메시지 목록 |
| Chips | `<md-assist-chip>`, `<md-filter-chip>` | 상태/필터/짧은 액션 | 거래 상태, 역할 필터, 위험 배지 |
| Sliders | `<md-slider>` | 연속값 조정 | 수수료율/프리미엄율 시뮬레이션 도구가 생길 때 |
| Progress indicators | `<md-linear-progress>`, `<md-circular-progress>` | 진행/대기 표시 | 자동검증 대기, 업로드 중, 승인검토 중 |
| Ripple | `<md-ripple>` | 커스텀 컴포넌트 터치 피드백 | 직접 만든 카드/목록 행에 Material 터치감을 줄 때 |

Material Web에 아직 없는 컴포넌트나 로드 부담이 큰 컴포넌트는 M3 디자인 원칙만 참고하고, 직접 CSS로 구현한다. 예: 현재의 좌측 문서 내비게이션, 정책 카드, 표, 모바일 참고 이미지 카드.

## 가져다 쓰는 기본 규칙

1. 기존 화면을 전부 Material Web으로 교체하지 않는다.
2. 사용자가 누르는 액션성 요소부터 적용한다.
3. 표, 정책 카드, 긴 문서 본문은 기존 HTML/CSS 구조를 유지한다.
4. Material Web 컴포넌트는 폼, 버튼, 탭, 대화상자처럼 접근성과 상태가 중요한 곳에 우선 적용한다.
5. 아이콘은 신규 UI에서 Material Symbols를 우선 사용하고, 기존 Lucide 아이콘은 단계적으로 유지/교체한다.
6. 색상, radius, motion은 `styles.css`의 토큰을 통해 통제한다.
7. 같은 목적의 컴포넌트는 같은 variant를 반복 사용한다.

## 버튼 선택 규칙

| 상황 | 권장 컴포넌트 | 예시 |
| --- | --- | --- |
| 한 화면에서 가장 중요한 확정 액션 | `<md-filled-button>` | 매칭 확정, ARS 동의 완료 |
| 중요하지만 최종 확정은 아닌 액션 | `<md-filled-tonal-button>` | 운영자 메시지 열기, 상태 확인 |
| 보조 액션 | `<md-outlined-button>` | 증빙 등록, 다시 조회 |
| 낮은 강조의 이동/상세보기 | `<md-text-button>` | 상세 보기, 원문 보기 |
| 모바일에서 대표 단일 액션 | `<md-fab>` | 운영자 메시지, 문의 작성 |
| 공간이 좁고 의미가 명확한 액션 | `<md-icon-button>` | 닫기, 새로고침, 검색 이동 |

예시:

```html
<md-filled-button>
  <md-icon slot="icon">check_circle</md-icon>
  매칭 확정
</md-filled-button>

<md-filled-tonal-button>
  <md-icon slot="icon">phone_in_talk</md-icon>
  ARS 동의
</md-filled-tonal-button>

<md-outlined-button>
  <md-icon slot="icon">upload_file</md-icon>
  증빙 등록
</md-outlined-button>
```

## 아이콘 사용 규칙

Material Symbols는 아이콘 이름을 텍스트로 넣는 방식이다.

```html
<span class="material-symbols-rounded" aria-hidden="true">support_agent</span>
```

Material Web 컴포넌트 내부에서는 `md-icon`을 우선 사용한다.

```html
<md-icon-button aria-label="운영자 메시지">
  <md-icon>support_agent</md-icon>
</md-icon-button>
```

아이콘 선택 기준:

| 의미 | 권장 아이콘 |
| --- | --- |
| 전체 시퀀스 | `route` |
| 단계/체크리스트 | `checklist` |
| 상태 흐름 | `account_tree` |
| 버튼 정책 | `ads_click` |
| 메시지 | `forum` |
| 운영자 메시지 | `support_agent` |
| 모바일 | `phone_iphone` |
| 모니터링 | `radar` |
| 취소/차단 | `block` |
| 보안 | `verified_user` |
| 예외/주의 | `warning` |
| 정산 | `receipt_long` |
| 운영 관리 | `fact_check` |
| 정책문서 | `menu_book` |
| 개발/데이터 | `database` |

아이콘만 있는 버튼에는 반드시 `aria-label`을 넣는다. 텍스트가 함께 있는 버튼의 아이콘은 `aria-hidden="true"`로 처리한다.

## 토큰 사용 규칙

우리 프로젝트는 두 종류의 토큰을 같이 쓴다.

- `--m3-*`: 사람이 읽고 조정하기 쉬운 프로젝트 전용 토큰
- `--md-sys-*`: Material Web 컴포넌트가 읽는 공식 시스템 토큰

색상을 직접 반복해서 쓰지 말고, 아래처럼 토큰을 통해 연결한다.

```css
.operator-message-button {
  color: var(--m3-on-primary);
  background: var(--m3-primary);
  border-radius: var(--m3-radius-full);
}
```

Material Web 컴포넌트는 컴포넌트별 토큰으로 조정한다.

```css
.m3-component-shelf md-filled-button {
  --md-filled-button-container-shape: var(--m3-radius-full);
  --md-filled-button-label-text-weight: 700;
}
```

## 적용 위치별 가이드

### 좌측 메뉴

- 현재 좌측 메뉴는 Material Symbols 아이콘과 M3 토큰 기반 hover/active 스타일을 사용한다.
- 메뉴 hover는 `translateX(2px)`, 아이콘 scale, 은은한 shadow 정도만 허용한다.
- 문서 탐색 메뉴이므로 과한 애니메이션, 큰 색상 변화, 레이아웃 이동은 금지한다.

### 모바일 UIUX

- 모바일 화면은 하단 네비게이션, 탭 라인, 바텀시트, FAB 사용을 우선 검토한다.
- FAB는 “운영자 메시지”, “문의 작성”, “증빙 제출”처럼 화면 맥락에서 하나의 대표 액션일 때만 사용한다.
- 긴 정책 설명은 바텀시트 또는 상세 진입으로 보내고, 첫 화면에는 상태와 다음 액션만 남긴다.

### 운영자 메시지

- 고객 화면: `md-filled-tonal-button` 또는 확장 FAB로 진입한다.
- 관리자 화면: 매칭 상세 상단에는 `md-icon-button`, 메시지 패널 안에서는 탭/리스트/텍스트필드를 사용할 수 있다.
- 고객 간 직접 소통을 막는 정책이 핵심이므로, UI 문구는 “상대방 메시지”가 아니라 “운영자 메시지”로 고정한다.

### ARS/PIN/매칭확정

- ARS 동의: `md-filled-tonal-button`
- PIN 입력: `md-outlined-text-field`
- 최종 매칭확정: `md-filled-button`
- 취소불가 확인: `md-dialog`

### 증빙/정산/검토

- 파일 등록: `md-outlined-button` + `upload_file`
- 검토 상태: chips 또는 기존 status chip
- 자동검증 대기: `md-linear-progress`
- 수수료율/프리미엄율 조정 시뮬레이션: `md-slider`

## 정적 페이지에서 새 컴포넌트 추가 절차

1. Material Web 문서에서 해당 컴포넌트 태그와 import 경로를 확인한다.
2. 현재처럼 전체 번들 `vendor/material-web/all.bundle.mjs`에 포함되어 있는지 브라우저에서 확인한다.
3. HTML 또는 `app.js` 렌더 함수에 `<md-*>` 태그를 추가한다.
4. `styles.css`에서 필요한 시스템 토큰 또는 컴포넌트 토큰만 조정한다.
5. `index.html`의 cache bust query를 올린다.
6. `node --check app.js`를 실행한다.
7. 로컬 서버에서 해당 컴포넌트가 실제 너비/높이를 갖고 렌더링되는지 확인한다.

확인 예시:

```js
[
  "md-filled-button",
  "md-fab",
  "md-icon"
].map((name) => ({
  name,
  count: document.querySelectorAll(name).length
}));
```

## npm/빌드 기반으로 전환할 때

현재는 정적 페이지라 로컬 번들을 쓴다. 프론트엔드 빌드 환경이 생기면 공식 Quick Start 방식으로 전환한다.

```bash
npm install @material/web
```

엔트리 파일:

```js
import "@material/web/button/filled-button.js";
import "@material/web/button/outlined-button.js";
import "@material/web/fab/fab.js";
import "@material/web/icon/icon.js";
```

HTML:

```html
<script type="module" src="./index.js"></script>
```

운영 배포에서는 bare module specifier를 해석할 수 있도록 Rollup, Vite 같은 번들러를 사용한다.

## 업데이트 규칙

1. Material Web 버전을 올릴 때는 `vendor/material-web/all.bundle.mjs`를 새 버전으로 교체한다.
2. Material Symbols 폰트를 교체할 때는 `vendor/material-symbols/material-symbols-rounded.ttf`와 CSS를 함께 확인한다.
3. 외부 CDN을 직접 참조하지 않는다. 내부망/오프라인 배포를 위해 항상 `vendor/`에 보관한다.
4. 새 파일을 추가하면 `OFFLINE_DEPLOY.md` 또는 배포 체크리스트에 포함한다.
5. GitHub Pages 반영 시 cache bust query를 반드시 변경한다.

## 금지/주의 사항

- 문서 본문 전체를 Material 컴포넌트로 감싸지 않는다.
- 버튼처럼 보이는 div를 만들지 않는다. 실제 버튼은 `<button>` 또는 `<md-*>` 버튼을 사용한다.
- 아이콘만으로 의미가 불명확한 액션을 만들지 않는다.
- Material Web에 없는 컴포넌트를 억지로 비슷한 태그명으로 만들지 않는다.
- 외부 URL의 폰트나 스크립트를 그대로 배포하지 않는다.
- 상태/정책 문구보다 시각 효과가 앞서지 않게 한다.

## 빠른 예시

운영자 메시지 진입 버튼:

```html
<md-filled-tonal-button>
  <md-icon slot="icon">support_agent</md-icon>
  운영자 메시지
</md-filled-tonal-button>
```

모바일 대표 액션:

```html
<md-fab label="운영자 메시지">
  <md-icon slot="icon">forum</md-icon>
</md-fab>
```

검색 필드:

```html
<md-outlined-text-field placeholder="본문 단어, 문구, 상태 검색">
  <md-icon slot="leading-icon">search</md-icon>
</md-outlined-text-field>
```

확인 체크:

```html
<label class="confirm-row">
  <md-checkbox touch-target="wrapper"></md-checkbox>
  본인명의 계좌에서 이체했음을 확인합니다.
</label>
```

상태 전환 탭:

```html
<md-tabs>
  <md-primary-tab active>매수자</md-primary-tab>
  <md-primary-tab>매도자</md-primary-tab>
  <md-primary-tab>운영자</md-primary-tab>
</md-tabs>
```
