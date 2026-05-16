# Phase 3.9-G 실행 계획 — AI News 기능 고도화
> 작성: Claude Code | 날짜: 2026-05-16 | 상태: IN PROGRESS
> 핸드오버 대상: Anti Gravity (중간 이어받기 가능하도록 작성)

---

## 📌 전제 확인

- **이미지 업로드**: Imgur API 이미 사용 중 (`Client-ID: 546c25a59c58ad7`, app.py:593)  
  → 편집 모달에서 파일 업로드 시 동일 API 재활용
- **현재 RSS 수집 한도**: `feed.entries[:20]` → 30으로 확대
- **현재 인사이트 조회 한도**: `limit(10)` → 30으로 확대
- **개발 레포**: `hsongkr/251216_portfolio` (main 브랜치)
- **협업 문서**: `hsongkr/nerdy-oracle-collab` (이 파일)

---

## 🗂️ STEP 목록 및 진행 현황

| STEP | 내용 | 상태 | 담당 | commit |
|------|------|------|------|--------|
| A | DB 마이그레이션 (컬럼 추가) | ✅ DONE | Claude Code | ef63358 |
| B | 수집 개선 (30건·기간 UI·URL박스·건수표시) | ✅ DONE | Claude Code | f95c661 |
| C | Step3 버튼 순서·종합분석·편집창 개선 | ✅ DONE | Claude Code | 1c25ef9 |
| D | /news-analytics 툴바·카드 전면 개선 | ✅ DONE | Claude Code | eec6254 |

---

## STEP A — DB 마이그레이션

**파일:** `models.py`, `migrations/005_news_insight_media.sql`

**추가 컬럼 (NewsInsight 테이블):**

| 컬럼 | 타입 | 설명 |
|------|------|------|
| `image_url` | TEXT | 대표 이미지 URL (Imgur 업로드 또는 직접 입력) |
| `youtube_url` | TEXT | 관련 YouTube 링크 |
| `related_tickers` | TEXT | JSON: `["NVDA","TSM"]` |
| `is_synthesis` | BOOLEAN | TRUE=종합분석, FALSE=단일기사 인사이트 |
| `source_title` | TEXT | 원문 기사 제목 |
| `source_url` | TEXT | 원문 기사 URL |
| `source_name` | TEXT | 언론사명 |
| `source_date` | TEXT | 원문 기사 날짜 (YYYY-MM-DD) |

**Railway 적용 방식:** app.py 기존 마이그레이션 블록(`ALTER TABLE ... ADD COLUMN IF NOT EXISTS`)에 추가

---

## STEP B — 수집 기능 개선

**B-1. 수집 건수 30건 확대**
- `feed.entries[:20]` → `feed.entries[:30]`
- `NewsRaw.query...limit(10)` → `limit(30)` (generate 함수 조회)

**B-2. 기간 필터 UI**
- STEP 2 섹션에 라디오 버튼: 1일 / 7일(기본) / 14일 / 30일
- POST collect 시 `days` 파라미터 전달 → `published_at >= cutoff` 필터

**B-3. 카테고리별 수집 현황 표시**
- 수집 버튼: `AI Infra 수집` → `AI Infra 수집 (저장 12 / 검색 28)`
- collect 응답에 `searched`, `saved` 카운트 포함 → JS로 버튼 텍스트 업데이트

**B-4. URL 복사 박스 (NotebookLM용)**
- 신규 라우트: `GET /admin/news-pipeline/urls?category=all&days=7`
- STEP 2 하단에 textarea + 📋 Copy 버튼
- 카테고리 필터 select + 기간 select

**B-5. 소스 확대 (장기)**
- 현재: Google News RSS만 사용
- 방향: NewsQuery 테이블에 feed_url 직접 등록 가능 → STEP 1 UI에서 RSS URL 추가
- 카테고리별 추천 RSS는 별도 기획 세션에서 결정

---

## STEP C — Admin Pipeline 개선

**C-1. Step4 버튼 순서 변경**
```
기존: [발행] [YouTube 큐] [초안으로] [✏️ 편집] [🗑]
변경: [발행] [초안으로] [✏️ 편집] [🗑] [🔬 심층분석]
```

**C-2. 종합분석 생성 버튼**
- STEP 3 카테고리 생성 버튼들 끝에 `[📊 종합분석 생성]` 추가
- 신규 라우트: `POST /admin/news-pipeline/generate-synthesis`

**C-3. 종합분석 생성 로직**
- 전 카테고리 Flash 상위 기사 수집 → Pro로 교차 인사이트 생성
- `is_synthesis=True`, `category='synthesis'`로 저장

**C-4. 편집창 개선 (이미지·YouTube·티커·원문정보)**
- 기존 모달에 아래 추가:
  - 🖼 이미지: 파일 업로드(→Imgur) OR URL 직접 입력
  - ▶ YouTube URL 입력 + 썸네일 미리보기
  - 🏷 관련 티커 (쉼표 구분, 예: `NVDA, TSM, 005930`)
  - 📰 원문 기사 제목 / URL / 언론사 / 날짜 입력

---

## STEP D — /news-analytics 전면 개선

**D-1. 레이아웃**
```
[AI 뉴스 인사이트]           ← 페이지 제목
──────────────────────────────
[종합분석] [AI Infra] [Energy] [AI서비스] [Physical AI] [Biology]
                                              ← sticky 툴바
──────────────────────────────
## 종합 분석 뉴스             ← 섹션 (id="section-synthesis")
[카드] [카드] ...

## AI Infra                  ← 섹션 (id="section-ai_infra")
[카드] [카드] ...
```

**D-2. 툴바 스크롤 연동**
- Intersection Observer: 섹션 뷰포트 진입 시 해당 툴바 버튼 하이라이트
- 버튼 클릭 → smooth scroll (툴바 높이 offset 고려)

**D-3. 카드 컴포넌트**
- 종합분석 배지 (is_synthesis=True)
- 인사이트 유형 배지
- 대표 이미지 (image_url 있는 경우)
- 본문 접힘/펼침 (기존 expand/collapse 유지)
- 관련 티커 태그
- YouTube 링크 버튼
- 원문 기사 링크 (제목·언론사·날짜)

---

## 🔧 핸드오버 가이드 (Anti Gravity용)

작업 이어받기 시:
1. `git pull origin main` (251216_portfolio)
2. `git pull origin main` (nerdy-oracle-collab) → 이 파일에서 현재 상태 확인
3. 위 STEP 표에서 ✅ DONE이 아닌 STEP부터 시작
4. 각 STEP 완료 후 아래 "구현 현황" 업데이트 + git push

---

## 📝 구현 현황 (완료 시 업데이트)

### STEP A
- 완료 일시: 2026-05-16
- 변경 파일: `models.py`, `migrations/005_news_insight_media.sql`
- commit: (완료 후 기입)

### STEP B
- 완료 일시: -
- commit: -

### STEP C
- 완료 일시: -
- commit: -

### STEP D
- 완료 일시: -
- commit: -
