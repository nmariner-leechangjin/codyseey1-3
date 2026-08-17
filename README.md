프로젝트 1. 자동화 도구 비교 구현
주제: Google Forms 교육평가 결과 자동 분류
1. 프로젝트 개요
Google Forms에 교육생의 이름, 이메일, 교육과정, 평가점수를 입력하면 평가점수를 기준으로 자동 분류하여 Google Sheets에 기록하는 자동화 워크플로우를 구현하였다.

동일한 자동화 업무를 Make와 Zapier 두 가지 도구로 각각 구현하고, 두 도구의 사용 방법과 특징을 비교하였다.

자동 분류 기준
80점 이상 → 합격자
80점 미만 → 보완대상
2. Make 구현
2-1. 전체 워크플로우
Trigger

Google Forms - Watch Responses

분기

Router

조건

합격자: 평가점수 ≥ 80
보완대상: 평가점수 < 80
Action 1

Google Sheets - Add a Row
→ 합격자 시트에 저장

Action 2

Google Sheets - Add a Row
→ 보완대상 시트에 저장

[캡처 1] Make 전체 워크플로우
alt text
![alt text](Make_workflow-2.png)
[캡처 2] Make 합격자 조건
alt text![alt text](Make_filter_합격자-2.png)

[캡처 3] Make 보완대상 조건
alt text![alt text](Make_filter_보완대상-2.png)

[캡처 4] Make 실행 결과 - 합격자
alt text![alt text](Make_action2_googlesheet_합격자-2.png)

[캡처 5] Make 실행 결과 -![alt text](Make_action1_googlesheet_보완대상-2.png) 보완대상
alt text
3. Zapier 구현
3-1. 전체 워크플로우
Trigger

Google Forms - New Form Response

분기

Paths - Split into paths

Path A 조건

평가점수 > 79
→ 80점 이상 합격자

Path B 조건

평가점수 < 80
→ 80점 미만 보완대상

Action 1

Google Sheets - Create Spreadsheet Row
→ 합격자 시트에 저장

Action 2

Google Sheets - Create Spreadsheet Row
→ 보완대상 시트에 저장

[캡처 6] Zapier 전체 워크플로우
alt text![alt text](Zapier_workflow-2.png)

[캡처 7] Zapier 합격자 조건
alt text![alt text](Zapier_filter_합격자-2.png)

[캡처 8] Zapier 보완대상 조건
alt text![alt text](Zapier_filter_보완대상-2.png)

[캡처 9] Zapier 실행 결과 - 합격자
alt text alt text![alt text](Zapier_action1_합격자-2.png)

[캡처 10] Zapier 실행 결과 - 보완대상![alt text](Zapier_action2_보완대상-2.png)
Google Sheets 보완대상 시트에서 Zapier 실행 결과가 보이는 화면 삽입 alt text alt text

4. Make vs Zapier 비교 분석
이번 프로젝트에서는 Make와 Zapier를 이용하여 동일한 자동화를 구현하였다.

자동화 과정은 다음과 같다.

Google Forms 응답 → 점수 확인 → 합격/보완대상 분류 → Google Sheets 저장

두 도구 모두 같은 결과를 만들 수 있었지만, 실제 제작 과정과 조건을 분기하는 방법에서 차이가 있었다.

비교 항목	Make	Zapier
자동화 화면	전체 자동화 과정을 하나의 흐름도로 볼 수 있다.	작업 과정이 위에서 아래로 단계별로 표시된다.
조건 분기 방법	Router로 길을 나누고 각 경로에 Filter 조건을 설정한다.	Paths로 길을 나누고 각 Path에 조건을 설정한다.
이번 과제의 조건	점수 80점 이상 → 합격 / 80점 미만 → 보완대상	점수 80점 이상 → Path A / 80점 미만 → Path B
Google Sheets 저장	각 경로 끝에 Add a Row 모듈을 연결하였다.	각 Path 끝에 Create Spreadsheet Row 액션을 연결하였다.
작업 과정 확인	여러 모듈과 분기 구조를 한 화면에서 확인하기 좋았다.	Trigger → Paths → Action 순서로 따라가기 쉬웠다.
설정 난이도	Router와 Filter의 역할을 처음 이해하는 데 시간이 필요했다.	각 단계를 순서대로 설정할 수 있어 상대적으로 이해하기 쉬웠다.
확장성	조건과 작업이 많아질수록 전체 구조를 시각적으로 관리하기 좋다고 느꼈다.	단순한 자동화는 편하지만 분기가 많아지면 화면이 길어질 수 있다.
초보자 관점	처음에는 어렵지만 구조를 이해하면 전체 자동화 흐름을 파악하기 좋다.	처음 사용하는 사람도 단계별로 따라가기 쉬웠다.
실제 사용하면서 느낀 가장 큰 차이
Make에서는 Router와 Filter가 서로 다른 역할을 한다.

Router는 자동화 흐름을 여러 방향으로 나누고, Filter는 각각의 경로로 데이터가 이동할 수 있는지를 판단한다.

이번 프로젝트에서는 Router를 이용해 합격자와 보완대상 두 개의 경로를 만들었다.

점수 80점 이상 → 합격자 경로
점수 80점 미만 → 보완대상 경로
반면 Zapier에서는 Paths 기능 안에서 경로와 조건을 함께 관리한다.

Path A에는 80점 이상 조건을 설정하고, Path B에는 80점 미만 조건을 설정하였다.

따라서 같은 자동화를 구현했지만 개인적으로는 Zapier의 조건 분기 방식이 처음 이해하기에는 더 쉬웠다.

