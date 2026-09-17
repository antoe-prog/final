# 체험 예약 확인·알림·취소·미방문·재예약·대기자석 회복·귀속 기준 v1

- 문서 상태: 공개 가능한 운영 기준
- 기준일: 2026-09-17
- 적용 대상: 신규 체험, 휴면 복귀 체험, 기존회원의 특별수업 예약
- 적용 목표: 신규 체험 실제 방문, 휴면 복귀, 기존회원 만족·재등록, 순수납 귀속의 정확성
- 데이터 상태: 내부 문의·예약·출석·등록·수납 데이터 미연결

## 1. 목적

체험 신청을 실제 방문으로 오분류하지 않고, 예약 확인부터 취소·재예약·대기자석 회복까지 하나의 검증 가능한 여정으로 관리한다. 알림 발송, 메시지 전달, 고객 확인, 실제 참여, 계약과 실수납을 서로 다른 사건으로 기록한다.

## 2. 핵심 판단

1. 문의, 예약 요청, 예약 확정, 실제 방문은 서로 다른 상태다.
2. 메시지 발송을 전달·열람·응답·참여로 간주하지 않는다.
3. 체크인 누락만으로 미방문을 확정하지 않는다.
4. 사업자 휴강·수업 변경·잘못된 예약·지점 이동을 고객 노쇼로 분류하지 않는다.
5. 예약 운영 알림과 영리 목적 광고성 후속을 분리한다.
6. 취소된 자리를 대기자가 실제 수락·예약·참여했을 때만 회복으로 집계한다.
7. 체험 후 계약, 결제 승인, 실수납과 이용 시작을 분리한다.

## 3. 상태 모델

| 상태 | 의미 |
|---|---|
| `INQUIRY_RECEIVED` | 채널에서 문의가 접수됨 |
| `TRIAL_REQUESTED` | 희망 지점·수업·시간을 요청함 |
| `BOOKING_PENDING` | 정원·자격·일정 확인 중 |
| `BOOKING_CONFIRMED` | 지점·일시·대상·준비사항이 확정됨 |
| `REMINDER_QUEUED` | 확정 예약을 기준으로 알림 생성 |
| `REMINDER_SENT` | 발송 시스템이 전송을 시도함 |
| `REMINDER_DELIVERED` | 제공 가능한 전달 증거가 있음 |
| `ATTENDANCE_CONFIRMED_BY_CUSTOMER` | 고객 또는 적법한 보호자가 참여 의사를 확인함 |
| `CANCELLATION_REQUESTED` | 취소 요청이 접수됨 |
| `CANCELLED` | 예약 시스템에서 취소 완료 |
| `RESCHEDULE_REQUESTED` | 다른 일정 요청이 접수됨 |
| `RESCHEDULED` | 새 일정이 확정되고 이전 예약이 종료됨 |
| `CHECKIN_PENDING` | 수업 종료 후 출석 검증 전 |
| `ATTENDED_VERIFIED` | 실제 참여 증거가 확인됨 |
| `NO_SHOW_CANDIDATE` | 출석 증거가 없으나 예외 검토 전 |
| `NO_SHOW_VERIFIED` | 사업자 취소·지각·대체 참여·체크인 누락을 배제함 |
| `RECOVERY_ELIGIBLE` | 연락 적격성과 목적을 확인함 |
| `REBOOKED_AFTER_NO_SHOW` | 미방문 뒤 새 예약이 확정됨 |
| `REATTENDED_VERIFIED` | 재예약 수업의 실제 참여가 확인됨 |
| `CONTRACTED` | 계약이 성립함 |
| `CASH_COLLECTED` | 환불·취소 전 실수납이 확인됨 |

`BOOKING_CONFIRMED`, `REMINDER_SENT`, `REBOOKED_AFTER_NO_SHOW`를 실제 참여로 사용하지 않는다.

## 4. 예약 확정 최소 필드

- journey_id: 개인 식별정보를 공개하지 않는 내부 여정 식별자
- branch_id: 공식 지점 코드
- class_id와 session_id: 반복 수업과 실제 회차 구분
- audience_type: 성인·미성년자·기타 승인 분류
- scheduled_start_at와 timezone
- capacity_source와 확인 시각
- booking_source: 네이버·Instagram·당근·전화·방문·추천 등
- booking_status와 status_changed_at
- customer_confirmation_status
- cancellation_or_reschedule_path
- attendance_evidence_source
- consent_scope와 수신거부 상태
- owner와 handoff_accepted_at

공개 저장소에는 이름, 연락처, 보호자 정보, 건강정보와 개별 결제 원장을 기록하지 않는다.

## 5. 예약 확인과 준비 안내

예약 확정 메시지는 다음 운영정보만 포함한다.

- 확정된 지점·날짜·시각
- 도착 권장 시각
- 확인된 복장·준비물
- 출입·주차 등 승인된 위치 안내
- 취소·재예약 방법
- 문의 가능한 공식 경로

