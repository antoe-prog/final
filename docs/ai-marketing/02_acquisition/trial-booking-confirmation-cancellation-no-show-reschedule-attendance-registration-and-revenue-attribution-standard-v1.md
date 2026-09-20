# 체험 예약·취소·노쇼·재예약·참석·등록·매출 귀속 기준 v1

- 작성일: 2026-09-20
- 적용 대상: 유도장·체육시설의 신규 체험, 휴면회원 복귀 체험, 기존회원 재참여 예약
- 공개 범위: 개인식별정보·연락처·출석·결제 원장을 포함하지 않는 일반 운영 기준

## 1. 목적

예약 건수와 실제 체험, 등록, 실정산을 분리해 신규 체험 확보·휴면 복귀·재등록·매출을 과장 없이 관리한다. 예약 확인이나 메시지 발송만으로 참석 또는 전환을 확정하지 않는다.

## 2. 표준 상태 흐름

`INQUIRY → BOOKING_REQUESTED → BOOKING_CONFIRMED → REMINDER_ELIGIBLE → REMINDER_SENT → CUSTOMER_CONFIRMED | CANCELLED | RESCHEDULED → ARRIVED → TRIAL_ATTENDED → CONTRACT_ACTIVATED → SETTLED`

상태는 앞 단계의 증거를 보존하되 뒤 단계로 자동 추정하지 않는다.

| 상태 | 최소 증거 | 다음 상태로 보지 않는 것 |
|---|---|---|
| `BOOKING_REQUESTED` | 희망 수업·지점·참여자 구분이 있는 요청 | 예약 확정 |
| `BOOKING_CONFIRMED` | 확정된 지점·세션·일시·정원 반영 | 실제 방문 |
| `REMINDER_SENT` | 채널·발송 시각·메시지 버전 | 전달·열람·응답 |
| `CUSTOMER_CONFIRMED` | 명시적인 참석 의사 응답 | 도착·수련 참여 |
| `CANCELLED` | 취소 주체·시각·대상 예약 | 노쇼 |
| `RESCHEDULED` | 기존 예약 종료와 새 예약 ID 연결 | 참석 |
| `ARRIVED` | 지점·세션에 맞는 체크인 또는 직원 확인 | 수련 완료 |
| `TRIAL_ATTENDED` | 세션 참여 완료 기준 충족 | 계약·결제 |
| `CONTRACT_ACTIVATED` | 유효한 계약·회원권 활성 증거 | 실정산 |
| `SETTLED` | 정산 원장의 성공 거래 | 환불 전 순매출 |

## 3. 취소·노쇼·재예약 규칙

1. 취소는 확정 예약을 참여 전에 명시적으로 종료한 사건이다. 취소 기한과 사유 분류는 대표 승인값을 사용한다.
2. 노쇼는 확정 예약이 있고, 취소·재예약 기록 없이 승인된 유예시간까지 `ARRIVED` 또는 `TRIAL_ATTENDED`가 없는 경우에만 판정한다.
3. 응답 없음은 노쇼가 아니다. 세션 종료와 유예시간 경과가 모두 필요하다.
4. 재예약은 기존 예약을 `RESCHEDULED`로 종료하고 새 예약 ID를 생성한다. 두 예약을 분모에 동시에 넣지 않는다.
5. 지점 변경은 단순 시간 변경과 구분하고, 원지점 예약 종료·수신 지점 승인·새 예약 확정을 모두 기록한다.
6. 대기명단 자동 승격은 예약 확정 사건이다. 이메일 알림은 참석 확인이 아니다.
7. 보호자·결제자·실제 참여자는 별도 역할로 관리하고, 보호자 응답을 참여자의 광고 수신 동의로 확장하지 않는다.

## 4. 세그먼트와 성과 귀속

- 신규: 기준 관찰기간 안에 과거 활성 계약·실제 참여가 없는 참여자
- 휴면 복귀: 대표가 승인한 휴면 기준을 충족한 뒤 다시 `TRIAL_ATTENDED` 또는 정규 참여를 한 사람
- 체험 후 미등록: `TRIAL_ATTENDED`는 있으나 관찰기간 안에 `CONTRACT_ACTIVATED`가 없는 사람
- 재등록: 기존 계약 종료 뒤 승인된 기간 안에 새 계약이 활성화된 사람
- 기존회원 만족·유지: 기존 활성 계약 상태에서 참여가 이어지는 경우로 신규·복귀와 중복 계산하지 않음

한 사람의 같은 세션·계약·정산은 한 지점과 한 주 귀속 경로에만 직접 귀속한다. 보조 접점은 별도 기록하되 매출을 복제하지 않는다.

## 5. 메시지·후속 작업 통제

- 예약 확인, 변경, 취소, 준비물 안내와 광고성 재방문 제안은 목적과 동의 근거를 분리한다.
- 발송 후보 생성, 승인, 실제 발송, 전달, 고객 응답을 각각 기록한다.
- 자동 발송은 대표가 승인한 대상·채널·시간대·문구·중단 규칙이 모두 있을 때만 허용한다.
- 취소·노쇼 후 후속 제안은 명시된 연락 허용 범위와 빈도 제한을 적용한다.
- 이 문서는 고객 메시지 발송이나 재예약 실행을 승인하지 않는다.

## 6. 최소 데이터 사전

