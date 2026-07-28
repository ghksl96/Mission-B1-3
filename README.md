# [프로젝트 1] 노코드 자동화 도구 비교 분석 보고서


## 1. 프로젝트 개요
구글 스프레드시트에 입력된 고객 문의 데이터의 유형(긴급/일반)을 판별하여, 각기 다른 이메일 응답을 자동으로 발송하는 워크플로우를 구축하고, 이를 서로 다른 두 가지 자동화 도구(Make, Zapier)로 구현하여 비교 분석하는 것을 목적으로 합니다.

<img width="832" height="864" alt="google1" src="https://github.com/user-attachments/assets/d84d9451-23b5-4c34-886d-08737fffe93c" />
<br><br>


<img width="762" height="136" alt="google2" src="https://github.com/user-attachments/assets/782a13ac-5b73-450a-8bd7-6590fe63b7d6" />


### 워크플로우 구조
- **Trigger**: Google Sheets - 외부 사건(새로운 행)이 발생했을 때 워크플로우를 깨우는 '신호'입니다.
- **Condition (Filter/Router)**: 
    - '문의유형' 열의 값이 **'긴급'**인 경우 Path A 실행
    - '문의유형' 열의 값이 **'일반'**인 경우 Path B 실행
- **Action**:Slack,Gmail - 트리거 이후 실제로 수행되는 '작업'(각 유형에 맞는 맞춤형 안내 메일 발송)
- **비교문장**: "Trigger가 **'언제 실행할 것인가'**를 결정하는 스위치라면, Action은 **'무엇을 할 것인가'**를 수행하는 실제 일꾼입니다."

---

## 2. 도구별 구현 및 실행 결과

### ① Make
- **워크플로우 구성**: 
  - Google Sheets 모듈과 Slack,Gmail 모듈 사이에 Router를 배치하여 조건 분기 설정
  <img width="1050" height="613" alt="make1" src="https://github.com/user-attachments/assets/394978e6-28f9-4674-9e2d-ef3eee59ed48" />

- **실행 로그 (분기별 자동 실행 증빙):**:
  - 구글 시트 행 감지 (Type: 긴급)   <br>
 <img width="1484" height="783" alt="make3" src="https://github.com/user-attachments/assets/d1617a4b-298e-4e8c-8b73-febc3fb4c9ff" />
 
<br><br>

 - Slack 메시지 전송 성공(Status: 200)  <br>
<img width="974" height="726" alt="image" src="https://github.com/user-attachments/assets/164caf3a-fe7b-49b5-af8f-9965a956b674" />


 <br>  <br> 


- 구글 시트 행 감지 (Type: 일반)   <br>
<img width="1458" height="855" alt="make4" src="https://github.com/user-attachments/assets/4017c58c-467c-47e6-83c0-41b3e0b5efb9" />

<br> <br>

- Gmail 메시지 전송 성공(Status: 200) <br>

<img width="610" height="319" alt="make5" src="https://github.com/user-attachments/assets/7e1c5e9c-d3b9-4e70-ba5f-48d0b747956f" />



### ② Zapier
- **워크플로우 구성**: 
  - 1단계 Trigger 이후 'Paths' 기능을 사용하여 Path A(긴급)와 Path B(일반) 분기 구성
<br>
<img width="842" height="831" alt="Zapier1" src="https://github.com/user-attachments/assets/4f48a1b6-2954-486f-9322-83d0cbe61a06" />
<br><br><br>

  
- **실행 로그 (분기별 자동 실행 증빙):**:
  - [Trigger] New Row (Type: 긴급)
    
<img width="1495" height="774" alt="Zapier2" src="https://github.com/user-attachments/assets/8593c9e9-0ab9-4ea2-991e-99b8fd8b6700" />

<br><br>

- Slack 메시지 전송 성공  <br>

<img width="664" height="693" alt="Zapier3" src="https://github.com/user-attachments/assets/1cdb96f5-f5cd-47af-b5ee-e4609680c72c" />
<br><br><br>

- [Trigger] New Row (Type: 일반) 

<img width="1107" height="833" alt="image" src="https://github.com/user-attachments/assets/6fc9b60e-ac08-43e6-ba12-f38b5dc95d81" />

