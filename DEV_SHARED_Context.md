# DEV_SHARED_Context.md
> 이 파일은 **Gemini(기획자) · Claude(기획자) · Claude Code(프로그래머) · Anti Gravity(프로그래머) · Human(관리자)** 5명이 공유하는 협업 게시판입니다.
> 작업 완료 시 해당 섹션을 업데이트하고 git commit & push 하세요.

---

## 📚 연관 상세 문서

> 이 파일은 현황 요약본입니다. 기술 상세는 아래 문서를 확인하세요.

| 문서 | 내용 | 대상 독자 |
|------|------|----------|
| **[AI_뉴스_파이프라인_개발상세_20260518.md](./AI_뉴스_파이프라인_개발상세_20260518.md)** | DB 스키마 전체 · API 엔드포인트 목록 · 핵심 함수 상세 · 프론트엔드 구조 · 알려진 이슈 · 파일 구조 | 개발자 인수인계, 다음 세션 컨텍스트 복구 |



## 📋 프로토콜

| 역할 | 주로 쓰는 섹션 | 읽어야 할 섹션 |
|------|--------------|--------------|
| Gemini (기획자) | 🎯 기획 결정사항 | 🔧 구현 현황, ❓ 미결 질문 |
| Claude (기획자) | 🎯 기획 결정사항 | 🔧 구현 현황, ❓ 미결 질문 |
| Claude Code (프로그래머) | 🔧 구현 현황 | 🎯 기획 결정사항, ❓ 미결 질문 |
| Anti Gravity (프로그래머) | 🔧 구현 현황 | 🎯 기획 결정사항, ❓ 미결 질문 |
| Human (관리자) | ❓ 미결 질문, 📌 현재 컨텍스트 | 전체 |

**항목 작성 형식:**
```
- [YYYY-MM-DD] [작성자] 내용
  - 상세 내용 (필요시)
  → 상태: OPEN / IN PROGRESS / DONE
```

---

## 📌 현재 프로젝트 컨텍스트

**프로젝트:** Nerdy Oracle — 글로벌 마켓 리포트 자동화 시스템
**운영 URL:** https://251216portfolio-production.up.railway.app
**GitHub:** https://github.com/hsongkr/251216_portfolio (main 브랜치)
**문서:** 이 파일(DEV_SHARED_Context.md)에 모든 내용 통합 관리 (2026-05-12 Anti Gravity 통합)

**현재 완료된 Phase:**
- ✅ Phase 1: 블로그 포스팅 안정화
- ✅ Phase 2: 리포트 캐시 DB 영속화
- ✅ 프롬프트 관리 UI (`/settings/prompts`)
- ✅ Phase 3: GNB 탭 + 페이지 분리 + UI 전면 리디자인
- ✅ Phase 3.5: PWA 구현 + 안드로이드 실기기 동작 확인
- ✅ 운영 버그 수정 (2026-05-09): 데이터 로딩 실패 + NOT NULL 오류
- ✅ Phase 3.9-A 운영 안정화: SECRET_KEY 교체, index.html 삭제, 프롬프트 저장 오류 수정, OAuth 로그인 간소화 (2026-05-12)
- ✅ Phase 3.9-B(일부): 프롬프트 탭 복원, Chrome↔PWA 네비게이션 분리, 보고서 즉시 재생성 UI (2026-05-13)
- ✅ Phase 3.9-D AI뉴스 파이프라인 뼈대: DB 모델 4개, RSS 수집, 인사이트 상태 관리, GNB 활성화 (2026-05-14, Claude Code)
- ✅ Phase 3.9-E AI 인사이트 자동 생성: BeautifulSoup 기사 본문 수집 + Gemini 2.5 Pro 인사이트 초안 생성, 카드 expand/collapse (2026-05-14, Anti Gravity)
- ✅ Phase 3.9-F 프롬프트·품질 강화: What→Why→So What 분석 프레임, Gemini Flash 1차 랭킹 필터, 인사이트 편집/삭제 모달 (2026-05-15, Claude Code)
- ✅ Phase 3.9-G 미디어·합성·UI 전면 개편: NewsInsight 미디어 컬럼 추가, 이미지 업로드, 합성(synthesis) 인사이트 생성, /news-analytics 완전 재설계, RSS 직접 피드 전환 (2026-05-15~16, Claude Code)
- ✅ Phase 3.9-H 파이프라인 UX 완성: 실시간 수집 진행 모달(Progress Bar + 피드별 통계 테이블), STEP3 아코디언 동작 설명, Google News 토픽 피드 병행, google-genai 패키지 교체 (2026-05-17~18, Anti Gravity)
- ✅ Phase 3.9-I 수집 기사 품질 필터링: quality_score 3단계(1=본문없음, 2=본문있음, 3=Flash검증), Gemini Flash 관련성 검증 라우트, URL 목록 품질 수준 선택, 기사 뱃지 시각화 (2026-05-18, Claude Code)
- ✅ 버그픽스 2건: Google News 리다이렉트 URL 해제(NotebookLM 호환), Gemini 503 재시도·Flash 폴백 (2026-05-19, Claude Code)

