# DEV_SHARED_Context.md
> 이 파일은 **Gemini(기획자) · Claude(기획자) · Claude Code(프로그래머) · Anti Gravity(프로그래머) · Human(관리자)** 5명이 공유하는 협업 게시판입니다.
> 작업 완료 시 해당 섹션을 업데이트하고 git commit & push 하세요.

---

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
**GitHub(개발):** https://github.com/hsongkr/251216_portfolio (main 브랜치, private)
**GitHub(협업):** https://github.com/hsongkr/nerdy-oracle-collab (이 파일, public)

**현재 완료된 Phase:**
- ✅ Phase 1: 블로그 포스팅 안정화
- ✅ Phase 2: 리포트 캐시 DB 영속화
- ✅ 프롬프트 관리 UI (`/settings/prompts`)
- ✅ Phase 3: GNB 탭 + 페이지 분리 + UI 전면 리디자인
- ✅ Phase 3.5: PWA 구현 + 안드로이드 실기기 동작 확인
- ✅ 운영 버그 수정 (2026-05-09): 데이터 로딩 실패 + NOT NULL 오류
- ✅ Phase 3.9-A 운영 안정화: SECRET_KEY 교체, OAuth 로그인 간소화, 프롬프트 저장 오류 수정 (2026-05-12)
- ✅ Phase 3.9-B(일부): 프롬프트 탭 복원, Chrome↔PWA 네비게이션 분리, 보고서 즉시 재생성 UI (2026-05-13)
- ✅ Phase 3.9-D AI뉴스 파이프라인 뼈대: DB 모델 4개 + RSS 수집 + 인사이트 상태 관리 + GNB 활성화 (2026-05-14, Claude Code)
- ✅ Phase 3.9-E AI 인사이트 자동 생성: BeautifulSoup 기사 본문 수집 + Gemini 2.5 Pro 인사이트 초안 생성 + 카드 expand/collapse (2026-05-14, Anti Gravity)
- ✅ Phase 3.9-F: 인사이트 품질 개선 전 STEP 완료 (프롬프트·Flash랭킹·편집 모달, 2026-05-16, Claude Code)
- ✅ Phase 3.9-G: AI News 고도화 전 STEP 완료 (2026-05-16, Claude Code)
  - A: DB 컬럼 추가 (image_url, youtube_url, tickers, is_synthesis, source_*)
  - B: 수집 30건·기간 필터·URL복사박스·건수표시
  - C: 종합분석 생성·편집창 이미지/YouTube/티커/원문·버튼 순서 변경
  - D: /news-analytics sticky 툴바·섹션 스크롤·종합분석 섹션·카드 전면 개선
  - 상세 계획: `Phase_3.9-G_실행계획.md` 참조
- ✅ Phase 3.9-H: AI 뉴스 버그픽스 & RSS 아키텍처 개선 (2026-05-16, Claude Code)
  - 5개 커밋 (ed28a95 → 159dd3a), 상세 내용: `Phase_3.9-H_버그픽스_로그.md` 참조

**현재 작업:**
- 🔜 운영 테스트 후 소규모 배포 (3.9 완료 기준)
- 🔜 포트폴리오 뼈대 (/portfolio CRUD + yfinance)
- 🔜 Phase 4: BYOK 생태계 구축

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
  → 상태: IN PROGRESS / 담당: Anti Gravity + Claude Code

- [2026-05-09] [Claude] guest vs member 권한 범위 확정
  - guest: 시황/리포트 읽기 전용 (생성 불가, 비로그인 유사 계정)
  - member: 무료 10회 열람 + Phase 4에서 BYOK 등록 후 생성 가능
  - Phase 4 구현 시 이 기준으로 권한 분기 처리
  → 상태: DONE / 담당: Claude Code (Phase 4 구현 시 반영)

- [2026-05-09] [Claude] AI뉴스 / 포트폴리오 페이지 Phase 3.9에서 뼈대 구현
  - Phase 3.9: 기본 UI + 데이터 표시 (AI 분석 기능 없음)
  - Phase 4: BYOK 기반 AI 분석 기능 추가
  - AI뉴스(/news-analytics): 3.9-D + 3.9-E로 완료 ✅ (RSS 수집 + Gemini 인사이트 생성)
  - 포트폴리오(/portfolio): 종목/평단가 입력 CRUD + yfinance 수익률 계산 (미착수)
  → 상태: PARTIALLY DONE / 포트폴리오 뼈대 미착수 (담당자 미정)

