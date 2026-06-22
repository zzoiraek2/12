# 내부망 배포 안내

이 화면은 내부망에서 동작하도록 외부 CDN 의존성을 제거했다.

## 필수 배포 파일

아래 파일과 폴더를 같은 구조로 배포한다.

```text
index.html
styles.css
app.js
usdt_safe_trade_sequence_interpretation_spec.md
vendor/
  mermaid.min.js
  lucide.min.js
  material-web/
    all.bundle.mjs
  material-symbols/
    material-symbols-rounded.css
    material-symbols-rounded.ttf
  fonts/
    pretendard/
      pretendard.css
      PretendardVariable.woff2
```

## 원인

기존 화면은 Mermaid와 Lucide를 외부 CDN에서 불러왔다. M3 테스트 스타일을 사용할 때도 Material Web과 Material Symbols를 외부에서 직접 불러오면 내부망에서 실패할 수 있으므로 로컬 `vendor` 파일을 사용한다.

```text
https://cdn.jsdelivr.net/...
https://unpkg.com/...
```

내부망에서는 해당 도메인에 접근할 수 없어 Mermaid 렌더링과 아이콘 렌더링이 실패한다.

## 현재 방식

현재 `index.html`은 로컬 파일을 사용한다.

```html
<script src="./vendor/mermaid.min.js"></script>
<script src="./vendor/lucide.min.js"></script>
<link rel="stylesheet" href="./vendor/material-symbols/material-symbols-rounded.css" />
<script type="module" src="./vendor/material-web/all.bundle.mjs"></script>
```

따라서 `vendor` 폴더만 함께 배포하면 통신이 없는 환경에서도 Mermaid 시퀀스, 기존 Lucide 아이콘, Material Symbols 아이콘, Material Web 컴포넌트, Pretendard 폰트가 렌더링된다.

## 확인 방법

내부망 서버에 올린 뒤 브라우저 개발자 도구 Network 탭에서 다음을 확인한다.

1. `vendor/mermaid.min.js`가 200으로 로드되는지 확인
2. `vendor/lucide.min.js`가 200으로 로드되는지 확인
3. `vendor/material-web/all.bundle.mjs`가 200으로 로드되는지 확인
4. `vendor/material-symbols/material-symbols-rounded.css`와 `.ttf`가 200으로 로드되는지 확인
5. `vendor/fonts/pretendard/pretendard.css`와 `.woff2`가 200으로 로드되는지 확인
6. `cdn.jsdelivr.net`, `unpkg.com`, `fonts.googleapis.com`, `fonts.gstatic.com` 요청이 없는지 확인

Mermaid 파일을 찾지 못하면 화면은 시퀀스 원문을 fallback으로 표시한다.