**현재 작업:**
- 🔄 Phase 3.9: 안정화 & 소규모 배포 테스트
  - 3.9-A ✅ / 3.9-B ✅ / 3.9-D ✅ / 3.9-E ✅ / 3.9-F ✅ / 3.9-G ✅ / 3.9-H ✅ / 3.9-I ✅
  - **남은 작업**: 3.9-C UI 정리, 포트폴리오(/portfolio) CRUD 뼈대, 소규모 배포 테스트

**다음 작업:**
- 🔜 Phase 4: BYOK 생태계 구축 (Phase 3.9 완료 후)
- 🔜 Phase 5: Flutter 네이티브 앱 (Phase 4 이후)

**개발 환경:**
- Claude Code 웹 세션 (`/home/user/portfolio/`) — 오전·낮 담당
- Anti Gravity — 저녁·밤 담당
- Mac/PC/DeX 병행 사용 가능

---

## 🎯 기획 결정사항

> Gemini, Claude 기획자가 작성. 구현 팀(Claude Code, Anti Gravity)이 읽고 실행.

- [2026-05-09] [Claude] Phase 4 착수 전 안정화 단계(Phase 3.9) 진행 결정
  - 이유: Phase 4 바로 달리기보다 안정화 → 소규모 배포 → 피드백 수집이 현명
  - 순서: 운영 안정화 → 프롬프트 개량 → UI 정리 → 뼈대 페이지 → 소규모 배포
  → 상태: IN PROGRESS / 담당: Anti Gravity

- [2026-05-09] [Claude] guest vs member 권한 범위 확정
  - guest: 시황/리포트 읽기 전용 (생성 불가, 비로그인 유사 계정)
  - member: 무료 10회 열람 + Phase 4에서 BYOK 등록 후 생성 가능
  - Phase 4 구현 시 이 기준으로 권한 분기 처리
  → 상태: DONE / 담당: Claude Code (Phase 4 구현 시 반영)

- [2026-05-14] [Claude Code] 3.9-E 포트폴리오 뼈대 구현 계획 제안
  - **범위**: `/portfolio` 엔드포인트 — 보유 종목 관리 + yfinance 실시간 수익률
  - **DB 모델**: PortfolioHolding (ticker, average_price, quantity, notes, created_by, created_at)
  - **라우트**:
    - GET `/portfolio` (@approved_required): 보유 종목 목록 + yfinance 데이터 (시가총액, 현가, 수익률) 표시
    - POST/PUT `/api/portfolio/holding` (@approved_required): 종목 추가/수정 (JSON)
    - DELETE `/api/portfolio/holding/{id}` (@approved_required): 종목 삭제
  - **UI**: 카드 레이아웃 — 종목명/티커 + 평단가 + 현재가 + 수익률(% 색상) + 총 자산 + [추가]/[편집]/[삭제] 버튼
  - **주의**: Phase 4(BYOK)에서 종목별 AI 분석(근거/리스크) 추가될 예정
  - **다음**: 3.9-C UI 정리 완료 후 착수 권장
  → 상태: PROPOSED / 담당: Claude Code 또는 Anti Gravity (담당자 협의 필요)

