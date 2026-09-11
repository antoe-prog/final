# Adult Returner Re-entry Lane v1

- 작성일: 2026-09-12
- 상태: 전략·계측·실험 설계 / 고객 발송·시간표 변경·가격 변경·광고 집행 전
- 목표: 외부 유도 경험자 신규 유입, 휴면회원 복귀, 기존 성인반 수업품질 보호, 재활성 이후 재등록·순기여 검증
- 내부 회원수·매출·전환율: **데이터 미연결**

## 오늘의 판단

이번 회차의 우선순위는 **오래 쉬었다가 다시 유도를 시작하려는 성인을 일반 초보·일반 성인체험과 분리해 `Returner` 경로로 관리하는 것**이다.

현재 FINAL JUDO에는 신규 초보 시작창, 두 번째 방문 브리지, 첫 6주 활성화 문서가 이미 있으므로 이번 문서는 그 과정을 반복하지 않는다. 대상은 본인이 과거 유도 경험 또는 장기 휴식 후 복귀라고 명시한 사람이다. 외부 도장 출신 경험자도 자발적으로 신청한 경우 동일한 `RETURNER` 코호트로 분류할 수 있지만, 경쟁 도장 회원명단을 수집하거나 타 도장 회원을 직접 영업하지 않는다.

핵심 문제는 `복귀자 = 완전 초보` 또는 `복귀자 = 즉시 기존 강도 적응 가능`이라는 두 극단의 가정을 모두 피하는 것이다. 과거 단·급, 마지막 수련시점, 현재 목표와 실제 첫 방문 반응을 별도로 확인하고, 정규 성인반 연결 여부는 현재 운영정책과 코치 판단으로 결정한다.

## 발견한 변화

### 1. 2026-09-28 시작 예정인 Mannheim Judo Club은 성인 초보와 복귀자를 한 과정에 명시적으로 포함한다

1. Mannheimer Judo-Club은 2026-09-28 시작하는 10회 성인 Schnupperkurs를 `Anfängerinnen und Anfänger sowie Wiedereinsteigerinnen und Wiedereinsteiger` 대상으로 공개했다. 첫날 체험, 낙법·메치기·굳히기 등 과정, 최대 10명 정원도 함께 안내한다.

참고점은 가격이나 10회라는 숫자가 아니라 **복귀자를 별도 모집대상으로 명시하고 정원이 있는 구조화된 재진입 경로를 둔다는 것**이다.

출처: https://www.1-mannheimer-judo-club.de/judo-anfaenger-und-wiedereinsteiger-kurs-fuer-erwachsene-ab-28-september-2026/

### 2. JudoScotland는 2026-09-13에 29세 이상 전용 월간 Veterans 세션을 운영한다

JudoScotland의 9월 Veterans 세션은 29세 이상을 대상으로 능력·성별·경험·급수와 관계없이 참여 가능하다고 안내하며, 같은 연령대의 judoka와 수련·교류하는 월간 구조를 사용한다.

이는 `30+ 성인이 반드시 별도반을 원한다`는 증거가 아니다. 다만 성인 유도 시장에서 **나이·경험단계를 기준으로 별도 진입·커뮤니티 경험을 설계하는 실제 사례**로 참고한다.

출처: https://www.judoscotland.com/event/scottish-veterans-judo-session-september-2026/

### 3. British Judo의 2026-09-12 대회는 Veteran·Masters 연령 세그먼트를 별도로 운영한다

British Judo 일정에 등록된 PSUK National Judo Championships는 Veterans +35, Masters +45, Masters +60 구분을 사용한다. 이것은 도장 마케팅 성과 근거가 아니라 **성인 유도의 장기 참여경로가 연령별로 실제 존재한다는 생태계 신호**다.

출처: https://www.britishjudo.org.uk/event/l3-psuk-national-judo-championships-manchester-2026/

### 4. Honolulu Judo Club은 2026년 8~9월 50+ 별도 프로그램과 성인 초보/경험자 재입문 수업을 병행한다

Honolulu Judo Club은 현재 50세 이상 대상 별도 Safe Falling Class와 함께, 첫 입문자 및 과거 경험을 다시 익히려는 성인을 포함하는 Beginning Judo for Adults를 운영한다.

해당 클럽이 제시하는 낙상·부상 관련 효능을 FINAL JUDO의 마케팅 주장으로 사용하지 않는다. 참고할 것은 **연령·과거경험에 따라 진입상품을 세분화하는 운영형태**다.

