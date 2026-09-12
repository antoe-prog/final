# Fall Rhythm Attendance Challenge v1

- 기준일: 2026-09-12
- 상태: 전략·계측·실험 설계 완료 / 고객 발송·게시·비용집행 미실행
- 우선 목표: 기존회원 재등록·만족, 30일 활성 보호
- 보조 목표: 휴면 전환 억제, 신규회원 초기 루틴 형성
- 내부 회원수·매출·전환율: **데이터 미연결**

## 오늘의 판단

이번 우선과제는 할인이나 신규 리드 캠페인을 하나 더 늘리는 것이 아니라, **회원 각자의 현실적인 주간 수업계획을 기준으로 4주간 운동 루틴을 만드는 `Fall Rhythm Challenge`를 검증하는 것**이다.

현재 9월 피트니스 시장에서는 단순 할인보다 `몇 회를 꾸준히 참여했는가`, `본인에게 맞는 목표를 골랐는가`를 중심으로 한 참여형 챌린지가 실제 운영되고 있다. Aspen Fitness는 2026년 9월 한 달 동안 15일 운동 참여를 목표로 하는 Strong September Challenge를 진행 중이고, Scout Studios는 같은 달 회원이 주 2회·4회·7회 중 본인 페이스에 맞는 티어를 선택하도록 한다. First Choice Martial Arts 역시 2026-08-30 공개 글에서 9월 백투스쿨 시기에 출석 루틴과 다음 승급 준비를 강조한다.

파이널유도는 이 사례의 숫자나 보상을 복제하지 않는다. 현재 반별 실제 출석, 회원별 정상 이용빈도, 코치 처리시간, 재등록률이 모두 데이터 미연결이므로, **고정 15회나 전원 동일 횟수 대신 회원권·반·개인 계획에 맞는 `personal cadence`를 기준으로 시험**한다.

## 발견한 변화

### 1. 9월 fitness challenge는 '할인'보다 '행동 횟수'를 핵심 장치로 쓰고 있다

Aspen Fitness의 Strong September Challenge는 9월 1~30일 중 15일 운동을 완료하는 구조다. 추천 보너스도 별도 장치로 두지만, 핵심 행동은 `show up`이다.

출처: https://www.aspenfitness247.com/strong-september-challenge

### 2. 같은 목표를 모두에게 강제하기보다 개인별 난이도를 선택하게 하는 사례도 확인된다

Scout Studios의 Make Your Move는 2026년 9월 한 달 동안 회원이 `주 2회 / 주 4회 / 주 7회` 중 자신의 페이스에 맞는 티어를 고르게 한다. 파이널유도에 동일 횟수를 적용할 근거는 없지만, **한 가지 출석목표를 모든 회원에게 강제하지 않는 설계**에는 참고할 수 있다.

출처: https://scoutstudios.com.au/whats-on/september-challenge-make-your-move/

### 3. 무도시설에서도 9월을 '출석 루틴 재정비' 시기로 명시하는 현재 사례가 있다

First Choice Martial Arts는 2026-08-30 게시글에서 백투스쿨 시즌에 출석을 유지하고 승급 준비 흐름을 이어가는 것을 강조한다. 이는 파이널유도 회원의 재등록 효과를 증명하는 자료는 아니지만, 현재 무도시설이 시즌 전환기 회원경험을 `출석 + 성장목표`와 연결하는 사례다.

출처: https://www.firstchoicemartialarts.com/blogs/news/step-into-september-strong-stay-disciplined-all-month-long

### 4. 강서구에서는 9~11월 무료·저비용 생활체육 선택지가 계속 운영된다

강서구공공체육시설은 2026년 하반기 러닝·슬로우조깅 등 여러 생활체육교실을 9~11월 운영하고 있다. 파이널유도는 가격 경쟁보다 `정해진 수업에 실제로 꾸준히 참여하고 기술 성장을 확인하는 경험`을 명확히 보여줄 필요가 있다.

출처: https://sports.gangseo.seoul.kr/fmcs/102

## CMO — 우선순위

1. 신규문의 수보다 `기존회원 active_30d`와 `renewal`의 기준선을 먼저 연결한다.
2. 할인 없는 참여 실험을 우선한다. 상품·무료개월·현금성 보상은 대표 승인 전 배제한다.
3. 고정 횟수 경쟁보다 각 회원의 기존 계획과 반 운영여건에 맞는 `personal cadence completion`을 측정한다.
4. 공개 순위표보다 개인 목표 달성형 구조를 기본안으로 둔다.

