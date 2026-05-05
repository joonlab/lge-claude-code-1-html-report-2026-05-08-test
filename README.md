# LG전자 임원 Claude Code 실습 1 — HTML 보고서 자동 생성

![License](https://img.shields.io/badge/license-MIT-green)
![Version](https://img.shields.io/badge/version-2.3-blue)
![Environment](https://img.shields.io/badge/env-Claude%20Cloud%20Sandbox-purple)
![Date](https://img.shields.io/badge/date-2026--05--08-lightgrey)

> 비서 30분 작업을 single-shot 1회로.

---

## 실습 목표

- 회의록·이메일·PPT 텍스트 3종을 **임원 1-page HTML 보고서**로 단일 프롬프트 변환한다.
- Claude Code Cloud Sandbox 환경에서 **로컬 설치 없이** 실습을 완수한다.
- CLAUDE.md 기반 **사전 박힌 규칙**(브랜드·언어·인쇄·톤)이 결과물에 일관 적용되는 흐름을 체득한다.

### 기대 효과

- **소요 시간**: 비서 보조 기준 30분 → 1분 이내 (single-shot 1회)
- **품질 일관성**: 보고서 양식·브랜드 규정 자동 적용으로 편차 제거
- **재사용성**: 동일 프롬프트로 차주 회의 보고서 즉시 재생성 가능

---

## 무엇을 하는가

`inputs/` 폴더의 회의록, 이메일 스레드, PPT 텍스트를 입력으로, 단일 시연 프롬프트(single-shot)를 통해 임원용 1-page HTML 보고서를 자동 생성한다. 출력 결과물은 본 repo에 함께 들어 있는 **CLAUDE.md**에 사전 정의된 규칙(한국어 출력, UTF-8, LG Red `#A50034` 액센트, Pretendard 폰트, 임원 톤, 인쇄 최적화)을 모두 자동 적용한다. 강사는 강의 현장에서 시연 프롬프트 본문을 안내한다.

### 처리 흐름

```
inputs/*.txt  →  Claude Code (single-shot)  →  output/*.html
                       ↑
                  CLAUDE.md (자동 로드 규칙)
```

---

## 결과물 형태

생성되는 파일은 단일 `.html` 파일이며 다음 요건을 만족한다.

- **1-page HTML** — 외부 리소스 의존 없는 self-contained 형태, **50KB 이하**
- **구성 섹션**
  - 한 줄 요약 카드
  - 주요 의사결정 3카드
  - Action Items 표 (담당 / 항목 / 마감 / 상태)
  - 리스크 박스
- **스타일** — 화이트 배경 + LG Red `#A50034` 액센트 1색, Pretendard 폰트
- **인쇄 최적화** — A4 1장 인쇄 기준 레이아웃
- **자가 검증 표 자동 포함** — 결과물 검증용 체크 표가 함께 생성됨

---

## Quick Start (Cloud Sandbox 6단계)

1. Claude Desktop App 실행 → 좌측 `Code` 탭
2. `+ New` → `Cloud` 환경 선택 (Default)
3. GitHub repo 선택: `joonlab/lge-claude-code-1-html-report-2026-05-08`, branch `main`
4. CLAUDE.md 자동 로드 확인
5. 강사 시연 프롬프트 입력 (강의 현장에서 안내)
6. 생성된 `output/*.html` 파일 Download → 본인 PC 더블클릭으로 렌더 확인

---

## 폴더 구조

```
├── README.md            # 본 파일
├── CLAUDE.md            # Claude Code 자동 로드 규칙
├── LICENSE              # MIT
├── inputs/              # 시연용 텍스트 3종
│   ├── meeting-notes.txt    # LG H&A 정례회의록 (약 1,200자)
│   ├── email-thread.txt     # 이메일 스레드 (약 1,500자)
│   └── ppt-text.txt         # PPT 발췌 텍스트 (약 2,500자)
└── output/              # 결과물 저장 (.gitkeep)
```

---

## 사전 준비물

- **Claude Desktop App** (최신 버전)
- **Claude Pro/Max 구독** — Cloud Sandbox 사용 권한
- **인터넷 연결** — Anthropic Cloud 및 GitHub 도메인 접근
- 로컬 IDE/터미널 **불필요**

### 권장 환경

- **브라우저**: Chrome / Edge 최신 버전 (인쇄 미리보기 호환)
- **OS**: macOS, Windows 모두 지원
- **권장 화면**: 13인치 이상 (보고서 1-page 미리보기 가독성)

---

## 참고 문서

- `CLAUDE.md` — Cloud Sandbox 진입 시 자동 로드되는 규칙 정의
- `LICENSE` — MIT

---

## Data Sources

`inputs/*.txt` 3종은 본 강의용으로 가공된 **가짜 데이터**다. 실제 LG전자 내부 자료가 아니며, 실습 외 용도로 사용되지 않는다.

---

## License

MIT

---

## 문의

- **주관**: LG전자 인재육성팀
- **강사**: 박준 (joonlab)
- **운영**: CMDS_JoonLab (박준 · 구요한)
