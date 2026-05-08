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
[YYYY-MM-DD] [작성자] 내용
상세 내용 (필요시) → 상태: OPEN / IN PROGRESS / DONE


---

## 📌 현재 프로젝트 컨텍스트

**프로젝트:** Nerdy Oracle — 글로벌 마켓 리포트 자동화 시스템
**운영 URL:** https://251216portfolio-production.up.railway.app
**GitHub(개발):** https://github.com/hsongkr/251216_portfolio (main 브랜치, private)
**GitHub(협업):** https://github.com/hsongkr/nerdy-oracle-collab (이 파일, public)
**개발 로드맵:** development_plan.md (v2.4) — 개발 레포 참조
**상세 인수인계:** HANDOVER.md — 개발 레포 참조

**현재 완료된 Phase:**
- ✅ Phase 1: 블로그 포스팅 안정화
- ✅ Phase 2: 리포트 캐시 DB 영속화
- ✅ 프롬프트 관리 UI (/settings/prompts)

**다음 작업:**
- 🔜 Phase 3: GNB 사이드바 + 페이지 분리 (/market/kr, /market/us)

---

## 🎯 기획 결정사항

> Gemini, Claude 기획자가 작성. 구현 팀(Claude Code, Anti Gravity)이 읽고 실행.

_(아직 항목 없음)_

---

## 🔧 구현 현황

> Claude Code, Anti Gravity가 작성. 기획 팀이 읽고 다음 기획에 반영.

- [2026-05-08] [Claude Code] Phase 2 완료 — Report 모델로 리포트 DB 저장
  - models.py: Report (content, is_posted, posted_at), PromptTemplate (prompt_body, schedule_hour/minute)
  - app.py: generate_report() → DB 저장, /api/report/<market> → DB 조회
  → 상태: DONE

- [2026-05-08] [Claude Code] 프롬프트 관리 UI 완료
  - /settings/prompts 라우트 (supervisor/owner 전용)
  - KR/US 탭 × 3 리포트 타입 = 6개 카드, 단일 텍스트박스 + HH:MM 시간 설정
  - {title_base} 플레이스홀더 지원
  → 상태: DONE

---

## ❓ 미결 질문 / 블로커

> 누구든 작성 가능. 담당자가 답변 후 RESOLVED 처리.

_(아직 항목 없음)_

---

## 📓 세션 로그

> 각 AI/사람이 작업 시작·종료 시 한 줄 기록. 최신이 위.

- [2026-05-08] [Claude Code] nerdy-oracle-collab 공개 레포 셋업 완료. Gemini/Claude 기획자 접근 가능.
- [2026-05-08] [Claude Code] DEV_SHARED_Context.md 초기 생성. Phase 2 + 프롬프트 관리 UI 완료. 다음 작업자는 Phase 3 GNB 착수 예정.
- [2026-05-08] [Human] 5인 협업 채널 구성 요청 → DEV_SHARED_Context.md 방식 채택.
