# Anti Gravity 작업 확인 결과 (2026-05-15)

> 작성: Claude Code | 날짜: 2026-05-15 오전  
> 목적: 어제(2026-05-14) 저녁 Anti Gravity가 GitHub에 push한 작업 내역 확인 및 정리

---

## ✅ 결론: GitHub 정상 업데이트됨 (커밋 3개)

`송훈`(Anti Gravity) 계정으로 2026-05-14 21:43~22:07 KST 사이에 3개 커밋이 push되어 있음.  
로컬 `/home/user/portfolio`에 `git pull` 완료 (`55eb267` → `e64e8db`).

---

## 커밋 상세

### 커밋 1: `bc521b36` — Phase 3.9-E Full text scraping & Gemini insight generation
**시각:** 2026-05-14 21:43 KST  
**변경 파일:** `app.py` +115줄, `requirements.txt` +1줄, `templates/admin_news_pipeline.html` +25줄, `DEV_SHARED_Context.md` +1줄

**구현 내용:**

1. **BeautifulSoup 기사 본문 수집** — RSS 수집 시 각 URL에 `requests.get()` + `<p>` 태그 추출
   - 최대 5,000자 제한
   - 5초 타임아웃, 실패 시 경고 로그만 남기고 `full_text=None`으로 계속 진행

2. **신규 라우트 `POST /admin/news-pipeline/generate`** (`@supervisor_required`)
   - 카테고리별 최근 기사 10건 조회
   - Gemini 2.5 Pro에 프롬프트 전달 (기사당 본문 최대 500자)
   - 브랜딩 금지어 명시 ("주가 예측", "매수 타이밍" 등 사용 불가)
   - Gemini JSON 응답 파싱 → `NewsInsight(status='draft')` 저장 + `PipelineRun` 기록
   - 마크다운 코드블록 자동 제거 처리 (` ```json ``` ` 형식 방어)

3. **STEP 3 버튼 활성화**
   - `admin_news_pipeline.html`에서 `disabled` 제거
   - `generateInsight(cat, btn)` JS 함수 연결

4. **`requirements.txt`에 `beautifulsoup4>=4.12.0` 추가** → Railway 자동 재배포 트리거됨

---

### 커밋 2: `75747c8e` — Expand/collapse + Markdown 렌더링
**시각:** 2026-05-14 21:58 KST  
**변경 파일:** `app.py` +5줄, `templates/news_analytics.html` +22줄

**구현 내용:**

- `@app.template_filter('markdown')` Jinja2 필터 등록 (Python `markdown` 라이브러리 활용)
- 인사이트 카드 클릭 시 **expand/collapse** 토글 (CSS `.expanded` 클래스)
  - 접힌 상태: 텍스트 150자 미리보기 + `[자세히 보기]` 힌트
  - 펼친 상태: `{{ ins.body | markdown | safe }}` — Markdown 완전 렌더링 (굵게, 목록, 소제목 등)

---

### 커밋 3: `e64e8db` — Style 패딩 수정
**시각:** 2026-05-14 22:07 KST  
**변경 파일:** `templates/news_analytics.html` +2줄

**구현 내용:**
- 인사이트 카드 내 리스트 항목 padding 소폭 조정 (가독성 개선)

---

## 의존성 검증 결과 (문제 없음)

| 항목 | 상태 |
|------|------|
| `markdown` 라이브러리 | ✅ 이미 `requirements.txt`에 있었음 (`markdown>=3.0.0`) |
| `requests` 라이브러리 | ✅ 이미 있었음 (`requests>=2.31.0`) |
| `beautifulsoup4` | ✅ 이번 커밋에서 추가됨 |
| `markdown` import in `app.py` | ✅ line 13에 있음 |
| `genai` (Gemini) | ✅ 기존에 이미 import됨 |

---

## 운영 환경 테스트 결과

> 사용자(송훈) 직접 확인 — **정상 동작 확인**

| 테스트 항목 | 결과 |
|------------|------|
| STEP 2 RSS 수집 → 기사 목록 갱신 | ✅ |
| STEP 3 "AI Infra 생성" 버튼 → Gemini 인사이트 draft 생성 | ✅ |
| STEP 4 draft → "발행" → published 상태 변경 | ✅ |
| `/news-analytics` 인사이트 카드 노출 | ✅ |
| 카드 클릭 → expand/collapse 토글 | ✅ |
| 펼친 상태 Markdown 렌더링 | ✅ |

---

## 현재 Phase 상태 요약

| Phase | 내용 | 상태 | 담당 |
|-------|------|------|------|
| 3.9-A | 운영 안정화 | ✅ DONE | Claude Code |
| 3.9-B | 프롬프트 탭 복원, PWA 네비게이션, 보고서 재생성 | ✅ DONE | Claude Code |
| 3.9-C | UI 정리 | 🔜 대기 | Anti Gravity |
| 3.9-D | AI뉴스 파이프라인 뼈대 (RSS + 파이프라인 UI) | ✅ DONE | Claude Code |
| 3.9-E | Gemini 인사이트 자동 생성 + 카드 UX | ✅ DONE | Anti Gravity |
| 포트폴리오 뼈대 | /portfolio CRUD + yfinance 수익률 | 🔜 대기 | 담당자 미정 |
| 소규모 배포 | 3.9 완료 후 피드백 수집 | 🔜 대기 | Human |

---

*관련 문서: `AI_News_개선계획_20260515.md` (품질 개선 기획안)*