- [2026-05-09] [Claude] 보고서 프롬프트 개량 방향
  - 현재 프롬프트: 데이터 나열 중심
  - 목표: 핵심 인사이트 중심, 너디 오라클 톤앤매너 반영
  - 톤: 전문 연구원 시각 + 스마트 개미 타깃 + 신뢰감 있고 분석적
  - /settings/prompts UI에서 직접 튜닝 후 Gemini와 함께 검토
  → 상태: IN PROGRESS / 담당: Human(튜닝) + Gemini(검토)

---

## 🔧 구현 현황

> Claude Code, Anti Gravity가 작성. 기획 팀이 읽고 다음 기획에 반영.

- [2026-05-16] [Claude Code] Phase 3.9-H 완료 — AI 뉴스 버그픽스 & RSS 아키텍처 개선
  - **버그 1**: 툴바가 스크롤 시 GNB 뒤로 숨는 문제 → JS로 .app-header 높이 동적 측정, toolbar.style.top 적용
  - **버그 2**: 뉴스 섹션 순서 오류 (dict.items() 순서 → categories 정의 순서 유지로 수정) + {% endif %} 누락
  - **버그 3**: admin_news_pipeline.html에 collectRSS·generateInsight 함수 중복 선언 제거
  - **버그 4**: default_queries 변수 템플릿에 미전달 → app.py 라우트에 추가
  - **기능**: 인사이트 생성 시 top 관련 기사의 source_url/title/name/date 자동 저장
  - **기능**: /news-analytics 카드 — 원문 링크를 제목 바로 아래 항상 노출, 날짜를 원문 발표일 우선 표시
  - **아키텍처**: Google News RSS → 직접 RSS 피드 + 키워드 필터 방식으로 전환
    - 원인: Railway 서버 IP가 Google News RSS에서 차단돼 수집 0건
    - STEP 1 UI: "키워드 필터" + "RSS 피드 URL 목록(줄바꿈 구분)" 두 영역으로 분리
    - 수집 로직: 각 RSS URL 순회 → 키워드 매칭(제목·요약) → 통과 기사만 저장
    - DEFAULT_FEED_URLS: TechCrunch, Ars Technica, The Verge, Reuters, STAT News 등 분야별 큐레이션
  - 251216_portfolio main push 완료 (commits: ed28a95 → fe8808d → 2bc0bd3 → 4610e99 → 159dd3a)
  → 상태: DONE / 운영 테스트 가능

- [2026-05-16] [Claude Code] Phase 3.9-F STEP3 완료 — 인사이트 admin 편집 UX 추가
  - POST /admin/news-pipeline/edit: 제목·본문·유형 수정 저장
  - POST /admin/news-pipeline/delete: 인사이트 삭제
  - admin 파이프라인 화면에 ✏️ 편집 버튼(모달) + 🗑 삭제 버튼 추가
  - 편집 모달: 제목 입력, 유형 드롭다운, 본문 textarea(Markdown), 글자수 카운터, 외부 클릭 닫기
  - 251216_portfolio main 브랜치 push 완료 (commit: f330f9e)
  → 상태: DONE / Phase 3.9-F 전체 완료 — 운영 테스트 후 소규모 배포 가능

- [2026-05-16] [Claude Code] Phase 3.9-F STEP2 완료 — Gemini Flash 뉴스 중요도 분류 추가
  - STEP 2.5 신설: Flash 모델로 수집 기사 10건에 중요도 점수(1~5) + spam 플래그 부여
  - 스팸·어그로 기사 자동 제거, 점수 3 미만 필터링 후 상위 5건만 Pro에 전달
  - Flash 분류 실패 시 자동 fallback(전체 기사 사용) — 운영 안정성 유지
  - 중요도 점수가 context_text에 표시돼 Pro 분석에도 활용
  - 251216_portfolio main 브랜치 push 완료 (commit: 740c61b)
  → 상태: DONE / 다음: STEP3 인사이트 admin 편집 UX 개선 (제목/본문 수정 가능하게)