- [2026-05-09] [Claude] AI뉴스 / 포트폴리오 페이지 Phase 3.9에서 뼈대 구현
  - 현재 "예정" 배지 제거 → 탭 활성화
  - Phase 3.9: 기본 UI + 데이터 표시 (AI 분석 기능 없음)
  - Phase 4: BYOK 기반 AI 분석 기능 추가
  - AI뉴스(/news-analytics): 검색식 입력 UI + Google RSS 기본 뷰 ✅ (3.9-D 완료)
  - 포트폴리오(/portfolio): 종목/평단가 입력 CRUD + yfinance 수익률 계산 (3.9-E 계획)
  → 상태: PARTIALLY DONE (3.9-D 완료, 3.9-E 계획수립)

- [2026-05-09] [Claude] 보고서 프롬프트 개량 방향
  - 현재 프롬프트: 데이터 나열 중심
  - 목표: 핵심 인사이트 중심, 너디 오라클 톤앤매너 반영
  - 톤: 전문 연구원 시각 + 스마트 개미 타깃 + 신뢰감 있고 분석적
  - /settings/prompts UI에서 직접 튜닝 후 Gemini와 함께 검토
  → 상태: OPEN / 담당: Human(튜닝) + Gemini(검토)

---

## 🔧 구현 현황

> Claude Code, Anti Gravity가 작성. 기획 팀이 읽고 다음 기획에 반영.

- [2026-05-19] [Claude Code] 버그픽스 2건 (commit: 35dbf9c)
  - **Google News 리다이렉트 URL 해제**: 수집 시 `news.google.com/articles/CBMi...` URL을 `requests.head(allow_redirects=True)`로 실제 기사 URL로 변환 저장 → NotebookLM 파싱 불가 문제 해결
  - **Gemini 503 재시도·폴백**: `_gemini_generate()`에 503/UNAVAILABLE 감지 → 최대 3회 재시도(10s, 20s) 추가. generate/synthesis 라우트: Pro 재시도 후에도 실패 시 Flash로 자동 폴백. UI: 503/429 에러 시 한국어 안내 + "잠시 후 재시도" 가이드
  → 상태: DONE / Railway auto-deploy 완료

- [2026-05-18] [Claude Code] 3.9-I 수집 기사 품질 필터링 시스템 구현 (commit: a7b7d05)
  - **`quality_score` 컬럼 추가** (`news_raw` 테이블): 1=본문없음, 2=본문있음(미검증), 3=Flash검증통과
  - **수집 시 자동 설정**: `_collect_rss_for_category` — full_text 300자 이상이면 2, 아니면 1
  - **신규 라우트 `POST /admin/news-pipeline/quality-filter`**: quality_score=2인 기사를 Gemini 2.5 Flash로 배치 검증(30건씩), 관련없으면 DB 삭제, 관련있으면 quality_score=3으로 승격
  - **`GET /admin/news-pipeline/urls` 개선**: `quality=all|text|curated` 파라미터 추가 (all>=1, text>=2, curated>=3)
  - **STEP 2 UI 개선**:
    - 🧹 전체/분야별 품질 필터 버튼 추가, 삭제/통과 건수 실시간 표시
    - URL 복사 박스에 품질 수준 선택기(전체/본문있음/검증됨) 추가
    - 기사 목록: ✦검증(보라)/✓본문(초록)/본문없음(회색) 배지로 품질 시각화
  → 상태: DONE / Railway auto-deploy 완료

