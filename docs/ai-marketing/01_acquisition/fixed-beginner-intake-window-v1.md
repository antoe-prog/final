# Fixed Beginner Intake Window — 초보 입문기수 파일럿 v1

- 상태: 설계 완료, 실제 모집·가격변경·고객연락·예약정책 변경 전
- 공개자료 확인일: 2026-09-12
- 목적: 신규 체험 확보 + 체험 후 등록 전환 + 휴면 복귀의 초기 경험 표준화
- 적용 전제: 반별 안전정원·실출석·체험 허용석·현재 체험정책은 내부 데이터 확인 전 `데이터 미연결`

## 1. 오늘의 가설

파이널유도멀티짐이 완전 초보 신규자를 아무 날짜에나 기존 혼합반으로 투입하는 방식만 운영하는 대신, 월 1~2회의 `초보 입문 시작창(start window)`을 별도로 두면 첫 수업 설명의 편차와 코치의 반복 온보딩 부담을 줄이고, 신규자가 같은 시점에 시작하는 사람들과 함께 2회차·유료등록·30일 활성까지 이어질 가능성이 있는지 검증할 수 있다.

이 문서는 `입문기수 방식이 무조건 더 좋다`고 가정하지 않는다. 파이널유도의 실제 전환율·유지율·코치 부담 데이터는 아직 연결되지 않았기 때문에, 상시 체험과 비교 실험으로 판단한다.

또한 이 전략은 `Capacity-Aware Waitlist`와 목적이 다르다. Waitlist는 원하는 반이 만석일 때 수요를 보존하는 전략이고, Fixed Beginner Intake는 좌석이 있더라도 **완전 초보의 시작 시점 자체를 구조화**하는 전략이다.

## 2. 최신 공개 사례에서 확인한 패턴

### A. Bishops Stortford Judo — 매월 첫 주 신규 초보자 intake

Bishops Stortford Judokwai는 주니어 신규 입문을 `매월 첫째 주`에 시작하도록 운영하고, 연령대별로 가능한 taster 날짜를 구체적으로 안내한다. taster 이후 1개월 trial로 연결한다. 즉 신규자를 연중 아무 수업에 분산 투입하지 않고 월별 시작창을 운영하는 사례다.

- Source: https://www.bishopsstortfordjudo.com/new-starters.html
- 확인일: 2026-09-12

### B. St Albans Judo Club — 고정 시작일이 있는 12주 성인 초보과정

St Albans Judo Club은 2026년 9월 17일 시작하는 성인 Beginners Course를 별도 운영하고, 초보자뿐 아니라 오랜 휴식 후 복귀하는 사람에게도 같은 입문경로를 제시한다. 정규 Technical/Open Mat 세션 중 일부는 신규자에게 적합하지 않다고 명시한다.

- Source: https://www.stalbansjudo.org.uk/classes/
- Source: https://www.stalbansjudo.org.uk/
- 확인일: 2026-09-12

### C. Japan Arts Centre Police Sport UK Judo — 9월 16일 시작 6주 성인 초보과정

해당 클럽은 2026년 9월 16일 시작하는 6주 성인 초보과정을 정규 혼합반과 분리해 안내한다. 이 사례의 무료기간·가격정책은 파이널유도에 복제하지 않고, `초보자 시작일을 특정하고 정규반과 구분한다`는 구조만 참고한다.

- Source: https://japanartscentrejudo.com/adults-judo
- 확인일: 2026-09-12

### D. 강서구공공체육시설 — 월별 예약 공지와 하반기 기간형 생활체육교실

강서구공공체육시설은 2026년 9월 9일 이미 10월분 축구·풋살·테니스 접수 일정을 공지하고 있으며, 하반기 생활체육교실도 별도 모집기간을 거쳐 기간형 프로그램으로 운영한다. 이는 강서구 생활체육 이용자에게 `미리 정해진 접수창·시작일·운영기간` 방식이 낯선 형식이 아니라는 지역 운영 신호다. 다만 이것이 파이널유도 입문기수의 수요나 전환을 보장하는 근거는 아니다.

- Source: https://sports.gangseo.seoul.kr/fmcs/30
- 확인일: 2026-09-12

## 3. 파이널유도 적용안

### Starter Window 상태

- `OPEN_ALWAYS_ON`: 현재처럼 상시 체험 가능
- `INTAKE_OPEN`: 다음 초보 입문기수 신청 가능
- `INTAKE_RESERVED`: 예약 완료, 첫 수업 대기
- `INTAKE_ACTIVE`: 입문기수 진행 중
- `NEXT_INTAKE`: 이번 시작창 종료, 다음 시작창 안내 가능
- `NOT_ELIGIBLE`: 연령·경력·수업난이도·안전정원 기준상 해당 입문기수 부적합

### 최소 데이터 필드

