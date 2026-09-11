# Capacity-Aware Waitlist & Alternate-Slot Routing v1

- 작성일: 2026-09-11
- 목적: 만석·준만석 수업의 신규 문의를 단순 거절하지 않고, 실제 여석이 있는 대체 시간·대기목록·다음 입문기수로 안전하게 라우팅해 신규 체험 기회를 보존한다.
- 상태: 설계 완료 / 고객 연락·예약정책 변경·앱 기능 적용·광고 집행 전
- 공개 범위: 개인 식별정보, 연락처, 개인별 출석·결제 원장, 비밀키·토큰은 포함하지 않는다.

## 1. 왜 지금 필요한가

기존 Capacity-First 원칙은 실제 안전정원과 체험 가능석이 확인된 반만 모집하도록 설계한다. 그러나 인기 시간대가 만석일 때 단순히 `마감`으로 끝내면 이미 발생한 수요를 잃을 수 있다.

2026년 9월 현재 St Albans Judo Club은 여러 유소년 반에 `WAITING LIST IN OPERATION`을 표시하고, 2026년 9월 Mini Judo 입문이 이미 가득 찬 경우 2027년 1월 시작 사전등록을 받고 있다. Singapore Judo Club도 원하는 반이 가득 찬 경우 별도 waitlist form을 제출하면 자리가 생길 때 알림을 받도록 운영한다. Prince Martial Arts Academy의 현재 온라인 시간표에서도 일부 세션에 `Book Waitlist`가 실제 노출된다.

이 사례들은 `만석 = 수요 종료`가 아니라 `대체 가능한 다음 행동을 제시`하는 운영이 실제 무도시설에서 사용되고 있음을 보여준다. 다만 외부 시설의 대기전환율이나 매출효과를 파이널유도의 예상성과로 사용하지 않는다.

## 2. 운영 원칙

### 원칙 A — 안전정원을 절대 초과하지 않는다
대기목록은 과예약(overbooking)을 위한 장치가 아니다. 안전정원과 코치 수용범위를 넘는 예약은 허용하지 않는다.

### 원칙 B — 대체 가능한 여석을 먼저 제안한다
희망 반이 가득 찬 경우 아래 순서로 처리한다.

1. 같은 대상·수준의 다른 시간대에 실제 체험 가능석이 있는지 확인
2. 대체시간이 있으면 선택지 제공
3. 대체시간이 없거나 고객이 원치 않으면 대기목록 opt-in 제공
4. 장기 만석이면 다음 입문기수 또는 다음 모집창구 사전알림 opt-in 제공

### 원칙 C — 자동 등록이 아니라 명확한 동의 기반
대기자 연락은 본인이 대기 알림을 신청한 경우에만 허용한다. 연락수단·보관기간·수신해제 방법을 운영정책에 맞춰 관리한다.

### 원칙 D — 기존회원 경험을 우선 보호한다
신규 체험석 확보를 이유로 기존회원 정규수업의 안전·수업밀도·지도품질을 희생하지 않는다.

## 3. 권장 상태값

`AVAILABLE` → 실제 체험 가능석 있음

`NEAR_CAPACITY` → 체험 가능석이 매우 제한적이나 아직 안전정원 이내

`FULL_ALT_AVAILABLE` → 희망 반은 만석이나 대체 가능한 다른 반 존재

`WAITLIST_OPEN` → 대체 반이 없거나 고객이 원치 않아 대기 신청 가능

`NEXT_INTAKE_ONLY` → 단기간 자리가 날 가능성이 낮아 다음 입문기수 안내만 가능

`CLOSED` → 안전·코치·운영상 신규 수요 수용 불가

## 4. 최소 데이터 구조

개인식별정보는 공개 문서에 넣지 않고 운영시스템 내부에서만 처리한다.

- branch_id
- class_id
- class_segment
- safe_capacity
- normal_member_count
- trial_capacity
- confirmed_trial_count
- available_trial_slots
- requested_slot
- alternate_slot_offered
- alternate_slot_accepted
- waitlist_opt_in
- waitlist_joined_at
- seat_opened_at
- seat_offer_sent_at
- seat_claimed_at
- verified_attendance
- paid_within_14d
- active_30d
- renewal_status

## 5. 퍼널

`local_search/content → inquiry → requested_slot → capacity_check → alternate_slot OR waitlist → reservation → verified_attendance → paid_within_14d → active_30d → renewal`

만석 수업만 따로 보면:

`full_slot_request → alternate_offered → alternate_attended`

또는

`full_slot_request → waitlist_opt_in → seat_opened → seat_claimed → attended`

## 6. 채널 적용