## Growth Analyst — 퍼널과 병목

핵심 유지 퍼널:

`eligible_member → goal_selected → sessions_planned → sessions_attended → cadence_completed → active_30d → renewal`

초기회원 별도 퍼널:

`new_paid_member → first_goal_selected → first_4w_attendance → active_30d → active_60d → renewal`

필요 최소 데이터:

- `member_id` — 운영DB 내부 전용
- `branch_id`
- `class_id`
- `membership_frequency_plan`
- `goal_window_start/end`
- `planned_sessions`
- `attended_sessions`
- `challenge_state` — NOT_STARTED / ACTIVE / COMPLETED / PAUSED / EXEMPT
- `active_30d_after_window`
- `renewal_after_window`
- `coach_extra_minutes`

개인 이름·연락처·출석원장·건강사유는 공개 GitHub에 저장하지 않는다.

### 예상 병목

- 모든 회원에게 같은 횟수를 적용해 주 1회 회원이 불리해지는 문제
- 추석·출장·시험 등 정상 일정 변동을 실패로 처리하는 문제
- 코치가 매회 수작업으로 체크하면서 행정부담이 커지는 문제
- 챌린지 참여 회원 때문에 일반수업이 과밀해지는 문제
- '많이 나온 사람 = 좋은 회원'으로 오해되는 브랜드 리스크

## Acquisition Marketer — 신규 체험과의 연결

이번 전략의 신규획득 역할은 제한한다. `Fall Rhythm Challenge`를 신규체험 할인으로 사용하지 않는다.

신규회원에게는 등록 후 첫 4주에 본인 현실적인 출석계획을 정하는 온보딩 옵션으로만 연결한다.

예시 흐름:

`trial_attended → paid_member → personal_cadence_selected → first_4w_attendance → active_30d`

체험 전환과 챌린지 효과를 동시에 바꾸면 인과가 섞이므로, 무료체험·쿠폰·추천보상 실험과 같은 코호트에서 중첩하지 않는다.

## CRM Marketer — 기존회원·휴면·만료예정

### 기존회원

- 참여는 opt-in 후보로 둔다.
- 목표는 기존 회원권과 실제 가능한 시간표를 기준으로 정한다.
- 결석 1회로 실패 처리하지 않고 `PAUSED / EXEMPT` 상태를 둔다.
- 공개 순위·벌칙·부정적 비교를 기본안에서 배제한다.

### 만료예정

재등록 직전 `챌린지 완료 = 자동 재등록 대상`으로 취급하지 않는다. 완료 여부는 참여경험 신호일 뿐이며, 실제 재등록은 별도 KPI로 측정한다.

### 휴면회원

휴면회원에게 일괄 참여 메시지를 보내지 않는다. 복귀의사를 자발적으로 보인 경우에만 `복귀 후 4주 개인 루틴` 옵션을 검토한다.

## Content Strategist — 채널별 실행안

### 네이버

후보 주제: `유도를 오래 배우려면 주 몇 회가 좋을까? 정답보다 중요한 건 내 일정에 맞는 루틴`.

특정 횟수가 건강·기술 향상을 보장한다고 주장하지 않는다. 실제 수업시간·반 이동 가능성 확인 후 게시한다.

### Instagram

개인회원 출석기록 대신 `이번 주 목표 세우기 → 수업 완료 체크 → 배운 기술 한 줄 기록` 형태의 코치 자체 그래픽을 사용한다. 회원 사진·이름은 별도 동의 없이는 사용하지 않는다.

### 당근

신규모집 할인보다 `가을 유도 루틴 시작` 콘텐츠를 검토하되, 실제 체험석이 확인된 시간만 CTA로 연결한다.

## Experiment Manager

### EXP-248 Personal Cadence Challenge vs Business-as-Usual

**가설:** 전원 동일 출석횟수가 아니라 개인별 계획에 맞춘 4주 루틴 목표가 이후 30일 활성과 재등록에 긍정적인 신호를 만들 수 있다.

**비교:**
- Control: 기존 수업운영
- Treatment: 목표 선택 → 진행 확인 → 기간 종료 회고

**핵심 KPI:**
- `goal_selected → cadence_completed`
- `eligible_member → active_30d`
- `eligible_member → renewal`
- 수업 불편·민원
- 코치 추가 처리시간