- `branch_id`
- `segment` — adult_beginner / junior_beginner / returning
- `lead_created_at`
- `preferred_schedule`
- `starter_window_id`
- `starter_window_reserved_at`
- `starter_window_attended`
- `visit_2_attended`
- `paid_within_14d`
- `active_30d`
- `renewed`
- `coach_onboarding_minutes`
- `existing_class_disruption_flag`

개인 이름·연락처·출석원장·결제원장은 이 공개 문서에 저장하지 않는다.

## 4. 채널별 콘텐츠 구조

### 네이버

- `성인 유도 초보는 언제 시작하면 좋을까?`
- 실제 입문 시작창이 확정된 경우에만 `다음 초보 입문 시작일`을 표기
- 가격·무료체험·정원은 실제 정책 확인 전 미표기

### Instagram

- `첫날 → 두 번째 방문 → 정규반 연결` 과정을 짧은 시리즈로 보여준다.
- 기존회원 얼굴·미성년자 촬영은 사전 동의 없는 경우 사용하지 않는다.

### 당근

- 실제 안전정원과 시작창이 확정된 경우에만 `강서구 성인 유도 초보 입문기수`처럼 지역 + 대상 + 시작일을 명확히 표기한다.
- `마지막 N자리`, `이번 달 마지막 기회` 같은 희소성 문구는 실제 데이터가 없으면 금지한다.

## 5. CRM 라우팅

- `완전 초보 + 일정 적합` → 다음 starter window 안내
- `완전 초보 + 이번 창 일정 불가` → 다음 창 알림 자발적 opt-in
- `과거 유도 경험 있음` → 복귀자용 starter window 또는 기존반 적합성 확인
- `이미 첫 체험 참석` → Second-Visit Trial Bridge 규칙 적용, 중복 영업메시지 금지
- `희망반 만석` → Capacity-Aware Waitlist 규칙 적용

## 6. 실험 설계

### EXP-211 Fixed Starter Window vs Always-On Trial

**가설**  
완전 초보를 정해진 시작창으로 모으면 상시 개별 투입보다 `첫 방문 → 2회차 → 14일 내 등록 → 30일 활성`과 코치 온보딩 효율이 개선될 수 있다.

**비교**
- Control: 현재 상시 체험 방식
- Test: 승인된 초보 입문 시작창 방식

**핵심 KPI**
- inquiry → verified first attendance
- first attendance → second attendance
- first attendance → paid within 14d
- paid → active 30d
- coach onboarding minutes per starter
- existing member class disruption

**성공 기준**  
기준선·최소 개선폭·필요 표본수는 `데이터 미연결`. 실험 시작 전에 확정한다. 30일 활성이나 수업품질이 악화되면 단순 등록 증가만으로 성공 판정하지 않는다.

**중단 기준**
- 시작창 때문에 적합한 신규 문의가 과도하게 오래 대기
- 안전정원 초과
- 기존회원 수업시간 침해
- 코치 온보딩 시간이 오히려 증가
- 입문기수 예약만 늘고 실제 출석·등록·30일 활성 개선이 없음

### EXP-212 Next-Intake CTA

이번 시작창을 놓친 신규 문의에게 `다음 시작일 알림`을 자발적 opt-in으로 제공했을 때, 단순 `마감/다음에 문의` 처리보다 다음 기수 예약·실방문이 개선되는지 측정한다.

- primary: next_intake_opt_in → reservation → verified attendance
- downstream: paid within 14d → active 30d
- 과도한 반복연락 금지

## 7. 대표 승인 전 금지사항

- 실제 시작일·정원·가격·무료체험 조건 확정
- 고객에게 모집 메시지 발송
- 네이버·인스타·당근 실제 게시
- 광고비 집행
- 기존반 시간표 변경
- 운영DB 스키마 배포
- 실제 데이터 없이 `마감임박`, `선착순`, `마지막 자리` 표현 사용

## 8. KPI 현재 상태

- 월 신규 문의 수: **데이터 미연결**
- 완전 초보 비중: **데이터 미연결**
- 상시 체험 예약→실방문: **데이터 미연결**
- 첫 방문→2회차: **데이터 미연결**
- 첫 방문→14일 등록: **데이터 미연결**
- 등록→30일 활성: **데이터 미연결**
- 휴면 복귀→30일 활성: **데이터 미연결**
- 코치 1인당 신규 온보딩 시간: **데이터 미연결**
- 기존회원 만족도/수업품질 영향: **데이터 미연결**
- 귀속매출: **데이터 미연결**

## 9. Auditor

공개 사례는 `고정 입문 시작일`과 `정규반과 분리된 초보과정`이 실제 여러 무도시설에서 사용된다는 근거일 뿐, 그 방식이 파이널유도에서 더 높은 전환율을 만든다는 증거는 아니다. 따라서 파일럿의 핵심은 희소성 마케팅이 아니라 **초보 온보딩 품질, 실제 출석, 등록, 30일 활성, 코치 부담을 함께 비교하는 것**이다.
