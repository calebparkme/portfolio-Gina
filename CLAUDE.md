# CLAUDE.md

이 파일은 Claude Code (claude.ai/code)가 이 저장소에서 작업할 때 참고하는 가이드입니다.

## 프로젝트 개요

캐나다 온타리오 밀턴에 거주하는 공인 로고테라피스트이자 NVC 전문가 김현희(Gina Hyunhee Kim)의 개인 포트폴리오 사이트 (ginakim.org). GitHub Pages로 배포되며, `CNAME` 파일이 `ginakim.org` 도메인을 연결합니다.

빌드 과정 없음, 패키지 매니저 없음, 테스트 없음 — 순수 HTML, CSS, 바닐라 JavaScript로 구성됩니다.

## 로컬 개발

브라우저에서 직접 열거나, 경로 문제를 피하려면 로컬 서버를 실행하세요:

```bash
python3 -m http.server 8080
# 또는
npx serve .
```

## 파일 구조

- `index.html` — 메인 단일 페이지 포트폴리오 (Hero → About → Services → Philosophy → Biography → Contact → SNS)
- `ongo-course.html` — "The Ongo Book 2.0" 온라인 강좌 랜딩 페이지; **자체 `<style>` 블록 포함** (외부 CSS 없음)
- `css/style.css` — `index.html` 전용 스타일 전체
- `images/` — 두 페이지에서 공통으로 사용하는 사진

## 이중 언어 시스템 (EN/KO)

`index.html`의 모든 사용자 노출 텍스트는 `data-en` / `data-ko` 속성 쌍으로 작성됩니다:

```html
<span data-en="About" data-ko="소개">About</span>
```

`index.html` 하단 인라인 `<script>`의 `setLang(lang)` 함수가 `[data-en]` 요소를 순회하며 `innerHTML`을 선택한 언어 속성값으로 교체합니다. 텍스트를 수정할 때는 **`data-en`과 `data-ko` 모두 수정**해야 하며, 태그 내부의 기본 텍스트(페이지 로드 시 영어로 표시됨)도 함께 업데이트해야 합니다.

`ongo-course.html`은 한국어 전용이며 언어 전환 기능이 없습니다.

## 문의 폼

`index.html` 인라인 스크립트에서 Formspree(`action="https://formspree.io/f/xvzdvlrj"`)로 AJAX fetch 전송합니다. 폼 관련 변경 시 백엔드 작업은 불필요하며, `action` 속성의 Formspree 엔드포인트만 중요합니다.

## 디자인 시스템

CSS 변수는 `css/style.css`의 `:root`에 정의되어 있습니다. 주요 토큰:

| 변수 | 용도 |
|---|---|
| `--accent` / `--sage` | 인터랙티브 요소의 주요 녹색 계열 |
| `--dark` | 제목, 푸터 배경 |
| `--warm-white` / `--cream` | 섹션 배경 교차 색상 |
| `--mid` | 본문 텍스트 |
| `--font-serif` | Cormorant Garamond — 제목, 인용구, 대형 표시 텍스트 |
| `--font-sans` | Lato + Noto Sans KR — 본문, 레이블, 내비게이션 |

`ongo-course.html`은 인라인으로 별도 색상 팔레트(`--sage`, `--gold`, `--charcoal` 등)를 정의하며, `style.css`와 값을 공유하지 않습니다.

## Biography 섹션 (수직 타임라인)

`#biography`는 `.timeline` 안에 `.timeline-item`들을 나열한 수직 타임라인입니다. `nth-child(odd/even)`으로 좌우 교차 배치되며(모바일에서는 단일 열로 축소), 각 항목은 `.timeline-marker`(연도 라벨 + 점)와 `.timeline-card`(제목 + 본문, 선택적으로 `.timeline-photo`)로 구성됩니다.

사진은 `.timeline-photo`에 붙는 tone 클래스로 시간의 흐름을 표현합니다: `tone-distant`(과거, 흑백/세피아) → `tone-muted`(중간, 채도 낮춤) → `tone-vivid`(현재, 채도 강조). 크기는 `size-sm` / `size-md` / `size-lg` / `size-wide`(가로 사진, 3:2)로 조절합니다. 새 타임라인 항목을 추가할 때 이 패턴(연도, 톤, 크기)을 유지하세요.

한 항목에 여러 장을 넣을 때:
- 사진 2장: `.timeline-photo-group`(세로 스택, 320px 폭). 파노라마처럼 매우 넓은 사진은 `aspect-pano`(3:1)를 추가로 붙입니다.
- 사진 3장: `.timeline-photo-trio`(320px 폭, 2열 그리드). 대표 사진에 `photo-main`(3:2, 상단 전체 폭), 나머지 두 장에 `photo-sub`(4:3, 하단 2열)를 붙입니다. About 섹션의 `about-images` 그리드와 같은 패턴입니다.

원본 사진은 `images/`에 대용량으로 들어오는 경우가 많으므로(수 MB, 큰 해상도), 웹에 올리기 전 폭 1200px 내외로 리사이즈하고 JPEG 품질 85 정도로 압축한 뒤 원본은 삭제합니다.

## 반응형 브레이크포인트

`css/style.css`에 정의:
- `≤ 1024px`: 2열 그리드 축소, 히어로 세로 배치
- `≤ 768px`: 내비게이션 링크 숨김, 전체 단일 열 레이아웃
