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

- `index.html` — 메인 포트폴리오, **영문판** (Hero → About → Services → Philosophy → Biography → Foundations → FAQ → Contact → SNS). 도메인 루트(`ginakim.org/`)에 대응.
- `ko/index.html` — 메인 포트폴리오, **한글판**. `ginakim.org/ko/`에 대응. `index.html`과 별개의 정적 파일 — 아래 "이중 언어 구조" 참고.
- `ongo-course.html` — "The Ongo Book 2.0" 온라인 강좌 랜딩 페이지, **한글판**(기본/루트). `ginakim.org/ongo-course.html`. **자체 `<style>` 블록 포함** (외부 CSS 없음).
- `en/ongo-course.html` — 같은 강좌 페이지의 **영문판**. `ginakim.org/en/ongo-course.html`.
- `css/style.css` — `index.html`/`ko/index.html` 전용 스타일 전체
- `images/` — 모든 페이지가 공통으로 사용하는 사진 (`favicon.svg` 포함)
- `docs/` — 언어별 PDF 등 다운로드 자료 (`NVC-Education-Program-Proposal-Eng.pdf` / `-Kor.pdf`)
- `robots.txt`, `sitemap.xml` — 검색엔진 크롤링/색인 설정. 4개 URL(`/`, `/ko/`, `/ongo-course.html`, `/en/ongo-course.html`) 모두 등재되어 있고 서로 `hreflang`으로 연결됨. 새 페이지를 추가하면 `sitemap.xml`에도 `<url>` 항목을 추가하세요.

## 이중 언어 구조 (EN/KO) — 언어별 정적 파일 분리

**과거에는 `data-en`/`data-ko` 속성 쌍 + JS `setLang()` 토글로 한 URL에서 언어를 전환했지만, 지금은 언어별로 완전히 분리된 정적 HTML 파일입니다.** AI/검색 크롤러 대부분이 JS를 실행하지 않아 토글 방식은 기본 언어 외 콘텐츠가 색인되지 않는 문제가 있었기 때문입니다. `data-en`/`data-ko` 속성과 `setLang()` 함수는 두 페이지 모두에서 완전히 제거되었습니다.

**쌍을 이루는 파일:**
- `index.html` (EN, 루트) ↔ `ko/index.html` (KO)
- `ongo-course.html` (KO, 루트) ↔ `en/ongo-course.html` (EN)

각 쌍은 루트 경로 쪽이 "원래 기본 언어"(기존 링크/북마크 보존 목적)이고, 반대 언어는 자기 언어 이름의 하위 폴더(`/ko/`, `/en/`)에 있습니다. 두 페이지는 서로 다른 정적 파일일 뿐 상태를 공유하지 않습니다.

**⚠️ 텍스트를 수정할 때 가장 중요한 점**: 이제 "원본 하나 고치면 양쪽에 반영"되는 구조가 아닙니다. 콘텐츠를 고치려면 **같은 내용을 담고 있는 짝 파일 두 곳을 모두 직접 찾아 고쳐야** 합니다(예: `index.html`의 서비스 설명을 고치면 `ko/index.html`의 대응 문단도 번역해서 고쳐야 함). 한쪽만 고치면 두 언어 버전이 어긋납니다.

**경로 규칙**: 루트에 있는 `index.html`/`ongo-course.html`은 기존처럼 상대경로(`css/style.css`, `images/...`, `docs/...`)를 씁니다. 반면 하위 폴더에 있는 `ko/index.html`, `en/ongo-course.html`은 한 단계 더 깊은 위치이므로 **모든 자산 경로가 절대경로**(`/css/style.css`, `/images/...`, `/docs/...`)로 되어 있습니다. 새 자산을 추가하거나 경로를 옮길 때 이 규칙을 지켜야 합니다.

**언어 전환 링크**: 각 파일의 nav(`.nav-lang` 또는 강좌 페이지 상단 언어 토글)에는 버튼이 아니라 실제 `<a href="...">` 링크(현재 보고 있는 언어는 `<span class="lang-btn active">`로 비활성 표시)로 상대 언어 파일을 가리킵니다. 크롤러도 따라갈 수 있는 진짜 링크여야 하므로 JS `onclick`으로 되돌리지 마세요.

**교차 링크(4개 파일 모두 언어에 맞게 고정되어 있음)**:
- `index.html`의 "NVC Education Program Proposal" 버튼 → `docs/NVC-Education-Program-Proposal-Eng.pdf` / `ko/index.html`은 `/docs/...-Kor.pdf`
- `index.html`의 "Online Course" 버튼 → `/en/ongo-course.html` / `ko/index.html`은 `/ongo-course.html`
- Foundations 섹션의 "Libro ONGO 2.0" 링크도 동일한 규칙

**`<head>` 메타데이터**: 4개 파일 모두 각자 언어에 맞는 `<title>`, `meta description`, Open Graph/Twitter 태그, 그리고 서로를 가리키는 `hreflang` 3종(`en`, `ko`, `x-default`)을 갖고 있습니다. `index.html`/`ko/index.html`에는 `Person` + `FAQPage` JSON-LD가 언어별로 각각 번역되어 들어있습니다(`#services`, `#faq` 텍스트를 고칠 때 두 파일의 JSON-LD 4곳 — EN 본문, EN JSON-LD, KO 본문, KO JSON-LD — 모두 갱신 필요).

`#faq` 섹션은 네이티브 `<details>/<summary>` 아코디언(JS 불필요)이며, 질문은 첫 문장에서 바로 답하는 형식(AEO)을 유지합니다.

## 문의 폼

