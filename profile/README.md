# 신용평가 AI 규제준수 자동감사 플랫폼(FinAudit)
<img width="1012" height="213" alt="image" src="https://github.com/user-attachments/assets/024e4193-5f12-4f30-a97a-d8f8367dcc9d" />

**AI 기본법이 2026년 2월에 개정**되고, **금융 7대 가이드라인이 2026년 6월에 만들어지면서** 신용평가 AI가 이를 위반하는지 확인하는 작업은 새로운 숙제로 떠오르게 되었습니다 <br>
금융기관이 신용평가에 AI를 쓰기 시작하면서, **이 모델이 특정 집단을 차별하지 않는다**는 걸 증명하는 일이 새로운 숙제가 됐습니다.
담당자가 데이터를 직접 뜯어보고, 법령 조항을 하나하나 대조하고, 보고서를 손으로 작성하던 과정을 통째로 자동화한 것이 **FinAudit**입니다.

모델과 감사 데이터를 올리기만 하면, FinAudit이 대신 물어봐 줍니다
- 이 모델은 성별·연령 같은 **보호 속성에 대해 공정하게 판단**하고 있는지
- **왜 이런 결과가 나왔는지 설명**할 수 있는지
- **AI 기본법이 요구하는 조항들을 실제로 충족**하고 있는지
- 분석이 끝나면 감사 보고서와 규제준수 판정서가 자동으로 만들어지고, 고객이 자신의 신용평가 결과에 이의를 제기하면 그 대응 문서 초안까지 함께 준비해 줍니다.<br>

한마디로 FinAudit은 "AI가 내린 결정을 사람이 설명할 수 있게" 만들어주는 플랫폼입니다. <br>
**고영향 AI 여부를 미리 진단하는 것부터, 감사 실행, 규제 자가점검, 보고서 생성, 이의제기 대응문서 초안 생성, 운영 모니터링**까지 <br>
신용평가 AI를 규제 앞에 떳떳하게 세우는 데 필요한 전 과정을 하나의 화면 흐름으로 이어갑니다.

<br><br>

## 시스템 아키텍처
<img width="1731" height="908" alt="SW 아키텍쳐" src="https://github.com/user-attachments/assets/e2c2eaed-12bb-4453-b527-7390defd02ea" />

FinAudit은 역할이 뚜렷하게 나뉜 세 개의 서버가 함께 움직입니다.

사용자가 마주하는 화면은 **프론트엔드**가 맡습니다.
- 모델을 업로드하고, 감사가 진행되는 과정을 지켜보고, 완성된 보고서를 내려받는 모든 화면이 여기서 만들어집니다.
- 프론트엔드는 화면을 그리는 데만 집중할 뿐, 실제 판단은 전혀 하지 않습니다.
<br>

모든 요청은 **백엔드**를 거칩니다.
- 회원 인증은 물론, 어떤 모델이 등록되어 있는지,
- 감사가 어디까지 진행됐는지,
- 어떤 법령 조항을 충족했는지 같은 데이터를 기록하고 관리하는 것도 백엔드의 몫입니다.
<br>

그리고 실제로 숫자를 계산하는 일은 **AI 서버**가 전담합니다.
- 백엔드로부터 모델과 데이터를 넘겨받으면, 이 AI 서버가 공정성 지표를 계산하고, 모델이 왜 그런 판단을 내렸는지 설명 가능성을 분석하고, 그 결과를 바탕으로 보고서 문장까지 작성합니다.
- 사용자나 프론트엔드가 AI 서버를 직접 호출하는 일은 없습니다 -> 항상 백엔드가 중간에서 필요한 만큼만 요청을 보내고 결과를 받아옵니다.
<br>

이렇게 화면(프론트엔드) · 데이터와 업무 흐름(백엔드) · 실제 분석(AI 서버)을 분리해둔 덕분에, 세 팀이 서로의 코드를 건드리지 않고도 **각자의 영역에서 동시에 개발을 진행**할 수 있었습니다.

