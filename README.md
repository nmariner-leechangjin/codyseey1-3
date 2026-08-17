프로젝트 2 --- 고객 문의 자동 분류 및 Slack 알림 자동화
0. 프로젝트 요약
자동화 도구: Zapier
연동 도구: Google Forms, Slack
핵심 구조: Trigger → Router(Paths) → 조건별 Action
분기 기준: 긴급도 = `긴급` / `일반`
목적: 고객 문의를 자동 분류하여 Slack 채널에 전달
보너스 과제: 제외
1. 문제 정의
Google Forms로 접수된 고객 문의를 담당자가 매번 확인하고 긴급 여부를
판단해 별도 채널에 전달하면 반복 작업이 발생한다. 이를 줄이기 위해 새
문의가 등록되면 Zapier가 자동으로 응답을 읽고 `긴급도`에 따라 두 경로로
분류한 뒤 Slack 채널에 전달하도록 설계하였다.
기대 효과
고객 문의 확인 과정 단축
긴급 문의의 빠른 식별
수동 복사·붙여넣기 감소
Slack을 통한 업무 공유
유사 접수 업무에 재사용 가능
2. 전체 자동화 구조
``` text
[Google Forms]
      ↓
[Trigger] New Form Response
      ↓
[Router] Paths
   ↙          ↘
Path A       Path B
긴급         일반
  ↓            ↓
Slack        Slack
Action A     Action B
```
처리 조건 및 우선순위
순서   단계      입력             조건/처리        출력
---
1      Trigger   신규 Form 응답   새 응답 발생     응답 데이터
2      Router    긴급도           긴급/일반 비교   실행 Path 결정
3-A    Path A    긴급 문의        긴급도 = 긴급    Slack 긴급 메시지
3-B    Path B    일반 문의        긴급도 = 일반    Slack 일반 메시지
긴급도는 하나의 값만 선택하도록 하여 두 Path가 동시에 실행되지 않도록
구성하였다.
3. 입력 데이터 설계
필드명     형식         예시                        필수   용도
---
고객명     Text         테스트고객A                 필수   문의자 식별
이메일     Email/Text   test-a@example.com          필수   연락 정보
문의유형   선택값       결제/환불                   필수   문의 종류
긴급도     선택값       긴급 / 일반                 필수   Paths 분기
문의내용   Long Text    결제 오류가 발생했습니다.   필수   상세 내용
제출용 테스트에는 실제 개인정보 대신 가상의 이름과 테스트 데이터를
사용한다.
캡처 1 --- Google Forms 입력 화면
![alt text](고객문의_googleform-1.png)![alt text](고객문의_googleform-1-1.png)
``` md
![Google Forms 입력 화면](images/01_google_forms.png)
```
4. Trigger 설계
서비스: Google Forms
Event: `New Form Response`
동작: 새로운 문의가 제출되면 Zap 실행
입력/출력
``` text
입력: 새로운 Google Forms 응답
출력: 고객명 / 이메일 / 문의유형 / 긴급도 / 문의내용
```
캡처 2 --- Zapier 전체 Workflow![alt text](고객문의_workflow-1.png)
``` md
![Zapier 전체 Workflow](images/02_zapier_workflow.png)
```
Trigger → Paths → Path A/Path B → Slack이 한 화면에 보이도록 캡처한다.
5. Router(Paths) 및 필터 규칙
Path A --- 긴급 문의
``` text
Field     : 긴급도
Condition : (Text) Exactly matches
Value     : 긴급
```
``` md
![Path A 긴급 조건](images/03_path_a_urgent.png)
```c:\코디세이B1-1\제출과제B1-3\project2\고객문의_pathA조건.png
Path B --- 일반 문의
``` text
Field     : 긴급도
Condition : (Text) Exactly matches
Value     : 일반
```
``` md
![Path B 일반 조건](images/04_path_b_normal.png)
```c:\코디세이B1-1\제출과제B1-3\project2\고객문의_pathB조건.png
분기 규칙
Path   조건            일치 시             불일치 시
---
A      긴급도 = 긴급   Slack 긴급 메시지   실행 안 함
B      긴급도 = 일반   Slack 일반 메시지   실행 안 함
Exact Match를 사용하여 `긴급`과 `일반`을 명확하게 구분한다.
테스트 데이터가 `긴급`인데 Path B 조건이 `일반`일 때
`Your path would not have continued.`가 표시되었다. 이는 오류가 아니라
조건 불일치로 해당 Path가 실행되지 않는 정상 결과이다.
6. Action 설계
Action A
Slack `Send Channel Message`
Path A 통과 시 실행
긴급 문의 전달
Action B
Slack `Send Channel Message`
Path B 통과 시 실행
일반 문의 전달
따라서 프로젝트 전체는 Trigger 1개 + Router(Paths) 1개 + 조건별 Action
2개로 구성된다.
7. Field Mapping
Google Forms   Slack Message
---
고객명         고객명
이메일         이메일
문의유형       문의유형
긴급도         긴급도
문의내용       문의내용
``` text
고객명: [Google Forms 고객명]
이메일: [Google Forms 이메일]
문의유형: [Google Forms 문의유형]
긴급도: [Google Forms 긴급도]
문의내용: [Google Forms 문의내용]
```
대괄호 부분은 직접 입력한 값이 아니라 Zapier 이전 Step에서 가져온 동적
데이터 필드이다.
캡처 5 --- Slack Mapping/Test
``` md
![Slack 필드 매핑 및 테스트](images/05_slack_mapping_test.png)
```c:\코디세이B1-1\제출과제B1-3\project2\고객문의_pathA슬랙샌드채널.pngc:\코디세이B1-1\제출과제B1-3\project2\고객문의_pathB슬랙샌드채널.png
8. 실제 실행 및 분기 검증
테스트 A --- 긴급
``` text
고객명: 테스트고객A
이메일: test-a@example.com
문의유형: 결제/환불
긴급도: 긴급
문의내용: 결제 오류가 발생했습니다.
```
기대 결과: Path A 실행 → Path B 미실행 → Slack 메시지 전송
테스트 B --- 일반
``` text
고객명: 테스트고객B
이메일: test-b@example.com
문의유형: 서비스 이용
긴급도: 일반
문의내용: 서비스 이용 방법을 문의합니다.
```
기대 결과: Path A 미실행 → Path B 실행 → Slack 메시지 전송
캡처 6 --- Slack 실제 결과
``` md
![Slack 실제 실행 결과](images/06_slack_result.png)
```c:\코디세이B1-1\제출과제B1-3\project2\고객문의_슬랙결과.png
가능하면 Slack 채널에 긴급 1건과 일반 1건이 각각 들어온 화면이 함께
보이도록 캡처한다.
9. 자동 실행 상태 증명
최종 제출 전 Zap을 Publish/ON 상태로 만들고 실제 Google Forms
응답으로 자동 실행을 확인한다.
``` text
1. Zap Publish/ON
2. Google Forms 테스트 문의 제출
3. Zapier Run History에서 Trigger/Path 실행 확인
4. Slack 메시지 도착 확인
```
추가 권장 캡처
``` md
![Zap Publish 및 실행 이력](images/07_zap_run_history.png)
```
이 화면은 자동 실행과 Trigger 동작 증명에 사용한다. 실제 Publish/ON 및
실행 이력을 확인한 뒤 캡처한다.
10. 처리 방식
본 프로젝트는 사용자가 Google Forms를 제출한 후 Zapier가 후속 작업을
수행하는 이벤트 기반 비동기 자동화이다.
``` text
사용자 Form 제출 완료
        ↓
Zapier 후속 처리
        ↓
조건 분기
        ↓
Slack 전달
```
사용자는 Slack 전송 완료를 기다릴 필요 없이 Form 제출을 완료할 수 있다.
11. 실패 처리 및 재시도 전략
---
오류                    1차 대응                대체 대응
---
Form 응답 읽기 실패     Run History 확인 후     원본 응답 수동 확인
재실행
Slack 일시 전송 실패    실패 Step 재실행        Slack 수동 전달
Slack 권한/인증 오류    계정 재연결             Workspace 권한 확인
분기값 오류             긴급/일반 값 확인       Forms/Path 조건 수정
정책
무한 재시도하지 않는다.
먼저 Zapier 실행 이력에서 실패 Step을 확인한다.
일시 연결 오류는 재실행한다.
인증 오류는 계정을 재연결한다.
반복 실패 시 원본 Google Forms 응답을 기준으로 수동 처리한다.
12. 보안 및 민감정보
제출물에는 다음 정보를 포함하지 않는다.
비밀번호
API Key
Access/OAuth Token
실제 고객 개인정보
실제 고객 이메일/전화번호
민감정보 비노출 확인: README와 제출 캡처에는 비밀번호, API Key,
Access Token 등 인증정보를 의도적으로 포함하지 않으며 테스트에는 가상의
고객명과 테스트 데이터를 사용한다.
GitHub 업로드 전 캡처에 개인 이메일, 계정정보, 토큰이 노출되지 않았는지
다시 확인한다.
13. 도구 선정 및 운영·권한
---
도구                    역할/선정 이유           권한 관리
---
Google Forms            입력 수집이 간단함       소유자/편집 권한
Zapier                  Trigger·Paths·Action을   연결 계정/Zap 편집 권한
시각적으로 연결
Slack                   팀 채널에서 결과 즉시    Workspace/Channel 권한
확인
Google Forms는 입력, Zapier는 자동화 제어, Slack은 출력 역할로 책임을
분리하였다.
14. 모듈 입력/출력 인터페이스
모듈             입력            출력
---
Google Forms     고객 5개 필드   Form Response
Zapier Trigger   Form Response   구조화 응답
Paths            긴급도          Path A 또는 B
Slack Action     5개 응답 필드   Channel Message
이 구조는 Slack을 다른 알림 도구로 교체하거나 입력 도구를 변경하는
방식으로 재사용할 수 있다.
15. 노코드 한계와 코드 확장
한계 1 --- 문의 내용 자체의 자동 판단 부족
현재는 사용자가 `긴급/일반`을 직접 선택한다.
확장: Python 또는 LLM API를 연결하여 문의 텍스트를 분석하고 긴급도를
자동 판정할 수 있다.
``` text
문의내용 → 텍스트 분석 → 긴급도 자동 판정 → 분기
```
한계 2 --- 복잡한 장애 처리
오류 유형, 재시도 횟수, 중복 방지 등 조건이 많아지면 노코드 Workflow가
복잡해진다.
확장: 백엔드/서버리스 함수를 추가하여 오류 로그 DB 저장, 재시도 횟수
제어, 중복 문의 방지, 담당자 자동 배정 등을 구현할 수 있다.
16. 핵심 개념
Trigger: Google Forms에 새 문의가 제출되는 시작 이벤트
Router/Paths: 긴급도 값에 따라 실행 경로를 나누는 분기
Action: 조건을 통과한 뒤 Slack 메시지를 전송하는 실제 작업
Field Mapping: 이전 Step 데이터를 다음 Step 필드에 연결하는 과정
17. 프로젝트 결과
``` text
Google Forms
      ↓
New Form Response
      ↓
Paths
  ↙       ↘
긴급      일반
 ↓         ↓
Slack     Slack
```
단순한 Trigger → Action을 넘어 입력값에 따라 서로 다른 Path를 실행하는
조건 분기 자동화를 구현하였다.
18. 네이토 사전평가 보완 반영
사전평가 지적                        반영 내용
---
실행 캡처 부족                       구성/조건/Slack 결과 캡처 위치 명시
별도 Markdown 필요                   본 보고서를 독립 `.md`로 제출
자동 실행 증명 부족                  Publish/ON + Run History 증빙 추가
Trigger/Action/분기 실행 증거 부족   긴급/일반 테스트 케이스 추가
민감정보 확인 없음                   비노출 확인 문구 추가
입력/액션/분기 조건 부족             조건/우선순위 표 추가
단계별 데이터 포맷 부족              입력 데이터 및 I/O 표 추가
Router 규칙 부족                     Exact Match 분기 규칙 명시
동기/비동기 설명 부족                이벤트 기반 비동기 처리 설명
운영·권한 설명 부족                  도구별 운영/권한 표 추가
재시도/대체 전략 부족                실패 처리 및 재시도 정책 추가
모듈 인터페이스 부족                 모듈별 입력/출력 표 추가
노코드 한계/코드 확장 없음           한계 2개와 확장 아이디어 추가
> 사전평가의 **LLM 모델 3종 비교, 평가축 5개 이상** 등은 별도의 LLM
> 비교·선정 보고서에 해당하므로 이 Zapier 프로젝트에 가상의 평가 결과를
> 만들어 넣지 않는다.
19. 이미지 목록
``` text
images/
├─ 01_google_forms.png
├─ 02_zapier_workflow.png
├─ 03_path_a_urgent.png
├─ 04_path_b_normal.png
├─ 05_slack_mapping_test.png
├─ 06_slack_result.png
└─ 07_zap_run_history.png
```
20. 최종 체크리스트
[ ] `.md` 파일로 제출
[ ] Google Forms 캡처
[ ] Zapier 전체 Workflow 캡처
[ ] Path A `긴급` 캡처
[ ] Path B `일반` 캡처
[ ] Slack Mapping/Test 캡처
[ ] Slack 실제 실행 결과 캡처
[ ] Zap Publish/ON 및 Run History 캡처
[ ] 긴급 Path 실제 실행 확인
[ ] 일반 Path 실제 실행 확인
[ ] 개인정보/비밀번호/API Key/Token 비노출 확인
[ ] GitHub README 이미지 표시 확인
GitHub 권장 구조
``` text
project2-customer-inquiry-automation/
├─ README.md
└─ images/
   ├─ 01_google_forms.png
   ├─ 02_zapier_workflow.png
   ├─ 03_path_a_urgent.png
   ├─ 04_path_b_normal.png
   ├─ 05_slack_mapping_test.png
   ├─ 06_slack_result.png
   └─ 07_zap_run_history.png
```