`index.html`/`ko/index.html` 각각의 인라인 스크립트에서 Formspree(`action="https://formspree.io/f/xvzdvlrj"`)로 AJAX fetch 전송합니다. 두 파일 모두 같은 Formspree 엔드포인트를 쓰지만, 제출 중/완료/에러 버튼 문구는 언어별로 하드코딩되어 있습니다(과거처럼 `currentLang` 변수로 분기하지 않음). 문구를 고칠 때 두 파일 모두 확인하세요. `ongo-course.html`/`en/ongo-course.html`의 수강신청 폼도 동일한 패턴(별도 Formspree 엔드포인트 `mlgaozyb`)입니다.

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

`ongo-course.html`과 `en/ongo-course.html`은 (각자 자신의 `<style>` 블록에) 인라인으로 별도 색상 팔레트(`--sage`, `--gold`, `--charcoal` 등)를 정의하며, `style.css`와 값을 공유하지 않습니다. 두 파일은 완전히 독립된 `<style>` 블록을 각각 갖고 있으므로, 디자인(색상·간격·컴포넌트 CSS)을 바꿀 때는 두 파일 모두 수정해야 합니다.

## Biography 섹션 (수직 타임라인)

`#biography`는 `.timeline` 안에 `.timeline-item`들을 나열한 수직 타임라인입니다. `nth-child(odd/even)`으로 좌우 교차 배치되며(모바일에서는 단일 열로 축소), 각 항목은 `.timeline-marker`(연도 라벨 + 점)와 `.timeline-card`(제목 + 본문, 선택적으로 `.timeline-photo`)로 구성됩니다.

사진은 `.timeline-photo`에 붙는 tone 클래스로 시간의 흐름을 표현합니다: `tone-distant`(과거, 흑백/세피아) → `tone-muted`(중간, 채도 낮춤) → `tone-vivid`(현재, 채도 강조). 크기는 `size-sm` / `size-md` / `size-lg` / `size-wide`(가로 사진, 3:2)로 조절합니다. 새 타임라인 항목을 추가할 때 이 패턴(연도, 톤, 크기)을 유지하세요.

한 항목에 여러 장을 넣을 때:
- 사진 2장: `.timeline-photo-group`(세로 스택, 320px 폭). 파노라마처럼 매우 넓은 사진은 `aspect-pano`(3:1)를 추가로 붙입니다.
- 사진 3장: `.timeline-photo-trio`(320px 폭, 2열 그리드). 대표 사진에 `photo-main`(3:2, 상단 전체 폭), 나머지 두 장에 `photo-sub`(4:3, 하단 2열)를 붙입니다. About 섹션의 `about-images` 그리드와 같은 패턴입니다.

원본 사진은 `images/`에 대용량으로 들어오는 경우가 많으므로(수 MB, 큰 해상도), 웹에 올리기 전 폭 1200px 내외로 리사이즈하고 JPEG 품질 85 정도로 압축한 뒤 원본은 삭제합니다.

## Foundations 섹션 (탭 UI)

`#foundations`는 Biography와 Contact 사이에 위치하며, 로고테라피와 NVC(비폭력대화)를 소개하는 탭 전환 섹션입니다. `.foundations-tab-btn` 버튼 클릭 시 인라인 `setFoundationsTab(tab)` 함수가 `data-tab`/`data-panel` 값이 일치하는 `.foundations-panel`에 `.active` 클래스를 토글합니다(언어 전환 `setLang`과 별개의 독립적인 토글 로직). 새 탭을 추가하려면 버튼과 패널 모두에 동일한 `data-tab`/`data-panel` 값을 부여해야 합니다.

각 패널은 `.foundations-grid`(본문 2단: 좌측 `.foundations-intro` 서술+인용구, 우측 `.foundations-principles` 핵심 개념 카드)와 하단 `.foundations-footer`(자격/활동 내역, `.credential-block` 또는 `.activity-list`)로 구성됩니다. 패널 제목(`.foundations-name`)도 다른 텍스트와 마찬가지로 `data-en`/`data-ko` 쌍이 필요합니다(자칫 누락하기 쉬움).

창시자 사진은 `.foundations-intro` 첫 자식으로 들어가는 `<figure class="foundations-photo-frame">`(직사각형, 좌측 float)로 표시되며, 본문 첫 단락을 감싸듯 배치됩니다(데스크톱). 모바일(`≤768px`)에서는 float이 해제되고 사진이 중앙 정렬로 쌓입니다. 빅터 프랭클 사진(`images/Viktor_Frankl2.jpg`)은 Wikimedia Commons의 CC BY-SA 3.0 DE 라이선스 이미지라 `<figcaption class="foundations-photo-credit">`에 출처 표기가 있습니다. 마샬 로젠버그 사진(`images/Marshall-Rosenberg1.jpeg`)은 사용자가 직접 제공한 파일로 출처/라이선스가 확인되지 않아 캡션이 없습니다 — 정확한 출처를 받으면 같은 방식으로 캡션을 추가하세요.

NVC 전파 활동(NVC Outreach) 아래 `.youtube-playlists`는 채널(`@hyunheekim1222`)의 재생목록 3개(NVC 기초, 대화의 코드, 강의 영상)를 소개하는 카드 그리드입니다. 각 `.playlist-card`의 썸네일은 실제 유튜브 재생목록 페이지를 헤드리스 브라우저로 스크린샷 후 크롭한 이미지(`images/yt-playlist-*.jpg`)입니다. 재생목록 내용이 바뀌면(영상 추가/제목 변경) 썸네일도 다시 캡처해서 교체해야 최신 상태가 유지됩니다.

## 반응형 브레이크포인트

`css/style.css`에 정의:
- `≤ 1024px`: 2열 그리드 축소, 히어로 세로 배치
- `≤ 768px`: 내비게이션 링크 숨김, 전체 단일 열 레이아웃
