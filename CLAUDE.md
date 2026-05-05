# CLAUDE.md — LG전자 임원 실습 1 (HTML 보고서 자동 생성)

> Claude Code가 Cloud Sandbox에서 본 repo를 클론하면 이 파일이 자동 로드됩니다.
> 한 줄로: **한국어·UTF-8·LG Red·Pretendard·임원 톤·인쇄 최적화 — single-shot.**

## §1. 언어·인코딩 (절대 규칙)

- 모든 출력 한국어. 영문 용어는 1차만 영문 병기 후 한국어 사용
- 모든 파일 UTF-8 (BOM 없음)
- HTML: `<meta charset="UTF-8">`, `<html lang="ko">` 강제
- 한자/일본어/중국어 절대 금지 (LG 임원 보고는 한국어 단일)

## §2. Cloud Sandbox 환경 가정

- 작업 디렉토리: 항상 repo 루트 (자동 clone됨)
- 입력 파일: 항상 `./inputs/<파일명>` (상대 경로)
- 출력 파일: 항상 `./output/<파일명>` (상대 경로, 평탄 구조)
- **로컬 PC 경로 추정 금지** — `~/Desktop`, `C:\Users` 등 절대 사용 안 함
- 결과물은 수강생이 Sandbox에서 직접 Download → 본인 PC에서 렌더

## §3. 파일명 규칙

`output/<순번>_<주제>_<YYYYMMDD>_<HHMM>.html`

- 순번: 01, 02, 03 (응답 시도 순)
- 주제: snake_case 한국어 가능 (`h_and_a_q1_review`)
- 시각: 한국 표준시(KST) 기준
- 예: `output/01_h&a_q1_review_20260508_1430.html`
- **하위 폴더 생성 금지** — 단일 실습 repo이므로 평탄 구조 유지

## §4. HTML 출력 표준

### §4.1 구조 (필수)

```html
<!doctype html>
<html lang="ko">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>...</title>
  <link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/orioncactus/pretendard/dist/web/static/pretendard.css">
  <script src="https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4"></script>
  <style>:root { --lg: #A50034; } body { font-family: "Pretendard", -apple-system, sans-serif; }</style>
</head>
<body class="bg-gray-50 text-gray-900 print:bg-white">
  <main class="max-w-3xl mx-auto p-8 print:p-4 space-y-6">...</main>
</body>
</html>
```

### §4.2 CDN 화이트리스트 (이 4개만 허용)

| 라이브러리 | URL | 폴백 |
|---|---|---|
| Tailwind v4 Play | `https://cdn.jsdelivr.net/npm/@tailwindcss/browser@4` | `unpkg.com/@tailwindcss/browser@4` |
| Pretendard | `https://cdn.jsdelivr.net/gh/orioncactus/pretendard/dist/web/static/pretendard.css` | 인라인 `@font-face` 폴백 |
| Chart.js (필요 시) | `https://cdn.jsdelivr.net/npm/chart.js` | SVG `<path>` 직접 그리기 |
| ApexCharts (선택) | `https://cdn.jsdelivr.net/npm/apexcharts` | Chart.js로 대체 |

> 사내망 차단 시 인라인 폴백. 외부 이미지·아이콘 폰트(FontAwesome 등) 금지.

### §4.3 임원 톤 가드레일

- **Top-down**: 결론 → 근거 → 디테일 (한 줄 요약 카드 최상단)
- **정량화 우선**: "증가" → "+4.2%", "확대" → "18% → 22%"
- **금지어**: "혁신적", "획기적", "압도적", "최고의", "놀라운" 등 과장 형용사
- **이모지 절대 금지** (체크 마크 ✓ 만 검증 표에서 허용)
- **다색 그라데이션 금지** — 화이트 + LG Red(#A50034) 단색 액센트
- **여백 충분**: `space-y-6`, `p-8`, 카드 사이 간격 `gap-4` 이상

### §4.4 인쇄 최적화 (필수)

```css
@media print {
  body { font-size: 11pt; background: white; }
  .no-print { display: none; }
  main { padding: 0.5in; }
}
```

- A4 1장 안에 들어가야 함 (모자라면 카드 축소, 넘치면 섹션 통합)
- `print:p-4`, `print:text-sm` Tailwind variant 적극 활용

## §5. 50KB 제약

- 단일 HTML 파일 50KB 이하 (외부 CDN 제외, 인라인 자산 포함)
- 초과 시 자동 축소: 데이터 표 → 핵심 5행만, 차트 → 카드 텍스트로 대체
- 측정 명령: `wc -c < output/*.html`

## §6. 자가 보완 규칙 (경로 평탄화)

검증 표에 ✗ 1개라도 있으면:

1. **이전 v1 보존** — 절대 덮어쓰기 금지
2. 새 파일 `output/01_h&a_q1_review_20260508_1430-v2.html` 생성 (동일 폴더)
3. 임원 앞에서 v1/v2 비교 시연 가능하도록

## §7. Spec-First 패턴 (강제)

수강생 프롬프트에 다음 5블록이 모두 있어야 함:

```
[목표] ... (한 줄)
[입력] ... (파일 경로 + 톤 가이드)
[출력 스펙] ... (파일 경로 + 섹션 구성 + 색상 + 폰트 + 제약)
[제약] ... (안 됨 / 반드시 / CDN)
[검증 기준] ... (5개 항목, 응답 끝에 표로 결과)
```

블록 누락 시 Claude는 누락 블록을 먼저 질문하고 시작.

## §8. 자가 체크리스트 (응답 끝 필수)

```
| 항목 | 합격 조건 | 결과 | 증거 |
|---|---|---|---|
| 1. 더블클릭 렌더 | HTML 단독 작동 | ✓/✗ | 파일 경로 |
| 2. 한글 인코딩 | charset UTF-8 + lang ko | ✓/✗ | 라인 번호 |
| 3. Action Items 완전 | 각 행 담당+마감 채워짐 | ✓/✗ | <table> 행 수 |
| 4. 인쇄 A4 1장 | @media print 적용 | ✓/✗ | <style> 위치 |
| 5. 임원 톤 | 이모지 0, 과장 형용사 0 | ✓/✗ | 본문 검색 |
```

## §9. 다운로드 안내 (응답 마지막)

응답 끝에 다음 안내문 자동 포함:

```
[다운로드 안내]
좌측 파일 트리에서 output/<생성된 파일> 우클릭 → Download
본인 PC에서 더블클릭하면 브라우저로 렌더됩니다.
Sandbox 세션이 만료되면 파일이 사라지니 반드시 미리 받으세요.
```

## §10. 금지 사항

- **Sandbox 외부 시스템 변경 금지** (`sudo`, `rm -rf /`, 시스템 패키지 변경)
- **repo 작업 디렉토리 밖 쓰기 금지** (`./output/` 외부에 산출물 저장 X)
- **외부 API 키 호출 금지** (OpenAI, Gemini 등 — 강의 한정)