출처: https://www.honolulujudoclub.com/classes

## Growth Analyst — 퍼널과 데이터 정의

외부 경험자 신규획득:

`returner_content_view → self_identified_returner → reservation → verified_return_visit → visit_2 → paid_within_14d → active_30d → renewal → revenue`

기존 휴면회원 복귀:

`dormant_eligible → approved_contact_scope → return_opt_in → return_visit → visit_2 → reactivated_30d → renewal → revenue`

권장 내부 필드:

- `cohort_type`: EXTERNAL_RETURNER / DORMANT_RETURNER
- `prior_judo_experience_self_declared`: YES / NO
- `time_since_last_training_bucket`: <1Y / 1-3Y / 3-5Y / 5Y+ / UNKNOWN
- `prior_grade_self_declared`: OPTIONAL / UNKNOWN 허용
- `return_goal`: FITNESS / TECHNIQUE / COMPETITION / COMMUNITY / OTHER / UNKNOWN
- `return_slot_id`
- `return_visit_attended`
- `second_visit_attended`
- `paid_within_14d`
- `active_30d`
- `renewal_status`
- `coach_onboarding_minutes`
- `existing_class_disruption_flag`

개인 식별정보·연락처·출석원장·결제원장은 공개 GitHub에 저장하지 않는다.

과거 단·급은 본인 진술과 공식 인정상태를 구분한다. `예전 ○단이었다`는 답변만으로 현재 승급·대회·회원자격을 자동 인정하지 않는다.

## Acquisition Marketer — 신규 획득 캠페인

대상은 `예전에 유도를 했고 다시 시작할 장소를 찾는 성인`이 스스로 해당 경험을 표시한 경우다.

메시지 방향 후보:

- `오래 쉬었던 유도, 처음부터 다시 할 필요는 없습니다. 현재 상태를 확인하고 다시 연결하는 성인 복귀 경로`
- `예전에 유도 경험이 있다면 첫날부터 무리한 대련보다 현재 감각과 수업 적합도를 먼저 확인`
- `단·급보다 지금 가능한 수업 강도와 목표부터 확인`

실제 첫날 커리큘럼이 확정되기 전에는 `대련 없음`, `낙법만 진행`, `개인 레슨`, `원래 띠 유지`, `즉시 승급 가능`을 약속하지 않는다.

경쟁 도장명·대회 참가자명단·SNS 팔로워를 영업대상으로 수집하지 않는다.

## CRM Marketer — 휴면 복귀 운영

휴면회원 연락은 기존 동의·연락허용 범위와 대표 승인을 확인한 사람만 대상으로 한다.

권장 분기:

1. 현재 활성회원·최근 재등록자는 제외한다.
2. 이미 복귀의사 없음 또는 수신거부를 밝힌 사람은 제외한다.
3. 복귀안내 대상이더라도 `오래 쉬었으니 다시 등록하세요`가 아니라 본인이 원하면 현재 가능한 시작경로를 확인하도록 한다.
4. 첫 복귀 방문 후에는 신규회원 온보딩과 동일하게 강제하지 않고, `RETURNER` 코호트로 2회차·30일 활성까지 추적한다.
5. 과거 결제금액·미납·건강정보를 공개 콘텐츠나 GitHub 문서에 사용하지 않는다.

## Content Strategist — 채널별 실행 초안

### 네이버

검색형 콘텐츠 후보:

- `성인 유도, 몇 년 쉬었다가 다시 시작할 때 확인할 5가지`
- `예전 띠가 있어도 복귀 첫날에 먼저 확인해야 할 것`
- `유도 복귀자와 완전 초보의 첫 수업이 같아야 할까?`

핵심 CTA는 `등록`보다 `현재 복귀 가능한 수업시간 확인`으로 둔다.

### Instagram

과정형 콘텐츠:

`오랜만에 도장 도착 → 준비운동 → 기본 움직임 확인 → 기술 감각 회복 → 다음 수업 선택`

실제 회원 사례를 촬영할 경우 별도 동의를 받고, 과거 대비 체력·체중·기술 변화 전후를 조롱하거나 과장하지 않는다.

### 당근

실제 여석이 있는 경우에만 지역형 문구를 검토한다.

예: `강서구 성인 유도 다시 시작 — 과거 경험이 있다면 현재 가능한 복귀 수업부터 확인`

`왕년에 선수였던 분`, `예전 단증 있으면 우대`처럼 불필요한 위계·과장 메시지를 사용하지 않는다.

## Experiment Manager