확인되지 않은 가격·무료 체험·혜택·잔여석·시간표를 넣지 않는다. 건강·부상 상태를 메시지에서 추정하거나 공개 답변으로 요청하지 않는다.

## 6. 알림·광고성 후속 분리

### 6.1 운영 알림

- 이미 확정된 예약의 일시·장소·준비사항·변경 경로를 전달한다.
- 예약 원장과 같은 회차를 참조한다.
- 취소 또는 재예약이 완료되면 이전 알림을 중단한다.
- 발송 실패·수신 거부·잘못된 연락처를 별도 상태로 남긴다.

### 6.2 광고성 후속

- 할인·혜택·추가 상품·기간 한정 등록 권유가 포함되면 운영 알림과 분리한다.
- 관련 법령과 내부 동의 원장에 따라 적격성을 확인한다.
- 예약을 했다는 사실만으로 모든 마케팅 수신 동의를 생성하지 않는다.
- 운영 안내에 광고 문구를 섞어 동의 요건을 우회하지 않는다.

## 7. 미방문 판정

다음 조건을 모두 검토한 후에만 `NO_SHOW_VERIFIED`로 전환한다.

1. 해당 회차가 실제로 운영됐는가
2. 사업자 취소·휴강·코치 변경·장소 변경이 없었는가
3. 고객이 사전에 취소 또는 재예약하지 않았는가
4. 지각·부분 참여·다른 회차 참여가 아니었는가
5. 종이 명부·현장 확인·앱 체크인 중 누락이 없는가
6. 미성년자라면 보호자 권한과 인계 상태가 맞는가

검증 전에는 `NO_SHOW_CANDIDATE`를 유지한다. 미방문 이유를 건강·동기·경제사정으로 추정하지 않는다.

## 8. 재예약 회복

- 미방문 후 연락은 수신 동의·연락 목적·담당자를 먼저 확인한다.
- 사과나 압박을 요구하지 않고 재예약 가능 여부와 공식 경로를 안내한다.
- 새 일정은 `RESCHEDULE_REQUESTED`와 `RESCHEDULED`를 구분한다.
- 새 예약이 잡혀도 실제 참여 전까지 회복 완료로 보지 않는다.
- 휴면회원의 복귀 예약은 신규 체험으로 중복 집계하지 않는다.
- 체험 후 미등록자와 기존회원의 결석을 같은 캠페인으로 섞지 않는다.

## 9. 취소 자리와 대기자석 회복

`취소 완료 → 자리 공개 → 적격 대기자 알림 → 대기자 수락 → 새 예약 확정 → 실제 참여`

- 대기 등록은 예약 확정이 아니다.
- 자동 승격인지 제한시간 내 수락 방식인지 정책을 명시한다.
- 연령·등급·회원권·지점 자격을 확인한다.
- 여러 사람에게 동시에 확정 자리를 약속하지 않는다.
- 자리 알림 발송만으로 회복 좌석으로 집계하지 않는다.
- 실제 참여 전까지 예상 매출이나 확정 매출로 처리하지 않는다.

## 10. 채널별 역할

| 채널 | 적합한 역할 | 주의사항 |
|---|---|---|
| 네이버 | 지점·시간표·예약 경로의 공식 안내 | 신청·승인·공개 반영 상태 분리 |
| Instagram 코치 개인 계정 | 입문 질문 해소와 공식 문의 경로 안내 | 코치 개인 계정이 가격·정원 기준 시스템이 되지 않음 |
| 당근 | 지역 소식과 공식 문의 연결 | 채팅을 예약·방문으로 간주하지 않음 |
| 운영 앱·CRM | 예약·알림·취소·재예약·출석 상태 관리 | 발송·전달·응답·참여 상태 분리 |

승인 대기 Instagram 콘텐츠에 미확정 시간·가격·좌석·무료 체험 문구를 추가하지 않는다.

## 11. KPI

- 예약 확정률 = `BOOKING_CONFIRMED / 적격 TRIAL_REQUESTED`
- 알림 전달률 = `REMINDER_DELIVERED / REMINDER_SENT`
- 확인률 = `ATTENDANCE_CONFIRMED_BY_CUSTOMER / REMINDER_DELIVERED`
- 실제 방문률 = `ATTENDED_VERIFIED / BOOKING_CONFIRMED`
- 검증 미방문율 = `NO_SHOW_VERIFIED / 실제 운영된 확정 예약`
- 미방문 재예약률 = `REBOOKED_AFTER_NO_SHOW / RECOVERY_ELIGIBLE`
- 재방문 회복률 = `REATTENDED_VERIFIED / REBOOKED_AFTER_NO_SHOW`
- 취소 자리 회복률 = `대기자 실제 참여 좌석 / 적격 취소 좌석`
- 체험 등록률 = `신규 CONTRACTED / ATTENDED_VERIFIED`
- 실수납 전환율 = `신규 CASH_COLLECTED / ATTENDED_VERIFIED`