| 영역 | 필수 필드 |
|---|---|
| 예약 | `booking_id`, `person_id`, `participant_role`, `branch_id`, `session_id`, `created_at`, `confirmed_at`, `source` |
| 변경 | `old_booking_id`, `new_booking_id`, `change_type`, `requested_at`, `approved_at` |
| 취소·노쇼 | `cancelled_at`, `cancel_actor`, `cutoff_version`, `grace_end_at`, `no_show_decided_at` |
| 메시지 | `message_id`, `purpose`, `consent_basis`, `approved_version`, `sent_at`, `delivery_status`, `reply_at` |
| 참여 | `arrival_at`, `attendance_evidence`, `trial_attended_at`, `staff_verifier` |
| 계약·정산 | `contract_id`, `activated_at`, `payment_id`, `settled_at`, `refund_id`, `chargeback_id` |
| 귀속 | `primary_source`, `assist_source`, `branch_id`, `attribution_window_version` |

실제 데이터가 연결되지 않은 지표는 `데이터 미연결`로 표시한다. 분모가 0이거나 상태가 결측이면 비율을 계산하지 않는다.

## 7. KPI 정의

| KPI | 분자 | 분모 |
|---|---|---|
| 예약 확정률 | `BOOKING_CONFIRMED` 고유 예약 | 유효 `BOOKING_REQUESTED` |
| 사전 취소율 | 기한 내 `CANCELLED` 고유 예약 | `BOOKING_CONFIRMED` |
| 재예약 완료율 | 새 예약이 확정된 고유 참여자 | `RESCHEDULED` 고유 참여자 |
| 노쇼율 | 승인 규칙으로 확정된 노쇼 | 취소·재예약 제외 확정 예약 |
| 실제 체험률 | `TRIAL_ATTENDED` 고유 참여자 | 취소·재예약 제외 확정 예약 |
| 체험→계약 활성률 | 관찰기간 내 `CONTRACT_ACTIVATED` | `TRIAL_ATTENDED` 고유 참여자 |
| 계약→실정산률 | `SETTLED` 고유 계약 | `CONTRACT_ACTIVATED` 고유 계약 |
| 순수납 | 성공 정산 | 환불·차지백·승인된 조정 차감 |

## 8. 채널 적용

- 네이버·당근·전화·웹 예약: 채널 클릭이나 대화 시작을 예약으로 계산하지 않는다.
- Instagram 코치 개인 계정: 프로필 방문·DM 버튼·댓글을 공식 지점 문의 또는 예약으로 자동 분류하지 않는다.
- 다지점: 지점 미확정 문의는 중앙 대기 상태로 두고, 담당 지점이 확정된 뒤 예약 상태를 생성한다.

## 9. EXP-435 — 예약부터 순수납까지 판정 정확도

합성 지점 8개, 사람 1,200명, 예약 사건 3,600건, 메시지 사건 7,200건, 출석·계약·정산 사건으로 검증한다.

성공 기준은 다음 오류가 모두 0건인 것이다.

- 예약 요청을 확정으로 계산
- 리마인더 예약을 실제 발송으로 계산
- 발송을 전달·열람·고객 확인으로 계산
- 무응답을 취소 또는 노쇼로 계산
- 기한 내 취소를 노쇼로 계산
- 재예약의 구예약과 신예약을 분모에 중복 포함
- 체크인을 실제 체험 완료로 계산
- 체험을 계약 활성 또는 실정산으로 계산
- 휴면 복귀·재등록을 신규로 중복 귀속
- 한 정산을 여러 채널·지점에 중복 귀속
- 보호자 연락처를 참여자의 광고 동의로 변환
- 환불·차지백을 순수납에서 누락

하나라도 실패하면 자동 리마인더·재예약·대기명단 이동, 고객 발송, 노쇼율·전환율·매출 보고를 중단한다.

## 10. 대표 승인 체크리스트

- 예약 확정의 공식 증거
- 취소 기한과 노쇼 유예시간
- 재예약 시 구예약 종료 규칙
- 도착·실제 체험 완료의 증거
- 서비스 안내와 광고성 메시지 구분
- 리마인더 채널·시간·빈도·문구 승인자
- 대기명단 승격과 지점 이관 책임자
- 신규·휴면·재등록의 관찰기간
- 계약 활성·실정산·환불·차지백 원장

## 11. 근거

- Gymdesk, [How to Make Sessions Bookable](https://docs.gymdesk.com/en/help/docs/booking) — 회원·방문자·리드 예약, 정원, 가격, 첫 방문 저장 방식, 대기명단과 취소 시 자동 승격을 구분. 2026-09-20 확인.
- Gymdesk, [Attendance Tracking](https://docs.gymdesk.com/en/help/docs/attendance-tracking) — 회원·방문자 체크인과 세션별 출석 로그를 별도 관리. 2026-09-20 확인.
- Gymdesk, [Gym Dashboard Overview](https://docs.gymdesk.com/en/help/docs/dashboard) — 예약·체크인·결제·후속 작업을 서로 다른 운영 신호로 표시. 2026-09-20 확인.
- Google, [Business Profile performance](https://support.google.com/business/answer/9918094?hl=en) — 프로필의 조회·클릭·예약 등 상호작용 지표와 실제 오프라인 참여를 구분할 때 참고. 2026-09-20 확인.

## 12. 감사 범위

이 기준에는 회원·보호자 식별정보, 연락처, 개인별 출석·건강·결제 원장, 승인되지 않은 가격·혜택·시간표, 승인 대기 Instagram 미디어·캡션, 미확정 행사 자료를 포함하지 않는다.