<br><br>

## 개발 스택

<table width="100%">
<thead><tr><th width="110">영역</th><th width="140">구분</th><th>기술</th></tr></thead>
<tbody>
<tr><td rowspan="11">프⁠론⁠트⁠엔⁠드</td><td>언⁠어</td><td>TypeScript</td></tr>
<tr><td>프⁠레⁠임⁠워⁠크</td><td>React 19</td></tr>
<tr><td>빌⁠드⁠ ⁠도⁠구</td><td>Vite</td></tr>
<tr><td>U⁠I⁠ ⁠라⁠이⁠브⁠러⁠리</td><td>Ant Design</td></tr>
<tr><td>라⁠우⁠팅</td><td>React Router</td></tr>
<tr><td>서⁠버⁠ ⁠상⁠태⁠ ⁠관⁠리</td><td>TanStack Query</td></tr>
<tr><td>클⁠라⁠이⁠언⁠트⁠ ⁠상⁠태⁠ ⁠관⁠리</td><td>Zustand</td></tr>
<tr><td>폼</td><td>React Hook Form + Zod</td></tr>
<tr><td>H⁠T⁠T⁠P⁠ ⁠클⁠라⁠이⁠언⁠트</td><td>Axios</td></tr>
<tr><td>인⁠증</td><td>JWT (Access / Refresh)</td></tr>
<tr><td>린⁠트⁠/⁠포⁠맷</td><td>ESLint + Prettier</td></tr>
<tr><td rowspan="13">백⁠엔⁠드</td><td>언⁠어</td><td>Java 21</td></tr>
<tr><td>프⁠레⁠임⁠워⁠크</td><td>Spring Boot 4.0.7 (Web MVC, Data JPA, Security, Validation)</td></tr>
<tr><td>데⁠이⁠터⁠베⁠이⁠스</td><td>PostgreSQL (+ pgvector)</td></tr>
<tr><td>마⁠이⁠그⁠레⁠이⁠션</td><td>Flyway</td></tr>
<tr><td>캐⁠시</td><td>Redis</td></tr>
<tr><td>인⁠증</td><td>JWT (jjwt)</td></tr>
<tr><td>스⁠토⁠리⁠지</td><td>AWS S3 (로컬은 MinIO)</td></tr>
<tr><td>A⁠P⁠I⁠ ⁠문⁠서⁠화</td><td>springdoc-openapi (Swagger UI)</td></tr>
<tr><td>모⁠니⁠터⁠링</td><td>Micrometer, Prometheus, Grafana</td></tr>
<tr><td>테⁠스⁠트</td><td>JUnit 5, Mockito, Testcontainers</td></tr>
<tr><td>부⁠하⁠ ⁠테⁠스⁠트</td><td>k6</td></tr>
<tr><td>빌⁠드⁠ ⁠도⁠구</td><td>Gradle</td></tr>
<tr><td>C⁠I⁠/⁠C⁠D⁠ ⁠&⁠ ⁠인⁠프⁠라</td><td>GitHub Actions, Docker, Docker Hub, AWS EC2</td></tr>
<tr><td rowspan="9">A⁠I⁠ ⁠서⁠버</td><td>언⁠어</td><td>Python 3.13</td></tr>
<tr><td>프⁠레⁠임⁠워⁠크</td><td>FastAPI, Uvicorn</td></tr>
<tr><td>데⁠이⁠터⁠ ⁠검⁠증⁠/⁠설⁠정</td><td>Pydantic v2</td></tr>
<tr><td>H⁠T⁠T⁠P⁠ ⁠클⁠라⁠이⁠언⁠트</td><td>httpx</td></tr>
<tr><td>L⁠L⁠M⁠ ⁠연⁠동</td><td>OpenAI 호환 Chat Completions API</td></tr>
<tr><td>객⁠체⁠ ⁠스⁠토⁠리⁠지</td><td>boto3 (AWS S3 / MinIO)</td></tr>
<tr><td>머⁠신⁠러⁠닝</td><td>XGBoost, Fairlearn, SHAP, LIME, scikit-learn</td></tr>
<tr><td>리⁠포⁠트⁠ ⁠생⁠성</td><td>Jinja2, Playwright(PDF 변환), python-docx</td></tr>
<tr><td>테⁠스⁠트</td><td>pytest</td></tr>
</tbody>
</table>