모든 계산에 분자·분모·기간·지점·원천을 표시한다. 분모가 0이거나 원천이 연결되지 않으면 계산하지 않는다.

## 12. EXP-365 — 체험 예약·미방문·재예약·대기자석 귀속 완전성

- 대상: 실제 고객·계정을 사용하지 않는 합성 예약 여정 72건과 합성 수업 회차 48건
- 성공 기준:
  - 문의의 예약 확정 오분류 0건
  - 발송의 전달·확인·참여 오분류 0건
  - 취소·재예약 완료 후 이전 알림 잔존 0건
  - 휴강·사업자 취소의 고객 미방문 오분류 0건
  - 체크인 누락의 `NO_SHOW_VERIFIED` 전환 0건
  - 재예약의 실제 복귀 오분류 0건
  - 대기 알림의 좌석 회복 오분류 0건
  - 휴면 복귀·지점 이동·기존회원의 신규 체험 중복 집계 0건
  - 운영 알림을 이용한 광고 동의 우회 0건
  - 승인 없는 실제 연락·혜택·예약·좌석 변경 0건
- 중단 조건: 개인정보 공개, 보호자 권한 누락, 이중 예약, 잘못된 지점 안내, 동의 없는 광고성 연락
- 방문률·재예약률·등록률·순수납 효과: 데이터 미연결 또는 계산 불가

## 13. 대표 승인·입력 필요 항목

- 지점·수업·회차별 공식 시간표와 정원 기준 시스템
- 예약 확정·취소·재예약·지각·부분 참여 정의
- 운영 알림의 채널·횟수·시점·발신 주체
- 광고성 정보 동의 원장과 수신거부 처리 책임자
- 미성년자 보호자 권한·인계 기준
- 출석 기준 원천과 체크인 누락 검토 책임자
- 대기열 순서·수락 제한시간·자동 승격 정책
- 미방문 후 연락 적격성과 종료 기준
- 신규·휴면 복귀·지점 이동·재등록 정의
- 계약·승인 결제·실수납·환불·차지백 기준

## 14. 공개 안전 기준

공개 저장소에서 다음을 제외한다.

- 회원·보호자 이름, 연락처, 식별 가능한 문의·예약·출석 기록
- 건강·부상·장애·결제·환불 원장
- 실제 발송 대상 목록과 동의 원장
- 로그인 정보·토큰·비밀키
- 승인 대기 Instagram 미디어·캡션
- 미확정 FINAL ROAD, 가격·시간표·행사·혜택·좌석
- 내부 예산·손익·목표 인원

## 15. 근거 출처

- Zen Planner, [Martial arts billing and scheduling software that runs your dojo](https://zenplanner.com/blogs/martial-arts-billing-and-scheduling-software-that-runs-your-dojo/) — 2026-08-06 게시·2026-09-17 확인. 무도장 예약 알림, 취소와 대기자석 연결 사례다. 외부 성과 수치는 내부 목표로 사용하지 않는다.
- Mindbody, [Booking and Scheduling Software for Yoga Studios](https://www.mindbodyonline.com/business/education/blog/booking-scheduling-software-yoga-studios) — 2026-07-23 게시·2026-09-17 확인. 예약·알림·취소·재예약·대기열·출석을 연결하는 체육시설 사례다.
- Zen Planner, [Best CRM tools for studio memberships that convert leads into members](https://zenplanner.com/blogs/best-crm-tools-for-studio-memberships-that-turn-leads-into-long-term-members/) — 2026-07-31 게시·2026-09-17 확인. 체험 예약·미방문 알림·체험 후속을 상태 기반으로 연결하는 사례다. 제시된 외부 전환 주장은 적용하지 않는다.
- Mindbody, [The 30-day Playbook for Converting New Fitness Studio Clients into Long-Term Members](https://www.mindbodyonline.com/business/education/blog/30-day-playbook-turning-new-clients-longterm-members) — 2026-08-20 게시·2026-09-17 확인. 예약 직후 확인과 방문 전 준비 안내 사례다. 외부 할인 예시는 내부 가격·혜택으로 사용하지 않는다.
- 국가법령정보센터, [정보통신망 이용촉진 및 정보보호 등에 관한 법률 제50조](https://www.law.go.kr/) — 2026-09-17 확인. 영리 목적 광고성 정보 전송의 사전 동의 원칙을 운영 알림과 광고성 후속 분리 기준에 반영했다.
- 한국인터넷진흥원, [불법스팸 전송 방지를 위한 정보통신망법 안내서 개정본 발간](https://www.kisa.or.kr/402/form?postSeq=2382) — 2024-03-28 게시·2026-09-17 확인. 광고성 정보 수신 동의 관리 기준을 확인했다.

이 문서는 공개 가능한 운영 기준이며 법률 자문이 아니다. 알림 발송, 고객 연락, 예약·좌석 변경, 혜택 적용 또는 결제를 실행했다는 기록이 아니다.