- [2026-05-17~18] [Anti Gravity] 3.9-H AI뉴스 파이프라인 UX 완성 + 파이프라인 수리 (3커밋)
  - **RSS 수집 수리**: Google News 토픽 피드 + 직접 피드 병행, 키워드 필터 완화(STOP_WORDS 제외, 최대 8개), 기간 14일로 확대 → ai_infra 카테고리 로컬 테스트: 43건 수집 성공
  - **google-genai 패키지 교체**: `google-generativeai`(deprecated) → `google-genai>=1.0.0`, `_gemini_generate()` 헬퍼 함수 도입, FutureWarning 완전 제거
  - **실시간 수집 진행 모달**: 전체 수집 누르면 팅업 트리거 — 진행 바(RSS 사이트 검색 완료 N/전체 M x 100%), 분야별 테이블(검색된 기사/저장된 기사/Step3 후보/피드 수) 실시간 표시, 완료 후 → STEP3 링크 자동 노출
  - **STEP3 아코디언 설명술**: ℹ️ 버튼 클릭 시 Phase A(Flash 랭킹)·Phase B(Pro 인사이트 작성)·금지어 데이지 노출
  - **신규 API**: `GET /admin/news-pipeline/collect-status` — 카테고리별 수집 현황(총 기사수/본문보유 수/Step3 후보) 반환
  → 상태: DONE / commit 3건 + Railway auto-deploy 완료

  - **3.9-F STEP1**: 프롬프트 엔지니어링 강화 — What→Why→So What 분석 프레임워크, INSIGHT_TYPES 상세 정의, 기사 컨텍스트 500자→1,500자 확대, 시황 보고서 톤앤매너 개선
  - **3.9-F STEP2**: Gemini Flash 1차 중요도 랭킹 필터 — 수집 기사 중 상위 5건만 Pro에 전달해 비용 절감 + 어그로 기사 자동 차단
  - **3.9-F STEP3**: 관리자 UI에 인사이트 편집 모달 및 삭제 버튼 추가
  - **3.9-G STEP A**: `NewsInsight` DB 컬럼 추가 — `image_url`, `youtube_url`, `related_tickers`, `is_synthesis`, `source_title/url/name/date`
  - **3.9-G STEP B+C**: 백엔드 라우트 신설 — `/admin/news-pipeline/urls`(수집 기사 URL 목록), `/admin/news-pipeline/upload-image`(이미지 업로드), `/admin/news-pipeline/edit`(인사이트 편집), `/admin/news-pipeline/delete`, `/admin/news-pipeline/generate-synthesis`(합성 인사이트 생성)
  - **3.9-G STEP D**: `/news-analytics` 회원용 화면 완전 재설계 — 카드 디자인 리뉴얼, 소스 링크·날짜 표시, 이미지 썸네일 지원
  - **RSS 피드 개선**: 직접 피드 URL(site별 RSS) + 키워드 필터링으로 변경 → 수집 0건 문제 해결
  → 상태: DONE / Railway auto-deploy 완료

- [2026-05-14] [Anti Gravity] 3.9-E AI 인사이트 자동 생성 구현 완료
  - **BeautifulSoup 기사 본문 수집**: RSS 수집 시 각 URL 크롤링, `<p>` 태그 추출 (최대 5,000자, 5초 타임아웃, 실패 시 `full_text=None` 무시 후 계속)
  - **신규 라우트 `POST /admin/news-pipeline/generate`** (@supervisor_required):
    - 카테고리별 최근 기사 10건 → Gemini 2.5 Pro 프롬프트 전달 (기사당 본문 최대 500자)
    - 브랜딩 금지어 ("주가 예측", "매수 타이밍" 등) 프롬프트 명시 반영
    - JSON 응답 파싱 → NewsInsight(status='draft') 저장 + PipelineRun 기록
    - 마크다운 코드블록 자동 제거 처리
  - **STEP 3 버튼 활성화**: `disabled` 제거 → `generateInsight(cat, btn)` JS 함수 연결
  - **인사이트 카드 UX 개선** (news_analytics.html):
    - 카드 클릭 시 expand/collapse 토글 (CSS `.expanded`)
    - 접힘: 150자 미리보기 + "[자세히 보기]" 힌트
    - 펼침: `{{ ins.body | markdown | safe }}` 완전 렌더링
    - Jinja2 `markdown` 필터 등록 (`@app.template_filter`)
  - **추가된 의존성**: `beautifulsoup4>=4.12.0` (requirements.txt → Railway 재배포 완료)
  → 상태: DONE / commit & push 완료 (3개 커밋, Railway auto-deploy)