<br><br>

-  Gmail 발송 완료 <br>

<img width="559" height="272" alt="image" src="https://github.com/user-attachments/assets/3fb69451-0315-4479-9bcd-cb3c0d305dc1" />
<br><br><br><br>

* **두 도구가 동일한 결과를 내도록 1:1 매핑 규칙을 설계했습니다.**
[데이터 흐름 및 I/O 테이블]

| 단계 | 노드명 | 입력(input) 필드 | 출력(Output) 필드 | 	데이터 형식 |
| :--- | :--- | :--- |:--- | :--- |
| **Trigger** | Google Sheets | New Row Detection |Name, Email, Type, Content |String/JSON |
| **Router** |	Filter/Path | Type (문의유형) |Boolean (Match/No Match) | Condition |
| **Action A** | Slack |Name, Content| ts (Timestamp), channel_id| API Response|
| **Action B** | Gmail | Email, Name | id (Message ID), threadId | SMTP |





---

## 3. 도구별 비교 분석 보고서

| 비교 항목 | Make (메이크) | Zapier (재피어) |
| :--- | :--- | :--- |
| **사용 난이도** | 보통 (캔버스형 시각적 구조) | 매우 쉬움 (직선형 구조) |
| **데이터 제어** |	함수, 데이터 포맷 변경 등 세밀한 제어 가능 | 단순 데이터 전달 |
| **연동 서비스 범위** | 1,600개 이상 (주요 앱은 다 있음) | 6,000개 이상 (압도적 1위) |
| **비(가성비)** | 저렴한 편 (Operation당 과금) | 비싼 편 (Task당 과금) |
| **에러 핸들링** | 에러 발생 시 별도 경로 설정 가능 | 	단순 재시도 |
| **협업/권한** | 워크스페이스 공유 | 팀/기업용 관리 최적화 |
| **운영/디버깅** | 상세 로깅 (I/O 추적) | 	단순 결과 리포트 |

---

## 4. 종합 의견

### 각 도구의 장단점 정리
- **Make**
  - **장점**: 복잡한 로직을 시각적으로 관리하기 좋고, 동일 작업 1,000회 수행 시 Make가 약 15배 저렴
  - **단점**: 인터페이스가 다소 복잡하여 초기 학습 곡선이 있음.
- **Zapier**
  - **장점**: 설정이 매우 쉽고 빠르며, 에러 발생 시 해결 가이드가 잘 되어 있음.세부권한 설정이 강력함
  - **단점**: Path 기능을 포함한 고급 기능을 사용하려면 유료 플랜 결제가 필수적임.

### 최종 결론
* **Zapier를 추천하는 경우:** "나는 복잡한 건 싫고, 그냥 A 앱에 뭐가 생기면 B 앱으로 바로 보내고 싶다!" 할 때 사용.
<br>

* **Make를 추천하는 경우:** "데이터를 조건에 따라 나누고 싶고, 가성비 있게 대량의 자동화를 돌리고 싶다!" 할 때 사용.

---


# [프로젝트 2] 자유 주제 자동화 설계 및 구현


## 1.자동화할 반복 업무 정의 <br>
* **업무명:** 나만의 AI 뉴스 비서: 매일 아침 핵심 요약 및 자동 배달 <br>
* **기존 업무 방식:** 매일 아침 업무 시작 전, IT 뉴스 사이트나 블로그 5~6곳을 직접 방문해 새로운 기사가 있는지 확인합니다.<br>
중요한 기사는 내용을 읽고 메모장에 요약한 뒤, 팀원들과 공유하기 위해 슬랙에 올리거나 나중에 보려고 노션에 복사해 둡니다. <br>

  
<br>

## 2.자동화 도구 선정 및 선정 이유 <br>
* **선정 도구:** Make (메이크)<br>
* **선정 이유:** <br>
**이유 1 (데이터 제어):** <br>
 Gemini의 JSON 응답에서 특정 스코어를 추출하여 분기하는 로직이 Make의 Data Mapper에서 더 정교하게 작동함.<br>