- [2026-05-16] [Claude Code] Phase 3.9-F STEP1 완료 — 인사이트 품질 개선 1차 배포
  - 기사 컨텍스트 500자 → 1,500자 확대
  - INSIGHT_TYPE_DEFINITIONS 상수 추가 (5개 유형별 한국어 예시)
  - 인사이트 생성 프롬프트 전면 교체: Nerdy Oracle 페르소나 + What/Why/So What 프레임워크 + 500~800자 길이 가이드
  - 시황 보고서 시스템 프롬프트에 애널리스트 톤 추가 (수치 해석 요구, 불확실 표현 금지)
  - DEFAULT_PROMPTS 섹션별 분량 가이드 추가 (200/250/150자)
  - 251216_portfolio main 브랜치 push 완료 (commit: 68b51df)
  → 상태: DONE

- [2026-05-14] [Anti Gravity] 3.9-E AI 인사이트 자동 생성 구현 완료
  - BeautifulSoup 기사 본문 수집 (RSS 수집 시 URL 크롤링, 최대 5,000자, 실패 시 무시)
  - `POST /admin/news-pipeline/generate` — Gemini 2.5 Pro → NewsInsight(draft) 저장
  - STEP 3 버튼 활성화 (`generateInsight` JS 함수 연결)
  - 인사이트 카드 expand/collapse + Markdown 완전 렌더링 (`| markdown | safe`)
  - `beautifulsoup4>=4.12.0` 추가 → Railway auto-deploy 완료
  → 상태: DONE / 3 커밋 push 완료

- [2026-05-14] [Claude Code] 3.9-D AI뉴스 파이프라인 뼈대 구현 완료
  - 신규 DB 테이블 4개: news_queries, news_raw, news_insights, pipeline_runs (SQLAlchemy 모델)
  - STEP별 구현 범위:
    - STEP 1 (검색식): UI 구현, 수동 편집 가능, 자동생성 버튼 비활성
    - STEP 2 (RSS 수집): **완전 구현** (feedparser로 Google News 수집, URL 중복 제거)
    - STEP 3 (AI 인사이트): UI만 구현, 버튼 비활성화, "3.9-E(또는 미래 phase) 예정" 안내
    - STEP 4 (검수 & 발행): **완전 구현** (상태 관리: draft/review/published/youtube_queue)
    - STEP 5 (실행 로그): **완전 구현** (PipelineRun 테이블 기록 — collect/generate/publish)
  - 신규 라우트 6개:
    - GET `/news-analytics` (@approved_required): 회원용 인사이트 게시판, published 인사이트 그룹별 표시
    - GET `/admin/news-pipeline` (@supervisor_required): 관리자 파이프라인 대시보드
    - POST `/admin/news-pipeline/query`: 검색식 저장/수정
    - POST `/admin/news-pipeline/collect`: RSS 수집 트리거 (카테고리별/전체)
    - POST `/admin/news-pipeline/publish`: 인사이트 상태 변경 및 로그 기록
  - GNB "AI뉴스" 탭 활성화 + 햄버거 메뉴 파이프라인 링크 추가
  - 신규 파일: templates/news_analytics.html, templates/admin_news_pipeline.html, migrations/003_news_pipeline.sql
  - 수정 파일: models.py (+4 classes), app.py (+6 routes +3 helpers +constants), requirements.txt (feedparser), templates/layout.html
  → 상태: DONE / commit & push 완료 (5 commits, Railway auto-deploy)

- [2026-05-13] [Claude Code] 보고서 즉시 재생성 기능 추가
  - `/api/admin/regenerate` POST 엔드포인트 (supervisor/owner 전용, 로그인 기반 — CRON_SECRET 불필요)
  - `/settings/prompts` UI 각 카드에 "🔄 지금 재생성" 버튼 추가
  - 업데이트된 프롬프트로 한국/미국 × 3 리포트 타입 = 6개 보고서를 브라우저에서 직접 재생성 가능
  - 수정 파일: app.py, templates/prompts.html
  → 상태: DONE / commit & push 완료