- [2026-05-14] [Claude Code] 3.9-D AI뉴스 파이프라인 뼈대 구현 완료 + 로컬 검증
  - **신규 DB 테이블 4개**: news_queries, news_raw, news_insights, pipeline_runs (SQLAlchemy ORM + auto create)
  - **STEP별 구현 범위**:
    - STEP 1 (검색식): UI 구현, 카테고리별 수동 편집 가능, 자동생성 버튼 비활성
    - STEP 2 (RSS 수집): **완전 구현** (feedparser로 Google News 수집, URL 중복 제거)
    - STEP 3 (AI 인사이트): UI 구현, 버튼 비활성화, "향후 phase에서 활성화" 안내
    - STEP 4 (검수 & 발행): **완전 구현** (상태 관리: draft/review/published/youtube_queue)
    - STEP 5 (실행 로그): **완전 구현** (PipelineRun 테이블 기록 — collect/generate/publish)
  - **신규 라우트 5개**:
    - GET `/news-analytics` (@approved_required): 회원용 인사이트 게시판, published 인사이트 카테고리별 표시
    - GET `/admin/news-pipeline` (@supervisor_required): 관리자 대시보드 (5개 STEP 통합)
    - POST `/admin/news-pipeline/query`: 검색식 저장/수정
    - POST `/admin/news-pipeline/collect`: RSS 수집 (카테고리별/전체, Google 속도제한 주의)
    - POST `/admin/news-pipeline/publish`: 인사이트 상태 변경 + 로그 기록
  - **UI 활성화**: GNB "AI뉴스" 탭 활성화, 햄버거 메뉴 관리자 링크 추가
  - **로컬 검증**: ✅ 4개 테이블, ✅ 5개 라우트, ✅ 템플릿 렌더링, ✅ RSS 수집 메커니즘
  - **수정 파일**: models.py, app.py, requirements.txt, templates/layout.html
  - **신규 파일**: templates/news_analytics.html, templates/admin_news_pipeline.html, migrations/003_news_pipeline.sql
  → 상태: DONE / 5 커밋 + Railway auto-deploy 완료

- [2026-05-14] [Anti Gravity] 외부 API(NotebookLM) 연동 사전 준비 완료
  - Node.js(`npm`) 시스템 패키지 설치 완료 (MCP 서버 실행 환경 구축)
  - 사용자는 IDE의 MCP 설정 메뉴에서 아래 설정을 직접 추가해야 함:
    - Name: `NotebookLM`
    - Command: `npx`
    - Arguments: `-y @jacob-bd/notebooklm-mcp-cli`
  → 상태: DONE / commit & push 완료

- [2026-05-13] [Anti Gravity] AI 프롬프트 콘텍스트 개선 (뉴스 발행일시 추가)
  - 구글 뉴스 RSS 및 yfinance API에서 `pubDate`를 추출하여 AI에게 명시적으로 제공함으로써, AI가 최신/과거 뉴스를 구분하지 못하고 "날짜 미상"으로 출력하던 문제 완벽 해결
  → 상태: DONE / commit & push 완료

- [2026-05-13] [Claude Code] 보고서 즉시 재생성 기능 추가 완료
  - `/api/admin/regenerate` 엔드포인트 신설 및 프롬프트 관리 화면 내 '지금 재생성' 버튼 추가
  → 상태: DONE / commit & push 완료

- [2026-05-13] [Claude Code] 프롬프트 탭 위치 유지 및 Chrome/Android 앱 네비게이션 분리 완료
  - redirect 시 탭 파라미터 전달 및 manifest scope 설정, SW 캐시 v3 업데이트
  → 상태: DONE