<br><br>

## 웹사이트 화면
실제 서비스 화면은 아래 문서에서 확인할 수 있습니다.

**[📸 웹사이트 화면 전체 보기 →](../docs/SCREENSHOTS.md)**


<br><br>

## 팀원 정보

| Name | GitHub | 만든 기능 |
|---|---|---|
| 김⁠다⁠애 | [<img src="https://github.com/fluidicon.png" width="16"/> a⁠s⁠i⁠a⁠s⁠y⁠o⁠u⁠a⁠s⁠i⁠a⁠s](https://github.com/asiasyouasias) | • 백엔드 프로젝트 초기 구성 및 모델·감사 파일 업로드 API(fairlearn 업로드, 검증·메타데이터, 버전 계열 관리, 임계값 설정)<br>• 법령 조항 pgvector 유사도 검색·임베딩 파이프라인, AI 기본법 조문 스키마·시딩 데이터<br>• 자가점검-법령 조항 자동 매핑 서비스(RAG) 및 매핑 조회 API<br>• AI 기본법 개정 자동 추적(law.go.kr 연동)·사용자 알림<br>• 감사 이력·모델 목록 조회, 데이터셋 재사용, 감사 취소·재시도(동시성 처리), 고아 감사 자동 실패 처리, 감사 실패 인앱 알림 |
| 김⁠다⁠진 | [<img src="https://github.com/fluidicon.png" width="16"/> D⁠a⁠j⁠i⁠n⁠-⁠0⁠1](https://github.com/Dajin-01) | • AI 서버 SHAP 분석 파이프라인 연동(FastAPI 연동, 비동기 실행·상태관리, REVIEW 판정, 결과 조회 API)<br>• 설명가능성·고영향 AI 사전진단 리포트를 HTML·PDF·Word로 생성해 저장·조회·다운로드까지 전 구간 연동<br>• 이의제기 대응문서 전역 SHAP·LLM 연동, SHAP WARNING 상태 표시<br>• 감사 결과 지표 도움말 툴팁, 헤더 로고·회원탈퇴 팝업 등 UI 개선 |
| 김⁠석⁠주 | [<img src="https://github.com/fluidicon.png" width="16"/> B⁠a⁠n⁠a⁠n⁠a⁠-⁠b⁠o⁠s⁠s](https://github.com/Banana-boss) | • 로그인·회원가입 페이지, 자동로그인·권한 체계 등 인증 화면 초기 구현<br>• 규제 자가점검(STEP4) 21문항·7그룹 아코디언 UI와 백엔드 확장(5→21문항, YES/NO/NA 응답), 건너뛰기 모달까지 전 구간 구현<br>• 모델 업로드·감사 실행 화면(신규/버전업 플로우), 감사 세부정보·산출물 관리 페이지<br>• 메인페이지·기능 살펴보기 페이지, 홈 화면 감사 카드, 마이페이지 구현<br>• SHAP 전역 피처 중요도 조회 API, Redis 캐시 역직렬화 오류 수정, 개선권고가이드 섹션 분리 |
| 오⁠희⁠주 | [<img src="https://github.com/fluidicon.png" width="16"/> m⁠o⁠m⁠i⁠j⁠u](https://github.com/momiju) | • AI 서버(FastAPI) 프로젝트 초기 세팅<br>• 회원가입·로그인 인증 플로우 전체 구현(이메일 인증 발송·검증, 비밀번호 찾기·재설정, 자동로그인(RTR), reCAPTCHA)<br>• 최종 감사 보고서 생성 파이프라인(OpenAI LLM 클라이언트, 프롬프트 빌더·템플릿) 및 생성·다운로드 API<br>• 플로팅 챗봇 UI, 이의제기 대응문서 페이지<br>• 인증·감사 화면 UX 개선(비밀번호 표시·숨김, 로딩 스피너, 보고서 카드 레이아웃, CSV 업로드 검증 도움말, 자가점검 건너뛰기 UI·버튼 비활성화 등)<br>• 인증·사용자 도메인 단위·통합 테스트 및 k6 시나리오 작성 |
| 정⁠민⁠호 | [<img src="https://github.com/fluidicon.png" width="16"/> C⁠M⁠I⁠N⁠O⁠9⁠9](https://github.com/CMINO99) | • 고영향 AI 사전진단 판정 API 구현(백엔드)<br>• 고영향 AI 사전진단 페이지 구현 및 플로우 개발(프론트엔드)<br>• 대시보드 집계 API, 감사 내역 전체조회·모델 버전 표시 API 구현(백엔드)<br>• 대시보드 화면 구현(감사 내역 더보기/접기, 표시 정보·문구 개선)(프론트엔드) |
| 조⁠영⁠웅(PM) | [<img src="https://github.com/fluidicon.png" width="16"/> J⁠o⁠J⁠i⁠m⁠i](https://github.com/JoJimi) | • 백엔드 프로젝트 초기 세팅(ERD 기반 JPA 엔티티, 전역 예외 처리, 프론트-백엔드 연동, PR/이슈 템플릿, README 아키텍처 문서화)<br>• JWT 인증(Redis 토큰 관리·RTR) 및 모니터링(node-exporter) 인프라 구축, 패키지 구조 리팩토링<br>• 게시판(공지 고정·관리자 댓글·첨부파일)과 마이페이지(프로필·비밀번호 변경·회원탈퇴)를 백엔드부터 프론트까지 구현<br>• 알림센터 인프라(감사완료·법령개정 알림, 알림벨 UI, 알림 초기화) 구축<br>• 감사 실행 플로우 UI(모델·데이터셋 업로드, 민감변수 선택, Validation 데이터셋), 챗봇 UI/응답 개선<br>• 공정성 지표 확장(Proportional/FDR/FOR/FPR Parity), SHAP·Fairlearn 병렬 실행 전환<br>• Redis 캐싱, k6 부하테스트, MinIO 로컬 환경, Swagger UI 등 성능·개발환경 정비 |
| 황⁠민⁠서 | [<img src="https://github.com/fluidicon.png" width="16"/> M⁠i⁠n⁠s⁠e⁠o⁠H⁠w⁠a⁠n⁠g](https://github.com/MinseoHwang) | • React 19 + Vite 프론트엔드 프로젝트 초기 세팅<br>• XGBoost 위험점수·승인임계값 산출, 감사 입력파일 검증 등 감사 분석 기반 로직<br>• Fairlearn 공정성 지표 계산 엔진(7종) 및 공정성 결과 조회·저장 API(S3 연동, 스키마 확장)<br>• OpenAI 호환 LLM 연동(AI 서버 클라이언트), SHAP 설명가능성 HTML 리포트 생성<br>• 편향진단·설명가능성·규제준수판정서·개선권고가이드·최종감사보고서 5종 리포트 생성·PDF/Word 변환과 저장·조회·다운로드 API<br>• 감사 질의응답 챗봇(대화 스키마·API, RAG 법령 인용, 리포트 서술 근거 연동)<br>• 감사 결과 버전 비교, 보고서 병렬 사전생성 등 프론트 개선<br>• board·dashboard·diagnosis 도메인 단위·통합 테스트 및 k6 시나리오 작성 |