### 네이버
- 만석 시간대를 계속 `체험 가능`으로 홍보하지 않는다.
- 실제 대체 가능 시간이 있으면 FAQ·소개문에 대상과 시간대 선택 구조를 명확히 한다.
- 네이버에 공식 waitlist 기능이 유도 업종에서 현재 제공되는지는 이번 조사에서 확인하지 못했으므로, 제공된다고 가정하지 않는다.

### 인스타그램
- `이번 주 가능 시간`과 `마감 시간`을 사실 기반으로 분리한다.
- 마감임박 표현은 실제 여석 데이터가 있을 때만 사용한다.

### 당근
- 지역 모집글은 실제 여석이 있는 반만 CTA를 열고, 만석 반은 대체시간 또는 사전알림 선택지만 제공한다.

### 자체 앱·운영플랫폼
- 향후 예약 기능 고도화 시 `WAITLIST_OPEN`, `seat_opened`, `seat_claimed`를 별도 상태로 관리할 수 있다.
- 자동 알림·자동 승급은 대표 승인 및 개인정보·메시지 정책 확정 전 구현·실행 완료로 간주하지 않는다.

## 7. 실험

### EXP-207 Full-Class Alternate Routing

**가설**  
만석 문의에 `불가`만 안내하는 것보다 실제 여석이 있는 대체시간을 제시하면 신규 수요를 안전정원 안에서 보존할 수 있다.

**주 KPI**
- full_slot_request → alternate_slot_accepted
- alternate_slot_accepted → verified_attendance
- verified_attendance → paid_within_14d
- paid_within_14d → active_30d

**중단 조건**
- 대체시간 정보 오류
- 안전정원 초과
- 기존회원 수업품질 저하
- 고객이 요청하지 않은 반복 연락

### EXP-208 Waitlist-to-Seat Recovery

**가설**  
대기 신청을 받은 뒤 실제 좌석이 생겼을 때 순차적으로 안내하면 기존에 잃던 수요 일부를 실방문으로 회수할 수 있다.

**주 KPI**
- waitlist_opt_in → seat_offer
- seat_offer → seat_claimed
- seat_claimed → verified_attendance
- verified_attendance → paid_within_14d

**판단 원칙**  
대기 등록자 수 증가만으로 성공 처리하지 않는다. 실방문·등록·30일 활성까지 확인한다.

## 8. 필요한 내부 입력

현재 모두 데이터 미연결이다.

- 지점별 안전정원
- 반별 정상회원 평균 실출석
- 반별 체험 허용석
- 만석 문의 건수
- 대체 가능 시간대
- 취소·노쇼로 다시 열린 좌석 수
- 대기목록 수신동의 방식
- 예약→실방문→등록→30일 활성 전환

## 9. Auditor 기준

- 인위적으로 자리를 적게 열어 가짜 희소성을 만들지 않는다.
- `마감임박`, `마지막 자리`, `대기 N명`은 실제 데이터 없이는 공개하지 않는다.
- 대기자라고 해서 회원보다 우선권을 자동 부여하지 않는다.
- 과예약을 통한 수익 극대화를 권장하지 않는다.
- 외부 피트니스 소프트웨어가 제시하는 높은 fill-rate·매출회수 수치는 판매자료 성격이 있으므로 파이널유도 목표치로 복사하지 않는다.
- St Albans, Singapore Judo Club, Prince Martial Arts Academy의 운영사례는 구조 참고용이며 전환성과 보장 근거가 아니다.

## 10. 공개 근거

- St Albans Judo Club — 현재 여러 유소년 반 waitlist 운영, 2026년 9월 Mini Judo 마감 후 2027년 1월 사전등록: https://www.stalbansjudo.org.uk/
- Singapore Judo Club FAQ — 만석 반에 대한 waitlist form과 자리 발생 시 알림: https://www.judo.sg/faq
- Prince Martial Arts Academy — 현재 온라인 일정에서 일부 수업 `Book Waitlist` 제공: https://prince-martial-arts-academy.gymdesk.com/
- Mindbody Product Team — late cancellation 시 waitlist auto-add 또는 first-to-claim 자동화 설명: https://www.mindbodyonline.com/business/education/product-waitlist-improvements
- ABC Glofox Scheduling — dynamic waitlists, automated waitlist confirmations, no-show reporting 제공: https://www.glofox.com/features/scheduling/

## 11. 이번 회차 결론

파이널유도의 다음 성장 단계는 모든 문의를 더 많이 모으는 것이 아니라, 이미 발생한 문의를 `실제 수용 가능한 자리`로 정확히 라우팅하는 것이다. 인기 시간대가 만석이면 광고를 계속 밀어 넣지 않고, 대체 반 → 대기목록 → 다음 입문기수 순으로 수요를 보존한다. 실제 회원수·반별 정원·전환율·매출은 데이터 미연결이며, 이 문서는 실행완료가 아니라 검토 가능한 공개 설계안이다.
