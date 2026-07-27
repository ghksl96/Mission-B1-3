# [프로젝트 1] 노코드 자동화 도구 비교 분석 보고서


## 1. 프로젝트 개요
구글 스프레드시트에 입력된 고객 문의 데이터의 유형(긴급/일반)을 판별하여, 각기 다른 이메일 응답을 자동으로 발송하는 워크플로우를 구축하고, 이를 서로 다른 두 가지 자동화 도구(Make, Zapier)로 구현하여 비교 분석하는 것을 목적으로 합니다.

<img width="832" height="864" alt="google1" src="https://github.com/user-attachments/assets/d84d9451-23b5-4c34-886d-08737fffe93c" />
<br><br>


<img width="762" height="136" alt="google2" src="https://github.com/user-attachments/assets/782a13ac-5b73-450a-8bd7-6590fe63b7d6" />


### 워크플로우 구조
- **Trigger**: Google Sheets - 새로운 행(Row)이 추가될 때 실행
- **Condition (Filter/Router)**: 
    - '문의유형' 열의 값이 **'긴급'**인 경우 Path A 실행
    - '문의유형' 열의 값이 **'일반'**인 경우 Path B 실행
- **Action**:Slack,Gmail - 각 유형에 맞는 맞춤형 안내 메일 발송

---

## 2. 도구별 구현 및 실행 결과

### ① Make
- **워크플로우 구성**: 
  - Google Sheets 모듈과 Slack,Gmail 모듈 사이에 Router를 배치하여 조건 분기 설정
  <img width="1050" height="613" alt="make1" src="https://github.com/user-attachments/assets/394978e6-28f9-4674-9e2d-ef3eee59ed48" />

- **실행 결과 (Log)**:
  - '긴급' 데이터 입력 시 상단 경로 활성화, '일반' 데이터 입력 시 하단 경로 활성화 확인
 <img width="1484" height="783" alt="make3" src="https://github.com/user-attachments/assets/d1617a4b-298e-4e8c-8b73-febc3fb4c9ff" />

 <br>

<img width="1458" height="855" alt="make4" src="https://github.com/user-attachments/assets/4017c58c-467c-47e6-83c0-41b3e0b5efb9" />

<br>
<img width="610" height="319" alt="make5" src="https://github.com/user-attachments/assets/7e1c5e9c-d3b9-4e70-ba5f-48d0b747956f" />



### ② Zapier
- **워크플로우 구성**: 
  - 1단계 Trigger 이후 'Paths' 기능을 사용하여 Path A(긴급)와 Path B(일반) 분기 구성
<br>
<img width="842" height="831" alt="Zapier1" src="https://github.com/user-attachments/assets/4f48a1b6-2954-486f-9322-83d0cbe61a06" />
<br>

  
- **실행 결과 (History)**:
  - Zap History에서 각 필터 조건에 따라 성공적으로 필터링되어 메일이 발송된 로그 확인
<br>

<img width="1495" height="774" alt="Zapier2" src="https://github.com/user-attachments/assets/8593c9e9-0ab9-4ea2-991e-99b8fd8b6700" />

<br>

<img width="664" height="693" alt="Zapier3" src="https://github.com/user-attachments/assets/1cdb96f5-f5cd-47af-b5ee-e4609680c72c" />


---

## 3. 도구별 비교 분석 보고서

| 비교 항목 | Make (메이크) | Zapier (재피어) |
| :--- | :--- | :--- |
| **사용 난이도** | 보통 (캔버스형 시각적 구조) | 매우 쉬움 (직선형 구조) |
| **데이터 제어** |	함수, 데이터 포맷 변경 등 세밀한 제어 가능 | 단순 데이터 전달 |
| **연동 서비스 범위** | 1,600개 이상 (주요 앱은 다 있음) | 6,000개 이상 (압도적 1위) |
| **가격(가성비)** | 저렴한 편 (Operation당 과금) | 비싼 편 (Task당 과금) |
| **에러 핸들링** | 에러 발생 시 별도 경로 설정 가능 | 	단순 재시도 |

---

## 4. 종합 의견

### 각 도구의 장단점 정리
- **Make**
  - **장점**: 복잡한 로직을 시각적으로 관리하기 좋고, 무료 플랜의 작업 횟수가 넉넉함.
  - **단점**: 인터페이스가 다소 복잡하여 초기 학습 곡선이 있음.
- **Zapier**
  - **장점**: 설정이 매우 쉽고 빠르며, 에러 발생 시 해결 가이드가 잘 되어 있음.
  - **단점**: Path 기능을 포함한 고급 기능을 사용하려면 유료 플랜 결제가 필수적임.

### 최종 결론
* **Zapier를 추천하는 경우:** "나는 복잡한 건 싫고, 그냥 A 앱에 뭐가 생기면 B 앱으로 바로 보내고 싶다!" 할 때 사용.
<br>

* **Make를 추천하는 경우:** "데이터를 조건에 따라 나누고 싶고, 가성비 있게 대량의 자동화를 돌리고 싶다!" 할 때 사용.

---
**작성자:** [본인 성함]  
**완료일:** 202X년 X월 X일