**이유 2 (비용 효율):** <br>
매일 수십 건의 RSS 뉴스를 처리해야 하므로, Task당 비용이 높은 Zapier보다 Make가 운영 면에서 정량적으로 유리함.

<br><br>

## 3.워크플로우 설계 (Architecture) <br>

**[수집 → 분석 → 필터링 → 배포/저장]**의 4단계로 구성됩니다. <br>

* **설계 기준:** Gemini가 생성한 Importance_Score 필드값이 8점 이상인 경우만 긴급 알림 발송.<br>
**데이터 예시:**<br>
**기사 A:** "신규 아이폰 출시" → Score: 6 (Notion만 저장)<br>
**기사 B:** "OpenAI 새로운 모델 발표" → Score: 9 (Slack 알림 + Notion 저장)<br>

* **Trigger (RSS):** 지정된 뉴스 사이트에서 새로운 기사 감지. <br>
* **Analysis (Gemini):** <br>
프롬프트 엔지니어링을 통해 3줄 요약 및 1~10점 사이의 중요도 점수 생성. <br>
* **Router (Condition):** <br>
**Path A (High Priority):** 중요도 8점 이상인 경우 Slack으로 즉시 전송. <br>
**Path B (Archive):** 모든 기사를 Notion 데이터베이스에 기록.
<br>

* **에러 핸들링 및 모니터링 설계**<br>
**재시도 전략:** <br>
API 호출 실패 시 3회 재시도 (간격: 5분, 15분, 30분 - Exponential Backoff). <br>
**대체 경로:** AI 분석 실패 시, 원문 제목만 추출하여 '분석 실패' 태그와 함께 Notion에 강제 저장. <br>
**모니터링:** 시나리오 중단 시 관리자 Slack으로 즉시 에러 리포트 알림 발송. <br>

* **모듈화 전략 및 재사용성** <br>
**모듈화 레벨:**<br>
**Collector (수집):** RSS 데이터를 표준 JSON으로 변환 (타 뉴스레터 프로젝트에 재사용 가능)<br>
**Analyzer (분석):** Gemini 프롬프트 엔진 (보고서 요약 등 타 업무에 재사용 가능)<br>
**Publisher (배포):** Slack/Notion 전송 모듈 (독립적 기능 수행)<br>
<br>
<img width="1160" height="678" alt="make6" src="https://github.com/user-attachments/assets/ac1ea91f-f537-41bd-a63a-fa963f5bffe2" />

<br>

- **실행 결과 (Log)**: <br>
  - notion과 slack에 입력되는지 확인
  

<img width="1267" height="298" alt="notion1" src="https://github.com/user-attachments/assets/0f37c3fd-7f9e-42ad-af8a-3a186d2a685f" />

<br>


<img width="1233" height="713" alt="slack1" src="https://github.com/user-attachments/assets/02c873de-f3cb-4ab6-b418-197754e9364b" />

<br><br>


* **노코드의 한계 및 코딩 확장 계획** <br>
**구체적 한계 사례**<br>
**한계:** RSS 본문이 암호화되어 있거나, 로그인이 필요한 페이지의 경우 노코드 기본 모듈로 크롤링 불가.<br>
**사례:** 특정 유료 뉴스레터의 본문 수집 실패.<br><br>


* **코딩 확장 및 실행 계획**
| 단계 | 확장 아이디어 | 	우선순위 |예상 소요 시간|
| :--- | :--- | :--- |:--- |
| **1단계** | Python(Selenium) 기반 커스텀 크롤러 API 구축 | High | 3 |
| **2단계** |	OpenAI Whisper 연동 (뉴스 영상 음성 추출 요약) | Medium | 5일 |
| **3단계** |React 기반 개인용 뉴스 큐레이션 |Low | 10일 |

<br><br>



* **보안 및 민감정보 노출 점검 결과** <br>
**점검 일시:** 2026년 07월 27일 <br>
**점검 방법:** 전체 문서 및 스크린샷 내 API Key, Token, 개인정보(이메일, 전화번호) 노출 여부 전수 검사<br>
**결과:** PASS. 모든 민감 정보는 마스킹 처 또는 환경 변수 처리되었음을 확인하였습니다.