**성공 기준:** 시작 전 기준선·최소개선폭·코치시간 상한을 대표 승인으로 고정한 뒤, active_30d 또는 renewal이 개선되고 운영부담·불편이 허용범위 안일 때 확대 검토.

**중단 기준:** 수업과밀, 안전문제, 공개비교 압박, 코치 업무량 초과, active_30d/renewal 개선 없이 체크업무만 증가할 경우.

현재 기준선·최소개선폭·표본수·처리시간 상한은 **데이터 미연결**이다.

### EXP-249 Fixed Target vs Self-Selected Cadence

**가설:** 모든 회원에게 동일 횟수를 주는 것보다 회원권·반·개인 일정에 맞는 목표선택이 완료율과 만족도에 더 적합할 수 있다.

**KPI:** 목표선택률, 완료율, 중도포기율, 30일 활성, 재등록, 문의·불편건수.

**중단 기준:** 선택지가 복잡해 상담시간만 늘거나, 상품별 형평성 논란이 발생하는 경우.

### EXP-250 Private Progress vs Public Leaderboard

**가설:** 개인 진행상태만 보여주는 구조가 공개 순위보다 부담을 줄이면서도 참여를 유지할 수 있다.

**기본안:** Private Progress.

**공개 리더보드는 대표 승인과 개인정보·미성년자 검토 없이는 시행하지 않는다.**

**KPI:** 참여완료, 옵트아웃, 불편·비교 스트레스 피드백, 30일 활성.

## 필요한 대표 승인·입력

1. 본관·지점별 회원권 정상 이용빈도 정의
2. 최근 8~12주 비식별 출석분포
3. 반별 실제 수용가능 인원과 평균 실출석
4. 30일 활성의 내부 정의
5. 재등록 기준일과 측정기간
6. 코치가 챌린지 관리에 쓸 수 있는 허용시간
7. 참여 완료 시 보상 제공 여부 및 비용 상한 — 현재 **데이터 미연결 / 미승인**
8. 미성년자 참여 시 보호자 안내·동의 기준

## KPI 상태

- 기존회원 active_30d: **데이터 미연결**
- 회원별 정상 주간 이용빈도: **데이터 미연결**
- 목표선택률: **데이터 미연결**
- 챌린지 완료율: **계산 불가**
- 참여자 재등록률: **계산 불가**
- 비참여자 재등록률: **계산 불가**
- 휴면 전환율: **데이터 미연결**
- 신규회원 첫 4주 출석: **데이터 미연결**
- 코치 추가 관리시간: **데이터 미연결**
- 귀속매출·순기여: **데이터 미연결**

## Auditor

- Aspen의 `15일`, Scout의 `주 2/4/7회`를 파이널유도 기준값으로 복제하지 않는다.
- `출석을 많이 하면 재등록한다`, `챌린지가 이탈을 막는다`는 인과를 내부 데이터 연결 전 사실처럼 사용하지 않는다.
- 추석·시험·출장·휴회 등 정상 결석을 실패로 낙인찍지 않는다.
- 건강상 이유를 마케팅 세그먼트에 사용하지 않는다.
- 미성년자 이름·출석횟수·순위를 공개하지 않는다.
- 할인·상품·무료개월·현금성 보상은 승인 전 약속하지 않는다.
- Buddy Referral, Trial Pricing, Naver Coupon 등 다른 실험과 동시 적용해 효과를 섞지 않는다.

## 근거 출처

확인일: 2026-09-12.

1. Aspen Fitness, Strong September Challenge, 2026-09-01~09-30. https://www.aspenfitness247.com/strong-september-challenge
2. Scout Studios, Make Your Move September Challenge, 2026-09-01~09-30. https://scoutstudios.com.au/whats-on/september-challenge-make-your-move/
3. First Choice Martial Arts, `Step into September Strong, Stay Disciplined all Month Long`, 2026-08-30. https://www.firstchoicemartialarts.com/blogs/news/step-into-september-strong-stay-disciplined-all-month-long
4. 강서구공공체육시설, 2026 하반기 생활체육교실. https://sports.gangseo.seoul.kr/fmcs/102

## 실행 제한

이 문서는 공개 가능한 전략 설계다. 실제 고객 메시지 발송, 챌린지 참가 등록, 회원별 출석기록 처리, 공개 순위표, 네이버·Instagram·당근 게시, 할인·상품 지급, 광고 집행, 계정 변경은 대표의 별도 명시적 승인 없이 실행하지 않는다.
