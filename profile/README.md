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

### 프론트엔드
| 구분 | 기술 |
|---|---|
| 언어 | TypeScript |
| 프레임워크 | React 19 |
| 빌드 도구 | Vite |
| UI 라이브러리 | Ant Design |
| 라우팅 | React Router |
| 서버 상태 관리 | TanStack Query |
| 클라이언트 상태 관리 | Zustand |
| 폼 | React Hook Form + Zod |
| HTTP 클라이언트 | Axios |
| 인증 | JWT (Access / Refresh) |
| 린트 / 포맷 | ESLint + Prettier |
<br>

### 백엔드
| 구분 | 기술 |
|---|---|
| 언어 | Java 21 |
| 프레임워크 | Spring Boot 4.0.7 (Web MVC, Data JPA, Security, Validation) |
| 데이터베이스 | PostgreSQL (+ pgvector) |
| 마이그레이션 | Flyway |
| 캐시 | Redis |
| 인증 | JWT (jjwt) |
| 스토리지 | AWS S3 (로컬은 MinIO) |
| API 문서화 | springdoc-openapi (Swagger UI) |
| 모니터링 | Micrometer, Prometheus, Grafana |
| 테스트 | JUnit 5, Mockito, Testcontainers |
| 부하 테스트 | k6 |
| 빌드 도구 | Gradle |
| CI/CD & 인프라 | GitHub Actions, Docker, Docker Hub, AWS EC2 |
<br>

### AI 서버
| 구분 | 기술 |
|---|---|
| 언어 | Python 3.13 |
| 프레임워크 | FastAPI, Uvicorn |
| 데이터 검증 / 설정 | Pydantic v2 |
| HTTP 클라이언트 | httpx |
| LLM 연동 | OpenAI 호환 Chat Completions API |
| 객체 스토리지 | boto3 (AWS S3 / MinIO) |
| 머신러닝 | XGBoost, Fairlearn, SHAP, LIME, scikit-learn |
| 리포트 생성 | Jinja2, Playwright(PDF 변환), python-docx |
| 테스트 | pytest |

<br><br>

## 디자인 시안




<br><br>

## 팀원 정보