반면 Make는 전체 자동화 구조를 한 화면에서 확인할 수 있어서 자동화 과정이 복잡해질 경우에는 구조를 파악하고 관리하는 데 장점이 있다고 느꼈다.

5. Make 장단점
장점
전체 자동화 구조를 한 화면에서 시각적으로 확인하기 좋다.
Router를 이용하여 여러 개의 조건 분기를 만들기 편리하다.
각 모듈에서 전달되는 데이터를 상세하게 확인할 수 있다.
향후 복잡한 자동화로 확장하기에 유리하다.
단점
처음 사용할 때 Router와 Filter의 차이를 이해하는 과정이 필요했다.
Google Forms의 응답 데이터가 여러 단계의 Collection 구조로 표시되어 실제 응답값(Value)을 찾는 과정이 복잡했다.
Zapier보다 초기에 익혀야 할 개념이 많다고 느꼈다.
6. Zapier 장단점
장점
단계별 설정 방식이 직관적이다.
Trigger → Paths → Action의 흐름을 이해하기 쉬웠다.
Google Forms의 데이터를 Google Sheets에 연결하는 과정이 상대적으로 간단했다.
각 단계에서 Test 기능을 사용할 수 있어 정상 작동 여부를 확인하기 편리했다.
단점
Paths와 Path Conditions가 별도로 존재하기 때문에 처음에는 두 기능의 차이를 이해해야 했다.
분기와 Action이 많아질 경우 Make보다 전체 자동화 구조를 한눈에 파악하기 어려울 수 있다.
사용량이나 기능에 따라 무료 플랜의 제한을 고려해야 한다.
7. 어떤 상황에서 어떤 도구가 적합한가
두 도구를 이용하여 동일한 자동화를 직접 구현해 본 결과, 간단한 업무 자동화를 처음 만드는 경우에는 Zapier가 상대적으로 쉽게 느껴졌다.

Zapier는 각 단계를 순서대로 설정하기 때문에 처음 사용하는 사람도 Trigger와 Action의 관계를 쉽게 이해할 수 있었다.

반면 Make는 처음에는 Router, Filter, Collection 등의 개념 때문에 상대적으로 복잡하게 느껴졌다.

하지만 전체 자동화 구조를 한 화면에서 확인할 수 있고 Router를 이용하여 여러 경로를 시각적으로 구성할 수 있기 때문에, 조건이 많고 복잡한 자동화로 확장하는 경우에는 Make가 유리하다고 판단하였다.

따라서 다음과 같이 선택할 수 있다고 생각한다.

간단하고 빠른 업무 자동화 → Zapier
여러 조건과 복잡한 분기가 필요한 자동화 → Make
8. 프로젝트를 통해 이해한 핵심 개념
Trigger
자동화 워크플로우를 시작시키는 이벤트이다.

이번 프로젝트에서는 Google Forms에 새로운 응답이 제출되는 것이 Trigger이다.

Make: Google Forms - Watch Responses
Zapier: Google Forms - New Form Response
분기
하나의 데이터 흐름을 여러 개의 경로로 나누는 기능이다.

Make: Router
Zapier: Paths
Filter / Condition
분기된 데이터가 특정 조건을 만족하는지 판단하는 역할을 한다.

이번 프로젝트에서는 평가점수를 기준으로 조건을 설정하였다.

80점 이상 → 합격자
80점 미만 → 보완대상
Make에서는 Filter, Zapier에서는 Path Conditions를 사용하였다.

Action
Trigger 이후 조건에 따라 실제로 수행되는 작업이다.

이번 프로젝트에서는 Google Sheets에 새로운 행을 추가하는 작업이 Action이다.

Action 1

합격자 Google Sheets에 새로운 행 추가

Action 2

보완대상 Google Sheets에 새로운 행 추가

전체 자동화 구조
Google Forms에 새로운 응답 제출

↓

Trigger

↓

분기 (Router / Paths)

↓

조건 판단 (Filter / Path Conditions)

↙　　　　　　　　　↘

80점 이상　　　　　80점 미만

↓　　　　　　　　　　　↓

Action 1　　　　　　Action 2

합격자 시트 저장　　　　보완대상 시트 저장

9. 최종 결과 및 결론
동일한 교육평가 자동 분류 업무를 Make와 Zapier 두 개의 노코드 자동화 도구로 각각 구현하였다.

Google Forms에 교육생의 평가 결과가 제출되면 평가점수를 자동으로 확인하여 80점 이상은 합격자, 80점 미만은 보완대상으로 분류하고 각각 Google Sheets에 자동 기록하도록 구성하였다.

두 자동화 도구 모두 다음 요구사항을 충족하도록 구현하였다.

Trigger 1개 이상
Action 2개
조건 분기 포함
합격자 경로 실제 실행 확인
보완대상 경로 실제 실행 확인
Make에서는 Router + Filter, Zapier에서는 Paths + Path Conditions를 이용하여 동일한 조건 분기 구조를 구현하였다.

이번 프로젝트를 통해 단순히 자동화 도구의 사용법을 익히는 것뿐만 아니라 반복 업무를 다음과 같은 구조로 분석하고 설계하는 방법을 이해할 수 있었다.

Trigger → 분기 → 조건 판단 → Action

또한 동일한 자동화를 서로 다른 플랫폼에서 직접 구현함으로써 Make와 Zapier의 UI, 설정 방식, 데이터 처리 방식 및 조건 분기 방식의 차이를 비교할 수 있었다.R