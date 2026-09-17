# 김정훈 · JeongHun Kim

**Problem Solving · AI Products**

사용자의 불편을 구체적인 문제로 좁히고, AI와 소프트웨어로 해결책을 구현합니다.  
정보를 찾고 비교하고 문서로 옮기는 과정을, 판단과 실행으로 이어지는 제품 흐름으로 바꾸는 데 집중합니다.

[Email](mailto:wjdgnsdl213@gmail.com) · Computer Engineering

## 문제에서 출발한 프로젝트

| 프로젝트 | 풀고 있는 문제 | 구현한 흐름 |
| :--- | :--- | :--- |
| **[국캐치 · Gukcatch](https://github.com/wjdgnsdl213/Gukcatch)** | 여러 국회 중계에서 기관 관련 발언을 찾아 보고서로 정리하는 반복 업무 | 실시간 감시 → 키워드 알림 → 질의·답변 보고 초안 |
| **[팜바톤 · FarmBaton](https://github.com/wjdgnsdl213/FarmBaton)** | 농장 승계에 필요한 가격·소득·조건 정보를 함께 비교하기 어려운 문제 | 공공데이터 → 인수 검토 리포트 → 조건 기반 매칭 |
| **[견적서 작성기](https://github.com/wjdgnsdl213/information_organization)** | 말이나 문자로 받은 요청을 견적 항목과 문서로 옮기는 작업 | 자연어·음성 → AI 추출 → 사용자 검토 → HWPX·PDF |

<details>
<summary><strong>01 · 국캐치 — 업무 맥락에서 해결할 문제를 좁히기</strong></summary>

국회 사이트가 이미 회의 종료 후 자막 전문을 제공한다는 점에서 출발했습니다.
프로젝트의 초점은 **실시간으로 관련 발언을 선별하고, 보고 초안까지 연결하는 것**입니다.

- **사용자 맥락:** 중계를 들으며 다른 업무를 병행해야 하는 담당자의 흐름을 [문제 정의 문서](https://github.com/wjdgnsdl213/Gukcatch/blob/master/ROADMAP.md)에 정리했습니다.
- **AI가 필요한 지점:** 여러 질의에 한꺼번에 답하는 회의의 문맥을 구간 판정에 사용하고, 관련 질의만 요약하는 [2단계 파이프라인](https://github.com/wjdgnsdl213/Gukcatch/blob/master/src/report-run.js)을 구현했습니다.
- **검토 가능한 결과:** 모델이 반환한 원문 줄 범위의 누락·중첩을 [코드로 검산](https://github.com/wjdgnsdl213/Gukcatch/blob/master/src/segment.js)합니다. 이 검산은 원문 포함 범위에 대한 확인이며, 요약의 정확도를 보장하지 않습니다.

`Node.js` · `Playwright` · `Claude API` · `Electron`

</details>

<details>
<summary><strong>02 · 팜바톤 — 비교·매칭을 구현하고 데이터로 가정 점검하기</strong></summary>

고령 농가와 청년농이 승계를 검토할 수 있도록 농장 정보를 리포트와 매칭 목록으로 정리합니다.
농장주와 인수 희망자의 서로 다른 관점을 제품 흐름에 반영했습니다.

- **판단 근거:** [공공데이터 기반 산식](https://github.com/wjdgnsdl213/FarmBaton/blob/main/docs/formula.md)으로 수치를 계산하고, [AI는 설명문 생성](https://github.com/wjdgnsdl213/FarmBaton/blob/main/backend/app/services/report_ai.py)에 사용합니다.
- **검증과 수정:** [토지 기준가 백테스트](https://github.com/wjdgnsdl213/FarmBaton/blob/main/docs/backtest_land_report.md)에 집계 방식의 변경과 전방 검증 결과를 기록했습니다. 검증 범위는 토지 기준가이며, 시설·영업권을 포함한 전체 인수 가치와 구분합니다.
- **응답 시간과 비용:** [매칭 목록](https://github.com/wjdgnsdl213/FarmBaton/blob/main/backend/app/routers/young_farmers.py)은 정해진 설명문으로 제공하고, PDF 요청 시 AI 설명을 생성·캐싱하도록 나눴습니다.

[화면·실행 안내](https://github.com/wjdgnsdl213/FarmBaton#readme) · [시연 영상](https://youtu.be/76HELiqfSxM)

`React` · `FastAPI` · `PostgreSQL / PostGIS` · `Claude API`

</details>

<details>
<summary><strong>03 · 견적서 작성기 — 작은 업무 흐름을 끝까지 연결하는 MVP</strong></summary>

문자나 한국어 음성으로 받은 요청에서 거래처·품목·수량·단가를 추출하고,
사용자가 초안을 수정한 뒤 실제 문서로 내려받는 MVP입니다.

- **입력 구조화:** [JSON Schema 기반 AI 추출](https://github.com/wjdgnsdl213/information_organization/blob/main/src/openrouter.js)로 비정형 요청을 편집 가능한 항목으로 바꿉니다.
- **역할 분리:** AI는 항목을 추출하고, 사용자는 초안을 확인하며, [서버는 입력 검증과 금액 계산](https://github.com/wjdgnsdl213/information_organization/blob/main/src/quote.js)을 담당합니다.
- **업무 완료까지:** [HWPX](https://github.com/wjdgnsdl213/information_organization/blob/main/src/hwpx.js)·[PDF](https://github.com/wjdgnsdl213/information_organization/blob/main/src/pdf.js) 생성과 [검증 테스트](https://github.com/wjdgnsdl213/information_organization/tree/main/test)를 함께 확인할 수 있습니다.

`JavaScript` · `Node.js` · `OpenRouter` · `Web Speech API`

</details>

## 함께 볼 프로젝트

**[퀀트 백테스트 대시보드](https://github.com/wjdgnsdl213/quant_backtester)**  
자연어로 전략을 만들고 과거 데이터로 비교하는 개인용 도구입니다.
[AI 출력](https://github.com/wjdgnsdl213/quant_backtester/blob/main/apps/engine/ai.py)을
[전략 스키마](https://github.com/wjdgnsdl213/quant_backtester/blob/main/apps/engine/dsl/schema.py)로 검증한 뒤 실행하는 구조를 담았습니다.
