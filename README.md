<!-- Byeongheon Jang (장병헌) · github.com/jang961111-hash -->
<!-- 이 레포의 README는 광고가 아니라 근거다. 배지보다 실측을 우선한다. -->

# 장병헌 (Byeongheon Jang) — AI 서비스 개발자

AI 코딩 에이전트로 빠르게 구현하고, 재현 테스트와 실측으로 그 구현을 검증·보완하는 백엔드 중심 개발자입니다. 대표작 **Ops Sentinel**은 동시 150건 부하에서 500 오류가 372건 나던 구조를, 커넥션 풀을 다시 기본값(10)으로 되돌린 채로 **성공 450/450**까지 고쳤습니다 — 아래 [대표작](#대표작)에 측정 조건을 그대로 적었습니다.

[Portfolio](https://jang961111-hash.github.io) · [LinkedIn](https://www.linkedin.com/in/byeongheon-jang-ai-pm/) · [Email](mailto:jang961111@gmail.com)

---

## About

전남대에서 철학을 공부하며 문제를 풀기 전에 구조부터 세우는 법을 배웠고, **SSAFY 14기**(삼성 청년 SW·AI 아카데미, 2025.07~2026.06 수료, SW·AI 교육 1,628시간)에서 그 구조를 직접 구현해 배포하는 법을 배웠습니다. 지금은 **SKALA 4기**(SK AI Leader Academy, 운영사 SK AX, 2026.07~ 재학)에서 백엔드·AI 엔지니어링 실력을 쌓고 있습니다.

문제 정의부터 배포까지 혼자 끝까지 가져가되, 완성 후에는 스스로를 의심합니다. 제출 당시의 수치 주장을 재측정해서 틀린 것은 틀렸다고 README에 다시 씁니다.

---

## 대표작

네 프로젝트 모두 코드는 Claude Code(AI 코딩 에이전트)가 짧은 세션 안에 작성했습니다. 아래 수치는 전부 제가 사후에 직접 설계·실행한 재현 테스트와 부하 측정 결과입니다.

### [Ops Sentinel](https://github.com/jang961111-hash/ops-sentinel) — 지표 이상을 규칙엔진으로 판정하는 Spring Boot API
가상 인프라 지표가 임계치를 넘으면 규칙엔진이 심각도·조치를 결정론적으로 정하고, 비관적 락으로 사건 중복 생성을 막고, 모든 판단을 AOP 감사로그로 남깁니다. 제출 당시 "커넥션 풀 30→60 증설로 해결"은 증상 완화였음을 재측정으로 확인했고, 락 안의 AI 호출과 트랜잭션 교착을 구조적으로 고친 뒤에는 **풀을 기본값 10으로 되돌려도 동시 150건에서 201 450/450 · 감사 450/450**(AI 응답 지연 3초 주입, 2026-09-28 재측정)입니다.
`Java 21 · Spring Boot 3 · JPA/MyBatis · H2/PostgreSQL · OpenAI`

### [장보고 (JangBogo)](https://github.com/jang961111-hash/jangbogo) — AI 에이전트용 결제 게이트웨이 (Solana devnet)
AI 구매 에이전트가 결제하려 할 때, **가맹점 쪽 결정론 정책 엔진**이 위임장(예산·범위·만료)과 온체인 지불(devnet, SPL 토큰)을 검증한 뒤에만 주문을 확정합니다. 같은 결제 증빙을 동시에 여러 번 보내면 주문이 최대 10건 생기던 경합 결함을, 검증 전 선점 + 기록 직전 재검사로 고쳐 **1건**으로 만들었습니다(검증 지연 650ms 주입, 동시 K=1·2·5·10 × 5회, 2026-09-28 재측정).
`Next.js · TypeScript · React · Solana devnet · Gemini`

### [RE:RUN](https://github.com/jang961111-hash/rerun-self-healing-workflow) — AI가 제안하고 사람이 승인해야 재실행되는 자가수정 워크플로
데이터 계약(zod) 검증에 실패하면 실제 LLM(gpt-4.1-mini)이 원인과 수정 프롬프트를 제시하고, **사람이 diff를 승인해야만** 실패 단계부터 다시 실행됩니다. 진단 프롬프트에서 정답 방향 예시 3줄만 지웠더니 올바른 자가수정 성공률이 **72% → 0%**로 무너지는 것을 사전 등록한 실험으로 확인했습니다(조건당 n=25, Fisher 양측 p<0.0001, 2026-09-28). "AI가 스스로 고쳤다"는 원래 주장을 스스로 반증해 README 맨 앞에 그대로 공개했습니다.
`Next.js · TypeScript · React · OpenAI`

### [skala-argus](https://github.com/jang961111-hash/skala-argus) — 반도체 설비 부품 교체 승인 워크플로 (개인 재구현)
SKALA 5인 팀 미니프로젝트에서 발표·백엔드·팀 간 가교를 맡았고(팀 공식 산출물은 별도 Spring Boot 레포), 이 레포는 같은 기간에 FastAPI + Vue 3로 병렬로 만든 개인 백업 구현입니다. 요청번호 채번을 문자열로 비교해 **하루 1,001번째 요청부터 등록이 전부 500 오류**가 나던 결함을 사후 재측정 중 발견해 고쳤습니다(750건 추가 등록 테스트로 재현·검증, 2026-09-28).
`Python · FastAPI · Vue 3 · SQLAlchemy · SQLite/PostgreSQL`

---

## 일하는 방식

네 프로젝트 모두 같은 순서로 사후 보완했습니다.

1. **재현 테스트 먼저** — 결함을 고치기 전에 실패하는 테스트부터 커밋합니다.
2. **수정** — 원인을 구조적으로 고칩니다(증상 완화와 구분).
3. **독립 리뷰** — 작성한 세션과 분리된 리뷰(별도 에이전트)를 거칩니다. 리뷰가 수정이 만든 새 퇴행을 잡은 적도 있습니다(장보고 PR #1).
4. **실측 전후 비교** — 같은 조건으로 수정 전/후 수치를 다시 재서 표로 남깁니다.
5. **과장 정정** — 제출·발표 당시의 부풀린 주장을 찾으면 근거와 함께 README에 다시 씁니다.

코드 대부분은 AI 에이전트가 짧은 세션 안에 씁니다. 제가 하는 일은 문제 범위와 설계 원칙을 정하고, 에이전트를 지시·검증하고, 그 결과가 사실인지 직접 실측해 보완하는 것입니다. 이 과정을 숨기지 않는 것이 강점이라고 생각합니다 — 각 레포 README의 "먼저 읽기" 섹션에 그대로 적어 두었습니다.

---

## 스택

대표작 4개에서 실제로 쓴 것만 적습니다.

| 분류 | 스택 |
|---|---|
| Backend | Java 21 · Spring Boot 3 · FastAPI (Python) · Next.js API Routes |
| Frontend | React 19 · Next.js 15/16 · Vue 3 + Vite |
| Data | PostgreSQL · H2 · SQLite |
| AI API | OpenAI (gpt-4o-mini, gpt-4.1-mini) · Google Gemini |
| Blockchain | Solana devnet (SPL Token) |
| Test / CI | JUnit5 + JaCoCo · pytest + coverage · vitest + Playwright · GitHub Actions |

---

## Now

- **SKALA 4기** AI 서비스 개발 트랙 진행 중
- 대표작 4개의 사후 보완 PR을 머지하고, skala-argus의 실제 LLM(Claude) 판정 에이전트를 마저 구현하는 중입니다
- 연락은 [Email](mailto:jang961111@gmail.com) · [LinkedIn](https://www.linkedin.com/in/byeongheon-jang-ai-pm/)로 주세요

<br/>

<div align="center">
<sub>전체 프로젝트는 <a href="https://jang961111-hash.github.io">포트폴리오</a>에 있습니다.</sub>
</div>