### EXP-215 Returner-Specific Entry vs Generic Adult Trial

**가설:** 과거 유도 경험을 스스로 표시한 성인에게 일반 성인체험보다 복귀자 전용 안내경로를 보여주면 예약 이후 실제 재참여와 30일 활성까지 개선될 수 있다.

**비교:**
- Control: 일반 성인 체험 안내
- Treatment: 복귀자 전용 CTA + 현재상태 확인 + 적합한 시작슬롯 제안

**핵심 KPI:**
- self_identified_returner → reservation
- reservation → verified_return_visit
- return_visit → visit_2
- return_visit → paid_within_14d
- paid → active_30d

**운영 KPI:**
- coach_onboarding_minutes
- existing_class_disruption_flag
- 고객 불편·과도한 연락 건수

**판정:** 기준선·최소개선폭·표본수는 **데이터 미연결**이다. 문의·예약만 늘고 2회차·14일 등록·30일 활성이 개선되지 않으면 확대하지 않는다.

### EXP-216 Restart Orientation vs Direct Regular-Class Entry

**가설:** 복귀자가 곧바로 일반 성인반에 들어가는 방식보다, 짧은 상태확인·수업안내를 거친 뒤 정규반으로 연결하는 방식이 초기 이탈과 기존반 운영마찰을 줄일 수 있다.

실험 시 두 그룹 모두 동일한 안전기준과 기본 지도품질을 보장한다. 일부러 대조군을 위험하거나 불친절하게 만들지 않는다.

**중단 기준:**
- 안전정원 초과
- 반복적인 기존회원 수업중단
- 코치 처리시간이 사전 허용범위 초과
- 과거 단·급 인정 관련 분쟁 증가
- 의료·체력상태를 마케팅용으로 수집·노출

## 필요한 대표 승인·입력

실행 전 확인할 값:

1. `휴면회원`의 내부 정의와 연락 허용범위
2. 최근 휴면회원 중 복귀의사를 자발적으로 표시한 비식별 집계
3. 외부 신규문의 중 과거 유도 경험자 비중
4. 복귀자 수용 가능한 지점·요일·시간·안전정원
5. 복귀 첫 방문에서 허용할 기술·란도리 범위
6. 과거 단·급 확인 및 현재 인정 절차
7. 도복·보험·체험비·회원권 정책
8. 첫 복귀 방문 후 정규 성인반 연결 기준

대표 승인 전 고객 메시지 발송, 휴면목록 추출, 할인·무료체험 약속, 시간표 변경, 광고집행을 하지 않는다.

## KPI 상태

| KPI | 상태 |
|---|---|
| 외부 문의 중 과거 유도 경험자 비중 | 데이터 미연결 |
| 휴면회원 수 | 데이터 미연결 |
| 연락 가능 휴면회원 수 | 데이터 미연결 |
| 복귀안내 opt-in | 데이터 미연결 |
| 복귀 예약→실방문 | 데이터 미연결 |
| 복귀 첫 방문→2회차 | 데이터 미연결 |
| 복귀 첫 방문→14일 등록/재활성 | 데이터 미연결 |
| 등록/재활성→30일 활성 | 데이터 미연결 |
| 복귀자 재등록률 | 데이터 미연결 |
| 신규 외부 returner CAC | 데이터 미연결 |
| coach_onboarding_minutes | 데이터 미연결 |
| 기존 성인반 운영마찰 | 데이터 미연결 |
| 귀속매출·순기여 | 데이터 미연결 |

## Auditor — 근거·브랜드·개인정보 리스크

- 해외 사례는 `복귀자·성인·Veteran을 별도 경로로 운영하는 사례가 있다`는 근거일 뿐 FINAL JUDO 수요·전환율을 증명하지 않는다.
- `30+`, `40+`, `50+` 자체를 체력저하·부상위험과 연결해 공포 마케팅하지 않는다.
- Honolulu의 낙상·부상 관련 표현을 FINAL JUDO의 의료·안전 효능 주장으로 사용하지 않는다.
- 복귀자에게 과거 단·급·선수경력을 과장해 공개하도록 요구하지 않는다.
- 휴면회원 연락은 기존 수신동의·관계·대표 승인 범위 안에서만 진행한다.
- 경쟁 도장 회원 또는 대회 참가자를 식별해 직접 마케팅하지 않는다.
- 성공은 `문의수`가 아니라 `실방문 → 2회차 → 14일 등록/재활성 → 30일 활성 → 재등록 → 순기여`로 판정한다.