- [2026-05-13] [Claude Code] BUG 2건 추가 수정 완료 (작동 확인)
  - 프롬프트 저장 후 탭 위치 유지: redirect에 ?tab={market} 파라미터 추가, prompts.html에서 복원
  - Chrome/Android 앱 네비게이션 분리: manifest.json scope="/" 추가 + layout.html display-mode 감지 스크립트 + SW 캐시 v3
  - PWA 재설치 후 Chrome↔앱 네비게이션 정상 분리 확인
  → 상태: DONE

- [2026-05-13] [Claude Code] GitHub pull 완료 — 밤사이 Anti Gravity 작업 확인 및 DEV_SHARED_Context 내 삭제된 파일 참조 정리.

- [2026-05-12] [Anti Gravity] 지표 6개로 확대(US 10Y·WTI·PHLX Semi 추가, Uranium 제거), 차트 날짜 표시 추가, 최근 리포트 3개 표시, 핀치줌 비활성화. 문서 Devlopment_Plan·HANDOVER·To-Do_List → DEV_SHARED_Context 통합.

- [2026-05-12] [Claude Code] BUG 2건 수정 (프롬프트 저장 오류, OAuth 로그인 단계 간소화) commit & push 완료.
  - 프롬프트 저장 오류: PromptTemplate 신규 생성 시 section1/2/3/warning_text 기본값('') 명시 (NOT NULL 방어)
  - OAuth 로그인 단계 간소화: 가입(signup) 모드만 consent, 로그인은 select_account로 변경
  → 상태: DONE

- [2026-05-12] [Claude Code] Phase 3.9 개발 환경 준비 완료 (Claude Code 웹 세션)
  → 상태: DONE

- [2026-05-09] [Claude Code] 운영 버그 2건 수정 완료
  - yfinance 병렬화(ThreadPoolExecutor) + NaN 안전처리
  - api_approved_required 신규 데코레이터 + session 임포트 수정
  - section1 NOT NULL 제약 DROP 마이그레이션
  → 상태: DONE

- [2026-05-08] [Claude Code] Phase 3.5 PWA 완료
  → 상태: DONE

- [2026-05-08] [Claude Code] Phase 3 UI 리디자인 완료
  → 상태: DONE

- [2026-05-08] [Claude Code] Phase 2 리포트 DB 영속화 완료
  → 상태: DONE

---

## ❓ 미결 질문 / 블로커

> 누구든 작성 가능. 담당자가 답변 후 RESOLVED 처리.

- [2026-05-13] [Claude Code → Human] 업데이트된 프롬프트로 보고서 재생성 방법
  - `/settings/prompts` 접속 → 각 리포트 카드의 "🔄 지금 재생성" 버튼 클릭
  - 버튼 클릭 시 백그라운드에서 새 보고서 생성 시작 (약 30~60초 소요)
  - 한국 3개 + 미국 3개 = 총 6번 클릭 필요
  - 단, Railway IP 제한으로 Claude Code 웹 세션에서는 직접 확인 불가 — 브라우저에서 직접 클릭
  → 상태: OPEN / 담당: Human (버튼 클릭)

- [2026-05-13] [Human → 개발팀] 커스텀 도메인 등록 필요 (Railway URL 노출 문제)
  - Google OAuth 계정 선택 화면에 `251216portfolio-production.up.railway.app` 노출
  - 코드로 해결 불가 → 도메인 구매 + Railway/DNS/OAuth 설정 필요
  → 상태: OPEN / 담당: Human (직접 처리)

- [2026-05-13] [Human → 개발팀] Google OAuth 앱 게시 신청
  - "Google에서 확인하지 않은 앱" 경고 제거를 위해 "테스트→프로덕션" 전환 필요
  - Google Cloud Console → OAuth 동의 화면 → 앱 게시
  → 상태: OPEN / 담당: Human (직접 처리)

---

## 📓 세션 로그

> 각 AI/사람이 작업 시작·종료 시 한 줄 기록. 최신이 위.