| Name | GitHub | 만든 기능 |
|---|---|---|
| 김⁠다⁠애 | [<img src="https://github.com/fluidicon.png" width="16"/> a⁠s⁠i⁠a⁠s⁠y⁠o⁠u⁠a⁠s⁠i⁠a⁠s](https://github.com/asiasyouasias) | • 법령 조항 pgvector 유사도 검색(RAG)<br>• 법령 조문 임베딩 생성 파이프라인<br>• AI 기본법 조문 스키마·시딩 데이터<br>• AI 기본법 개정 자동 추적·알림<br>• 자가점검 결과-법령 조항 자동 매핑 서비스<br>• 법령 매핑 조회 API(항 단위 근거·요약 라벨)<br>• STEP4 규제 자가점검 응답 저장/조회 API<br>• 감사 이력 목록 조회 API<br>• 모델 목록 조회 API<br>• 감사 데이터셋 재사용(모델 계열 공유)<br>• 감사 취소·재시도 API 및 동시성 처리<br>• 서버 재시작 시 고아 감사 자동 실패 처리<br>• 감사 실패 인앱 알림(백엔드+프론트 알람 UI) |
| 김⁠다⁠진 | [<img src="https://github.com/fluidicon.png" width="16"/> D⁠a⁠j⁠i⁠n⁠-⁠0⁠1](https://github.com/Dajin-01) | • SHAP 설명가능성 분석 파이프라인 FastAPI 연동<br>• SHAP REVIEW 판정 단계 추가<br>• 설명가능성 HTML·PDF·Word 리포트 생성·저장·조회·다운로드<br>• 고영향 AI 사전진단 PDF·Word 보고서 생성·다운로드<br>• 이의제기 대응문서 전역 SHAP·LLM 연동<br>• SHAP WARNING 상태 표시·연동<br>• 감사 결과 지표 도움말 툴팁<br>• 헤더 로고·회원탈퇴 팝업 등 UI 개선 |
| 김⁠석⁠주 | [<img src="https://github.com/fluidicon.png" width="16"/> B⁠a⁠n⁠a⁠n⁠a⁠-⁠b⁠o⁠s⁠s](https://github.com/Banana-boss) | • 규제 자가점검(STEP4) 21문항·7그룹 아코디언 UI<br>• STEP4 자가점검 백엔드 확장 및 실제 API 연결<br>• 자가점검 건너뛰기 확인 모달<br>• 모델 업로드·감사 실행 화면(신규/버전업 플로우)<br>• 감사 세부정보 상세보기 페이지<br>• 감사 산출물 관리 페이지<br>• 메인페이지·기능 살펴보기 페이지 구현<br>• 홈 화면 최근/진행중 감사 카드<br>• 마이페이지 구현<br>• 회원가입·로그인·자동로그인·권한 체계<br>• SHAP 전역 피처 중요도(TOP N) 조회 API<br>• Redis 캐시 역직렬화 오류 수정<br>• 개선권고가이드 노력의무 섹션 분리 |
| 오⁠희⁠주 | [<img src="https://github.com/fluidicon.png" width="16"/> m⁠o⁠m⁠i⁠j⁠u](https://github.com/momiju) | • 회원가입·로그인 인증 플로우(폼·API·자동로그인)<br>• reCAPTCHA 적용<br>• 비밀번호 재설정·표시 토글<br>• 최종 감사 보고서 생성 파이프라인(OpenAI LLM 클라이언트, 프롬프트 빌더·템플릿)<br>• 최종 감사 보고서 생성·다운로드 API<br>• 플로팅 챗봇 UI<br>• 이의제기 대응문서 페이지<br>• CSV 업로드 검증·도움말 메시지<br>• 보고서 카드 레이아웃 개선<br>• 자가점검 건너뛰기 UI 처리<br>• 인증·사용자 테스트 코드 작성 |
| 정⁠민⁠호 | [<img src="https://github.com/fluidicon.png" width="16"/> C⁠M⁠I⁠N⁠O⁠9⁠9](https://github.com/CMINO99) | • 대시보드 집계 API 구현<br>• 대시보드 감사 내역 전체조회·더보기/접기<br>• 대시보드 API 모델 버전 표시<br>• 대시보드 화면 구현 및 문구 개선<br>• 고영향 AI 사전진단 페이지 구현·플로우 수정 |
| 조⁠영⁠웅 | [<img src="https://github.com/fluidicon.png" width="16"/> J⁠o⁠J⁠i⁠m⁠i](https://github.com/JoJimi) | • 게시판 기능(백엔드 API+프론트 UI)<br>• 마이페이지(프로필 조회·수정, 비밀번호 변경, 회원탈퇴)<br>• 알림센터 인프라 구축(감사완료·법령개정 알림, 알림벨 UI, 알림 초기화)<br>• 감사 실행 플로우 UI(모델·데이터셋 업로드, 민감변수 선택, Validation 데이터셋)<br>• 공정성 지표 확장(Proportional/FDR/FOR/FPR Parity)<br>• SHAP·Fairlearn 분석 병렬 실행 전환<br>• 챗봇 UI/응답 개선(인용 표기, 근거 충실도, enum 한글화)<br>• Redis 캐싱 적용<br>• k6 부하테스트 스크립트 작성<br>• MinIO 로컬 개발환경 도입<br>• Swagger UI 적용·패키지 구조 리팩터링 |
| 황⁠민⁠서 | [<img src="https://github.com/fluidicon.png" width="16"/> M⁠i⁠n⁠s⁠e⁠o⁠H⁠w⁠a⁠n⁠g](https://github.com/MinseoHwang) | • XGBoost 위험점수·승인임계값 산출<br>• 감사 입력파일 검증<br>• Fairlearn 공정성 지표 계산 엔진(7종)<br>• 공정성 감사 API(S3 연동)<br>• OpenAI 호환 LLM 연동(AI 서버 클라이언트)<br>• SHAP 설명가능성 HTML 리포트 생성<br>• 편향진단·설명가능성·규제준수판정서·개선권고가이드·최종감사보고서 5종 리포트 생성(PDF·Word 변환 포함)<br>• 리포트 저장·조회·다운로드 API(백엔드)<br>• 감사 질의응답 챗봇(RAG 법령 인용 결합)<br>• 감사 결과 버전 비교, 보고서 병렬 사전생성 등 프론트 개선 |


