# Phase 3.9-H 버그픽스 & RSS 아키텍처 개선 로그
> 작성: Claude Code | 날짜: 2026-05-16 | 상태: DONE
> 251216_portfolio commits: ed28a95 → fe8808d → 2bc0bd3 → 4610e99 → 159dd3a

---

## 수정 항목 목록

### BUG 1 — 툴바 스크롤 시 GNB 뒤로 숨는 문제 (commit: ed28a95)
**파일**: `templates/news_analytics.html`

**원인**: `.news-toolbar { top: 0 }` 고정값으로 GNB(`.app-header`)가 차지하는 높이를 무시

**수정**:
- JS `initToolbar()` 함수 추가: `document.querySelector('.app-header').getBoundingClientRect().height`로 실제 높이 측정
- `toolbar.style.top = headerH + 'px'` 동적 적용
- `scroll-margin-top`도 동적 갱신 (헤더 + 툴바 합산)
- `window.addEventListener('resize', initToolbar)` — 화면 크기 변경 시 재계산
- Intersection Observer도 새 TOOLBAR_H로 재생성

---

### BUG 2 — 뉴스 섹션 순서 오류 + Jinja 템플릿 오류 (commit: ed28a95)
**파일**: `templates/news_analytics.html`

**원인 1**: `{% for cat, ins_list in insights_by_category.items() %}` — dict 삽입 순서(DB 조회 순)로 렌더링되어 AI Biology가 첫 번째로 나오는 등 순서 불일치

**원인 2**: `{% if ins_list %}` 블록의 `{% endif %}`가 `{% endfor %}` 앞에 없어 Jinja 구조 오류

**수정**:
```jinja2
{% for cat in categories %}
{% set ins_list = insights_by_category.get(cat, []) %}
{% if ins_list %}
  ... 섹션 HTML ...
{% endif %}
{% endfor %}
```
→ `CATEGORIES` 딕셔너리 정의 순서(AI Infra → Energy Infra → AI 서비스 → Physical AI → AI Biology) 유지

---

### BUG 3 — JS 함수 중복 선언 (commit: fe8808d)
**파일**: `templates/admin_news_pipeline.html`

**원인**: Phase 3.9-F에서 추가된 구버전 `collectRSS`·`generateInsight` 함수가 3.9-G 신버전과 같은 파일에 공존 (JavaScript는 마지막 선언이 이기므로 동작은 했으나 코드 혼란)

**수정**: 구버전 두 함수 제거

---

### BUG 4 — default_queries 변수 템플릿 미전달 (commit: fe8808d)
**파일**: `app.py`, `templates/admin_news_pipeline.html`

**원인**: 템플릿 STEP 1에서 `{{ default_queries[cat] }}`을 참조하나 `admin_news_pipeline` 라우트가 `default_queries` 파라미터를 미전달 → `_ensure_default_queries()`가 항상 실행돼 실제 에러는 숨겨짐

**수정**: `render_template(..., default_queries=DEFAULT_QUERIES, ...)`

---

### FEATURE — 인사이트 생성 시 원문 정보 자동 저장 (commit: 2bc0bd3)
**파일**: `app.py` (`admin_news_pipeline_generate` 라우트)

**배경**: AI 생성 인사이트에 `source_url/title/name/date` 미저장 → 카드에 원문 링크 표시 불가

**수정**:
```python
related_ids = item.get('related_news_ids', [])
src_article = next((n for n in top_news if n.id == related_ids[0]), None) or top_news[0]
NewsInsight(
    ...
    source_url=src_article.url,
    source_title=src_article.title,
    source_name=src_article.source,
    source_date=src_article.published_at.strftime('%Y-%m-%d'),
)
```

---

### FEATURE — /news-analytics 카드 원문 링크·날짜 개선 (commit: 2bc0bd3)
**파일**: `templates/news_analytics.html`

**변경 내용**:
- 원문 링크(source_box)를 카드 하단 → **제목 바로 아래**로 이동, 항상 노출(접힘 상태에서도)
- 날짜: `ins.created_at` → **`ins.source_date` 우선**, fallback: `updated_at` → `created_at`
- 날짜를 `card-top` 우측에 배치하여 한눈에 확인 가능

**카드 레이아웃 (변경 후)**:
```
[밸류체인 파급]                      [2025.05.14]
美中 AI 반도체, '관리형 통제'로
📰 NVIDIA H200 Export... — Reuters
핵심 사실 로이터 보도에 따르면...  [자세히 보기]
```

---

### BUG 5 + ARCHITECTURE — RSS 수집 0건 문제 → 직접 피드 전환 (commits: 4610e99, 159dd3a)
**파일**: `app.py`, `templates/admin_news_pipeline.html`

**원인**: Railway 서버 IP가 Google News RSS에서 차단 → 모든 카테고리 `searched=0, added=0`

**해결 방향**: Google News RSS 검색 URL → **분야별 직접 RSS 피드** + **키워드 필터링**

**아키텍처**:
```
[직접 RSS 피드들] → feedparser 수집 → [키워드 필터] → DB 저장
  (TechCrunch, Ars Technica,           (query_text의 단어 중
   Reuters, The Verge 등)               하나라도 제목·요약 포함)
```

**DEFAULT_FEED_URLS** (분야별 기본 RSS):
| 분야 | RSS 피드 |
|------|---------|
| AI Infra | TechCrunch, Ars Technica Tech Lab, The Verge, Reuters Tech |
| Energy Infra | TechCrunch, Reuters Business, Ars Technica |
| AI 서비스 | TechCrunch AI, The Verge AI, Ars Technica |
| Physical AI | TechCrunch Robotics, TechCrunch, Ars Technica Cars |
| AI Biology | TechCrunch Biotech, STAT News, Reuters Health |

**STEP 1 UI 변경**:
- 기존: 검색식 1개 textarea
- 변경: ① 키워드 필터 textarea + ② RSS 피드 URL 목록 textarea (줄바꿈 구분)
- 저장 시 두 필드 모두 DB에 저장 (query_text + feed_url)
- `updated_by = 사용자 이메일` 저장 → 다음 배포 시 시스템 자동 갱신 방지

**_collect_rss_for_category 변경**:
- `feed_url`을 `\n` split → 각 URL별 feedparser 실행
- 각 entry에 키워드 필터 적용 (title + summary)
- feed_url 도메인을 source 이름 fallback으로 사용

---

## 현재 운영 상태 (2026-05-16 기준)

| 항목 | 상태 |
|------|------|
| /news-analytics 툴바·섹션 | ✅ 정상 |
| 원문 링크·날짜 표시 | ✅ 신규 생성 인사이트부터 자동 |
| RSS 수집 | ✅ 직접 피드로 전환 (Railway에서 작동) |
| 인사이트 생성 (Step 3) | ✅ Flash 랭킹 + Pro 생성 |
| 편집 모달 (이미지·YouTube·티커·원문) | ✅ 정상 |
| 종합분석 생성 | ✅ 정상 |

## 다음 필요 작업
- 운영 테스트: Step 2 수집 → Step 3 생성 → Step 4 발행 → /news-analytics 확인
- 기존 인사이트(source_url 없는 것): 편집 모달에서 원문 URL 수동 입력 필요
- 포트폴리오 뼈대 (/portfolio CRUD + yfinance) — 미착수