- [2026-05-12] [Claude Code] BUG 2건 수정 완료
  - 프롬프트 저장 오류: PromptTemplate 신규 생성 시 section1/2/3/warning_text 기본값('') 명시 (NOT NULL 방어)
  - OAuth 로그인 단계 간소화: `prompt='consent'` → 가입(signup) 모드만 consent, 로그인은 select_account로 변경
    → 기존 사용자 재로그인 시 불필요한 동의 화면·재인증 단계 제거
  - 수정 파일: app.py (lines 316-318, 241), models.py (lines 34-37)
  → 상태: DONE / commit & push 완료

- [2026-05-11] [Claude Code] Phase 3.9 개발 환경 준비 완료 (Claude Code 웹 세션)
  - Railway CLI v4.57.4 설치 (npm 글로벌)
  - Python 3.11 venv + requirements.txt 전체 패키지 설치 완료
  - 로컬 .env 파일 생성 (SQLite 로컬 DB 사용)
  - Flask 앱 로컬 실행 확인 (http://localhost:5001 → Nerdy Oracle 로그인 페이지 정상)
  - `templates/index.html` 구버전 파일 삭제 완료
  - git 인증 설정 완료 (PAT 기반)
  - Railway 연결: railway login 후 `railway link`로 fabulous-heart / 251216_portfolio 선택 필요
  - ✅ SECRET_KEY 교체 완료 (2026-05-11): Railway 웹 대시보드에서 강력한 랜덤 키로 교체
  - ℹ️ 이 클라우드 환경 IP는 Railway 네트워크 allowlist 외부 → 프로덕션 API curl 직접 불가 (브라우저/cron-job.org에서는 정상)
  → 상태: DONE (Railway 로그인은 Human 직접 처리 필요)

- [2026-05-09] [Claude Code] 운영 버그 2건 수정 완료
  - yfinance 병렬화(ThreadPoolExecutor) + NaN 안전처리
  - api_approved_required 신규 데코레이터 + session 임포트 수정
  - section1 NOT NULL 제약 DROP 마이그레이션
  - SW 캐시 v1→v2 버전 업
  → 상태: DONE

- [2026-05-08] [Claude Code] Phase 3.5 PWA 완료
  → 상태: DONE

- [2026-05-08] [Claude Code] Phase 3 UI 리디자인 완료
  → 상태: DONE

- [2026-05-08] [Claude Code] Phase 2 리포트 DB 영속화 완료
  → 상태: DONE

---

## ❓ 미결 질문 / 블로커

- [2026-05-09] [Claude Code → Anti Gravity] Phase 3.9 착수 전 확인 요청
  - 1. `templates/index.html` 구버전 파일 삭제 → ✅ DONE (2026-05-11 Claude Code)
  - 2. Railway `SECRET_KEY` 강력한 랜덤 키로 교체 → ✅ DONE (2026-05-11 Human)
  - 3. Phase 3.9 상세 작업 목록 → DEV_SHARED_Context.md 내 통합 관리
  → 상태: DONE

- [2026-05-08] [Claude Code → 기획팀] 손님(guest) 역할 기능 범위 질문
  → 상태: RESOLVED / [2026-05-09] [Claude] guest=읽기전용, member=열람+생성으로 확정

---

## 📓 세션 로그

> 각 AI/사람이 작업 시작·종료 시 한 줄 기록. 최신이 위.

- [2026-05-18] [Anti Gravity] 3.9-H 완료. 실시간 수집 진행 모달 + STEP3 아코디언 + google-genai 교체 + RSS 수집 수리. 3 커밋 push.
- [2026-05-17] [Anti Gravity] RSS 수집 수리(Google News 토픽 피드 병행, 키워드 필터 완화) + google-genai 패키지 전환 완료. commit push.
- [2026-05-16] [Anti Gravity] GitHub 최신 상태(159dd3a) 로컵 pull 완료. 3.9-F/G 커밋 10건 동기화. DEV_SHARED_Context 업데이트.
- [2026-05-15] [Claude Code] git pull 완료 — Anti Gravity 3.9-E 커밋 3개 로컬 동기화. DEV_SHARED_Context 양쪽 업데이트. next: 3.9-C UI 정리 또는 포트폴리오 뼈대 (담당자 협의).
- [2026-05-14] [Anti Gravity] 3.9-E 완료 (기사 본문 수집, Gemini 인사이트 생성, 카드 expand/collapse, Markdown 렌더링). 3개 커밋 push.
- [2026-05-14] [Claude Code] 3.9-D 검증 완료. 로컬 DB·라우트·템플릿 확인, RSS 메커니즘 검증.
- [2026-05-14] [Claude Code] 3.9-D 파이프라인 완성 (모델 4개 + 라우트 5개 + 템플릿 2개 + GNB 활성화). 5개 git 커밋 + Railway auto-deploy 완료.
- [2026-05-14] [Anti Gravity] NotebookLM 연동을 위한 Node.js 설치 및 MCP 설정 가이드 문서화 완료.
- [2026-05-13] [Anti Gravity] 뉴스 데이터에 발행일시(pubDate) 추가 파싱 로직 구현 (AI의 과거/현재 뉴스 컨텍스트 인지 강화).
- [2026-05-13] [Claude Code] 보고서 즉시 재생성 기능 추가 (`/api/admin/regenerate` 엔드포인트 및 관리 UI 버튼 연동).
- [2026-05-13] [Claude Code] BUG 2건 추가 수정 완료 (작동 확인)
  - 프롬프트 저장 후 탭 위치 유지: redirect에 ?tab={market} 파라미터 추가, prompts.html에서 복원
  - Chrome/Android 앱 네비게이션 분리: manifest.json scope="/" 추가 + layout.html display-mode 감지 스크립트 + SW 캐시 v3
  - PWA 재설치 후 Chrome↔앱 네비게이션 정상 분리 확인
  → 상태: DONE
- [2026-05-13] [Claude Code] GitHub pull 완료 — 밤사이 Anti Gravity 작업 확인 및 DEV_SHARED_Context 내 삭제된 파일 참조 정리.
- [2026-05-12] [Anti Gravity] 지표 6개로 확대(US 10Y·WTI·PHLX Semi 추가, Uranium 제거), 차트 날짜 표시 추가, 최근 리포트 3개 표시, 핀치줌 비활성화. 문서 Devlopment_Plan·HANDOVER·To-Do_List → DEV_SHARED_Context 통합.
- [2026-05-12] [Claude Code] 오늘 작업 종합 정리. 다음 세션(Anti Gravity)은 3.9-C UI 정리 또는 3.9-D AI뉴스 뼈대 구현 착수 권장.
- [2026-05-12] [Claude Code] 프롬프트 적용 흐름 코드 검토 완료 — get_prompt_template() → Gemini 삽입 정상 연결 확인. 오후 cron 리포트 결과 확인 예정.
- [2026-05-12] [Claude Code] BUG 2건 수정 (프롬프트 저장 오류, OAuth 로그인 단계 간소화) commit & push 완료.
- [2026-05-11] [Claude Code] Claude Code 웹 세션 개발 환경 구성 완료. Railway CLI 설치, Python venv, 로컬 앱 실행 확인. templates/index.html 삭제. Phase 3.9 개발 준비 완료.
- [2026-05-09] [Claude] Phase 3.9 안정화 단계 기획 결정사항 작성. guest/member 권한 확정. AI뉴스/포트폴리오 뼈대 구현 방향 결정. To-Do_List.md 업데이트.
- [2026-05-09] [Claude Code] 운영 버그 2건 수정 완료. 문서 일괄 업데이트. 토큰 쿼타 소진으로 퇴근. 다음: Phase 3.9 (Anti Gravity 담당).
- [2026-05-08] [Human] Phase 3.5 PWA 안드로이드 실기기 확인 완료.
- [2026-05-08] [Claude Code] Phase 3.5 PWA 완료 + 햄버거 메뉴 z-index 버그 수정 + 보고서 1일 차트 변경.
- [2026-05-08] [Claude Code] DEV_SHARED_Context.md 초기 생성.
- [2026-05-08] [Human] 5인 협업 채널 구성 요청 → DEV_SHARED_Context.md 방식 채택.