- [2026-05-16] [Claude Code] Phase 3.9-H 완료. 툴바 위치·섹션 순서 버그픽스, 원문 링크·날짜 카드 개선, RSS 수집 아키텍처 직접 피드+키워드 필터로 전환. 5개 커밋 push (ed28a95→159dd3a).
- [2026-05-16] [Claude Code] Phase 3.9-G 전 STEP 완료 (A→D). DB 마이그레이션·수집 개선·종합분석·편집창 확장·news-analytics 전면 개선. 251216_portfolio 4개 커밋 push (ef63358→eec6254).
- [2026-05-16] [Claude Code] Phase 3.9-F STEP3 완료. 인사이트 편집 모달 + 삭제 버튼 추가. 251216_portfolio push 완료 (commit: f330f9e). Phase 3.9-F 전 STEP 완료.
- [2026-05-16] [Claude Code] Phase 3.9-F STEP2 완료. Flash 뉴스 중요도 분류(1~5점 + spam 필터) 추가. 10건→상위5건 선별 후 Pro 전달. 251216_portfolio push 완료 (commit: 740c61b).
- [2026-05-16] [Claude Code] Phase 3.9-F STEP1 구현 완료. 인사이트 프롬프트 전면 개선 + 시황 보고서 톤 강화. 251216_portfolio push 완료.
- [2026-05-16] [Claude Code] 기획 문서 2개 생성 → nerdy-oracle-collab main 업로드: `AI_News_개선계획_20260515.md`, `작업확인_AntiGravity_20260515.md`
- [2026-05-15] [Claude Code] Anti Gravity 3.9-E 커밋 확인 + git pull 완료. DEV_SHARED_Context 양쪽(portfolio + nerdy-oracle-collab) 업데이트. next: 3.9-C UI 정리 또는 포트폴리오 뼈대 착수 (담당자 협의).
- [2026-05-14] [Anti Gravity] 3.9-E 완료 (기사 본문 수집, Gemini 인사이트 생성, 카드 expand/collapse, Markdown 렌더링). 3개 커밋 push.
- [2026-05-14] [Claude Code] 3.9-D 완료 검증. 4개 테이블·5개 라우트·템플릿·RSS 검증.
- [2026-05-13] [Claude Code] 보고서 즉시 재생성 기능 구현 완료. /api/admin/regenerate 엔드포인트 + prompts.html 버튼 추가. Railway 배포 후 브라우저에서 버튼 클릭으로 6개 보고서 재생성 가능.
- [2026-05-13] [Claude Code] BUG 2건 추가 수정 완료 (작동 확인)
  - 프롬프트 저장 후 탭 위치 유지: redirect에 ?tab={market} 파라미터 추가, prompts.html에서 복원
  - Chrome/Android 앱 네비게이션 분리: manifest.json scope="/" 추가 + layout.html display-mode 감지 스크립트 + SW 캐시 v3
  - PWA 재설치 후 Chrome↔앱 네비게이션 정상 분리 확인
- [2026-05-13] [Claude Code] GitHub pull 완료 — 밤사이 Anti Gravity 작업 확인 및 DEV_SHARED_Context 내 삭제된 파일 참조 정리.
- [2026-05-12] [Anti Gravity] 지표 6개로 확대(US 10Y·WTI·PHLX Semi 추가, Uranium 제거), 차트 날짜 표시 추가, 최근 리포트 3개 표시, 핀치줌 비활성화. 문서 Devlopment_Plan·HANDOVER·To-Do_List → DEV_SHARED_Context 통합.
- [2026-05-12] [Claude Code] BUG 2건 수정 (프롬프트 저장 오류, OAuth 로그인 단계 간소화) commit & push 완료.
- [2026-05-12] [Claude Code] Phase 3.9 개발 환경 준비 완료. Railway CLI 설치, Python venv, 로컬 앱 실행 확인.
- [2026-05-09] [Claude] Phase 3.9 안정화 단계 기획 결정사항 작성. guest/member 권한 확정. AI뉴스/포트폴리오 뼈대 구현 방향 결정.
- [2026-05-09] [Claude Code] 운영 버그 2건 수정 완료. 토큰 쿼타 소진으로 퇴근. 다음: Phase 3.9 (Anti Gravity 담당).
- [2026-05-08] [Human] Phase 3.5 PWA 안드로이드 실기기 확인 완료.
- [2026-05-08] [Claude Code] Phase 3.5 PWA 완료 + 햄버거 메뉴 z-index 버그 수정 + 보고서 1일 차트 변경.
- [2026-05-08] [Claude Code] DEV_SHARED_Context.md 초기 생성.
- [2026-05-08] [Human] 5인 협업 채널 구성 요청 → DEV_SHARED_Context.md 방식 채택.
