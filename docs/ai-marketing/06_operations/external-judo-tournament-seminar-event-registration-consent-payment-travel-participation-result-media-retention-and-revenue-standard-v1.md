# 외부 대회·세미나·행사 참가 신청·동의·결제·실참여·결과·후속 귀속 기준 v1

- 문서 버전: v1
- 공개 검토일: 2026-09-20
- 적용 대상: 파이널유도멀티짐의 외부 대회, 합동훈련, 세미나, 캠프 및 유사 행사 운영
- 제외 대상: 승인되지 않은 내부 행사 기획, 실제 참가자 명단, 연락처, 건강·부상자료, 개인별 결제자료, 내부 예산·손익

## 1. 목적

행사 공지부터 사후 참여까지를 하나의 상태로 뭉치지 않고 다음 사건으로 분리한다.

1. 행사 정보 확인
2. 참가 의향
3. 자격 검토
4. 신청 제출
5. 보호자·개인정보·촬영 등 필요한 동의
6. 참가비 청구
7. 실제 정산
8. 이동·준비 확인
9. 현장 체크인
10. 실제 참여
11. 공식 결과 확인
12. 결과·사진 공개 권한 확인
13. 후속 수업 참여
14. 실제 재등록과 순수납

이 분리는 신규 체험, 기존회원 만족, 휴면회원 복귀, 재등록 및 매출을 과장 없이 측정하기 위한 최소 기준이다.

## 2. 핵심 판단

- 공지는 신청이 아니다.
- 관심 표현은 참가 의향일 뿐 자격 확정이 아니다.
- 신청 제출은 접수 완료나 출전 확정이 아니다.
- 청구서 생성은 결제가 아니다.
- 결제 승인은 실제 정산이 아니다.
- 차량 배정은 탑승이나 도착이 아니다.
- 현장 체크인은 실제 경기·세미나 참여가 아니다.
- 내부 기록은 공식 결과가 아니다.
- 결과 게시 권한과 얼굴·이름 공개 동의는 별개다.
- 참가나 입상은 재등록이 아니다.
- 행사 관련 수납과 회원권 매출을 중복 귀속하지 않는다.

## 3. 객체 모델

### 3.1 `event`

- `event_id`
- `event_type`: 대회 / 세미나 / 합동훈련 / 캠프 / 기타
- `organizer_id`
- `official_source_url`
- `event_title`
- `venue`
- `start_at`
- `end_at`
- `registration_deadline`
- `eligibility_rule_version`
- `status`: draft / approved / open / closed / changed / cancelled / completed

### 3.2 `event_version`

- `event_version_id`
- `event_id`
- `source_snapshot_at`
- `source_document_id`
- `effective_at`
- `change_type`
- `changed_fields`
- `verified_by`

일시·장소·체급·연령·준비물·비용·접수 마감 변경을 원본 행사와 분리해 보존한다.

### 3.3 `participant`

- `participant_id`
- `person_id`
- `member_id`
- `guardian_relationship_id`
- `home_location_id`
- `program_id`
- `rank_or_category_at_application`

공개 문서와 분석 테이블에는 식별정보 대신 내부 키만 사용한다.

### 3.4 `eligibility_review`

- `eligibility_review_id`
- `participant_id`
- `event_id`
- `rule_version`
- `review_status`: pending / eligible / ineligible / exception_review
- `reviewed_at`
- `reviewed_by`
- `reason_code`

출석, 등급, 연령, 체급, 코치 추천 중 하나만으로 자동 확정하지 않는다.

### 3.5 `application`

- `application_id`
- `event_id`
- `participant_id`
- `submitted_at`
- `submitted_by_role`
- `application_status`: draft / submitted / accepted / waitlisted / rejected / withdrawn / cancelled
- `organizer_reference`

### 3.6 `consent`

- `consent_id`
- `participant_id`
- `event_id`
- `consent_type`: participation / guardian / privacy / medical_emergency / transport / media
- `policy_version`
- `granted_at`
- `expires_at`
- `withdrawn_at`
- `scope`

하나의 동의를 다른 목적에 재사용하지 않는다.

### 3.7 `invoice_and_payment`

- `invoice_id`
- `application_id`
- `charge_type`: organizer_fee / transport / accommodation / equipment / gym_service
- `invoice_status`
- `payment_attempt_id`
- `payment_status`
- `settled_at`
- `refund_id`
- `chargeback_id`
- `net_cash_amount`

외부기관 전달금과 도장 수익을 구분한다.

### 3.8 `travel_and_arrival`

- `travel_plan_id`
- `participant_id`
- `transport_mode`
- `guardian_handoff_required`
- `departure_check_in_at`
- `venue_arrival_at`
- `return_handoff_at`
- `incident_reference`

차량 좌석 예약이나 단체 채팅 참여를 실제 이동·도착으로 간주하지 않는다.

### 3.9 `participation`

- `participation_id`
- `application_id`
- `venue_check_in_at`
- `participation_status`: no_show / checked_in / participated / partial / withdrawn / disqualified / cancelled_by_organizer
- `participation_started_at`
- `participation_ended_at`
- `verified_by`

### 3.10 `result`

- `result_id`
- `participation_id`
- `result_source`
- `result_status`: pending / official / corrected / void
- `placement_or_completion`
- `confirmed_at`
- `published_at`

구두 전달, 사진, 비공식 대진표는 공식 결과와 분리한다.

### 3.11 `media_asset`

- `asset_id`
- `event_id`
- `captured_at`
- `subjects_count`
- `identifiable_subjects`
- `consent_check_status`: not_required / pending / verified / denied / withdrawn
- `approved_channels`
- `retention_until`
- `published_url`

단체사진이라도 식별 가능한 사람이 있으면 공개 범위를 확인한다.

### 3.12 `follow_up_outcome`

- `follow_up_id`
- `participant_id`
- `event_id`
- `follow_up_type`: feedback / recovery / next_class / renewal / reactivation / referral
- `assigned_at`
- `completed_at`
- `outcome_event_id`

행사 후 메시지 발송과 사람의 응답, 실제 수업 참여, 실제 재등록을 분리한다.

## 4. 상태 전이

### 4.1 행사

`draft → approved → open → closed → completed`

예외 전이:

- `open → changed`
- `open → cancelled`
- `closed → changed`
- `completed → corrected`

### 4.2 신청

`draft → submitted → accepted | waitlisted | rejected`

종료 전이:

- `submitted → withdrawn`
- `accepted → cancelled`
- `waitlisted → accepted`

### 4.3 결제

`invoice_created → payment_attempted → authorized → settled`

예외 전이:

- `payment_attempted → failed`
- `settled → partially_refunded | refunded | disputed | charged_back`

### 4.4 참여

`accepted → checked_in → participated`

예외 전이:

- `accepted → no_show`
- `checked_in → partial`
- `checked_in → withdrawn`
- `accepted → cancelled_by_organizer`

### 4.5 결과

`pending → official → published`

예외 전이:

- `official → corrected`
- `official → void`

## 5. 퍼널 정의

### 5.1 신규 체험 획득

행사 콘텐츠 노출 → 유효 문의 → 체험 예약 확정 → 실제 체험 참여 → 신규 회원권 실정산

외부 행사 참가자를 자동으로 신규회원으로 계산하지 않는다.

### 5.2 휴면회원 복귀

복귀 대상 판정 → 연락 적법성 확인 → 사람 응답 → 행사 또는 수업 예약 → 실제 참여 → 복귀 회원권 실정산

행사 관심이나 신청만으로 복귀를 확정하지 않는다.

### 5.3 기존회원 유지·재등록

행사 참가 → 다음 정규수업 실제 출석 → 관찰기간 내 재등록 실정산

행사 참가와 재등록 사이의 관찰기간과 비교군이 없으면 인과 효과를 주장하지 않는다.

### 5.4 매출

행사 순수납 = 행사 관련 실정산
- 환불
- 차지백
- 외부기관 전달금
- 직접 변동비

회원권 순수납은 별도 원장으로 유지한다.

## 6. 자격·안전·동의 가드레일

### 6.1 자격

- 최신 주최기관 공고와 적용 버전을 저장한다.
- 연령·체급·등급·소속·선수등록 등 실제 요구사항을 임의 생성하지 않는다.
- 코치 추천과 공식 참가 자격을 구분한다.
- 예외 승인은 승인자와 근거를 기록한다.

### 6.2 미성년자

- 참여자, 보호자, 계약자, 결제자를 분리한다.
- 법정대리인 권한 확인과 각 동의 목적을 분리한다.
- 보호자 동의가 촬영·광고·건강정보 공개까지 자동 확장되지 않도록 한다.

### 6.3 건강·부상

- 공개 문서에 개인별 건강·부상정보를 넣지 않는다.
- 행사 운영에 필요한 최소 범위만 권한이 있는 담당자가 확인한다.
- 응급 대응 기록과 마케팅 분석을 분리한다.
- 부상 발생을 참여 실패나 이탈로 자동 분류하지 않는다.

### 6.4 이동·인계

- 출발·도착·귀가 인계 책임자를 정한다.
- 차량 배정과 실제 탑승을 분리한다.
- 보호자 인계 완료 전 자동 종료 처리하지 않는다.
- 일정 변경 시 기존 참가자에게 별도 안내 여부를 기록한다.

### 6.5 촬영·공개

- 촬영 동의와 공개 동의를 분리한다.
- 채널별 공개 범위를 확인한다.
- 결과 공개와 얼굴·이름 공개를 분리한다.
- 동의 철회 후 삭제·비공개 처리 이력을 남긴다.

## 7. 채널별 공개 기준

### 7.1 네이버

- 주최기관, 행사명, 일시, 장소, 대상, 신청 마감, 비용, 취소 기준의 승인 버전만 사용한다.
- 변경일과 확인 원천을 표시한다.
- 선착순·마감 임박은 실제 정원과 접수 상태가 연결된 경우에만 쓴다.

### 7.2 Instagram

- 코치 개인 계정에서는 코치의 준비·관찰·교육 설명 맥락을 우선한다.
- 브랜드 공식 발표 주체처럼 쓰지 않는다.
- 참가자 얼굴·이름·결과는 공개 권한이 확인된 범위에서만 사용한다.
- 입상·성장·재등록을 보장하는 표현을 쓰지 않는다.

### 7.3 당근

- 지역, 대상, 접수 방법, 실질적 조건을 명확히 한다.
- 행사 신청을 회원권 가입으로 오인시키지 않는다.
- 가격·혜택·무료 표현은 승인된 조건과 일치해야 한다.

## 8. 역할과 책임

### CMO

- 행사 목적을 신규, 복귀, 유지, 매출 중 하나의 1차 목표로 지정한다.
- 안전·개인정보·운영 역량이 확보되지 않으면 홍보를 보류한다.

### Growth Analyst

- 관심, 신청, 수납, 체크인, 참여, 결과, 후속 참여, 재등록을 분리한다.
- 분자·분모·기간·원천을 표시한다.

### Acquisition Marketer

- 승인된 공개 조건만 채널별로 변환한다.
- 문의와 신청, 참가와 등록을 혼동하지 않는다.

### CRM Marketer

- 보호자·참여자·결제자와 연락 동의를 확인한다.
- 서비스 안내와 광고성 후속을 구분한다.

### Content Strategist

- 준비 과정, 안전한 참여, 학습 포인트, 사후 회고를 순환한다.
- 미확정 일정·결과·혜택은 초안에 넣지 않는다.

### Experiment Manager

- 관찰기간과 성공·중단 기준을 사전에 고정한다.
- 성과가 없는 경우에도 결과를 보존한다.

### Auditor

- 자격·비용·결과·미디어 동의·매출 귀속의 근거를 검사한다.
- 개인정보와 내부 자료가 공개 저장소에 포함되지 않았는지 확인한다.

## 9. KPI 정의

| KPI | 분자 | 분모 | 제외 |
|---|---|---|---|
| 신청 완료율 | 제출된 신청 | 유효 참가 의향 | 중복·시험 신청 |
| 자격 승인율 | 적격 승인 | 사람 검토 완료 | 검토 대기 |
| 정산 완료율 | 실정산 신청 | 청구된 신청 | 면제·취소 |
| 실참여율 | 실제 참여 | 출전·참가 확정 | 주최 취소 |
| 다음 수업 참여율 | 관찰기간 내 실제 출석 | 성숙한 실제 참여자 | 관찰기간 미성숙 |
| 재등록률 | 관찰기간 내 재등록 실정산 | 성숙한 재등록 대상 | 기존 장기계약 비대상 |
| 행사 순수납 | 실정산-차감액 | 해당 없음 | 외부기관 전달금 |
| 미디어 준수율 | 동의 확인 후 공개 자산 | 식별 가능 공개 자산 | 식별 불가 자산 |

분모가 0이거나 원천이 연결되지 않으면 `계산 불가`로 표시한다.

## 10. EXP-421 — 행사 상태·동의·참여·후속 귀속 정확도

### 목적

실제 고객·회원자료를 사용하기 전에 합성 데이터로 상태 오분류와 중복 귀속을 검증한다.

### 합성 데이터

- 행사 120건
- 행사 버전 480건
- 참여자 600명
- 자격 검토 1,200건
- 신청 1,800건
- 동의 4,800건
- 청구·결제 3,600건
- 이동·체크인·참여 2,400건
- 결과 1,200건
- 후속 수업·재등록 사건 2,400건

### 성공 기준

- 공지의 신청 완료 오분류 0건
- 참가 의향의 자격 확정 오분류 0건
- 신청 제출의 출전 확정 오분류 0건
- 보호자 동의의 촬영·광고 동의 재사용 0건
- 청구·승인의 실정산 오분류 0건
- 차량 배정의 실제 탑승·도착 오분류 0건
- 체크인의 실제 참여 오분류 0건
- 비공식 결과의 공식 결과 오분류 0건
- 참가·입상의 재등록 자동 승격 0건
- 행사비·외부기관 전달금·회원권 매출 중복 귀속 0건
- 환불·차지백 미차감 0건
- 동의 미확인 식별 미디어의 공개 후보 0건

### 중단 기준

위 항목 중 하나라도 발생하면 실제 자동 신청 확정, 결제·참여·결과 안내, 미디어 공개, 재등록 후속 및 성과 보고를 중단한다.

## 11. 필요한 대표 승인·입력

- 행사 유형별 승인자
- 공식 주최기관과 공고 확인 원천
- 참가 자격과 예외 승인 절차
- 미성년자 참여·개인정보·건강·이동·촬영 동의 버전
- 신청·대기·취소·환불 기준
- 외부기관 납부액과 도장 수익 구분
- 이동·인계·비상 연락 책임
- 결과 확인과 정정 책임자
- 채널별 공개 승인자
- 행사 후 참여·재등록 관찰기간
- CRM·예약·출석·결제·회계 기준 원장

## 12. 구현 순서

1. 행사와 행사 버전 테이블 생성
2. 참여자·보호자·결제자 관계 연결
3. 자격 검토와 신청 상태 분리
4. 동의 종류별 버전·범위·철회 기록
5. 청구·승인·정산·환불 연결
6. 이동·도착·체크인·실참여 사건 연결
7. 공식 결과 원천과 정정 이력 연결
8. 미디어 자산과 공개 권한 연결
9. 다음 수업·재등록·순수납 관찰
10. EXP-421 통과 후 제한적 운영

## 13. 공개 전 체크리스트

- [ ] 최신 공식 공고와 버전을 확인했다.
- [ ] 승인되지 않은 행사 정보를 제거했다.
- [ ] 신청·결제·참여·결과 상태를 분리했다.
- [ ] 미성년자·보호자·결제자 역할을 분리했다.
- [ ] 필요한 동의의 범위와 버전을 확인했다.
- [ ] 가격·환불·마감·정원 표현의 근거가 있다.
- [ ] 식별 가능한 사진·영상 공개 권한을 확인했다.
- [ ] 실제 결과와 정정 가능성을 반영했다.
- [ ] 행사 순수납과 회원권 매출을 중복 계산하지 않는다.
- [ ] 개인정보·건강·결제·내부 손익자료가 공개 파일에 없다.

## 14. 근거 출처

- Gymdesk, [Schedule Management](https://docs.gymdesk.com/en/help/docs/schedule) — 일정은 공개 노출, 예약, 출석, 이용권한에 연결되며 기존 참가자는 일정 변경만으로 자동 안내되지 않는다. 2026-09-20 확인.
- Gymdesk, [How to Make Sessions Bookable](https://docs.gymdesk.com/en/help/docs/booking) — 회원·방문자·리드, 예약 옵션, 정원, 가격, 대기와 청구서 생성을 분리한다. 2026-09-20 확인.
- Gymdesk, [Digital Point-of-sale](https://docs.gymdesk.com/en/help/docs/pos) — 상품·장비 거래와 결제 설정을 별도로 관리한다. 2026-09-20 확인.
- IJF, [Coaching without Boundaries](https://www.ijf.org/news/show/coaching-without-boundaries) — 대회 당일 코치 업무에 선수의 경기 시각·경기 순서 등 주변 운영을 포함하는 사례를 제시한다. 2026-09-20 확인.
- IJF, [Documents](https://www.ijf.org/ijf/documents/25) — 경기·교육 관련 공식 문서의 확인 원천. 2026-09-20 확인.

## 15. 감사 메모

이 문서는 공개 가능한 일반 운영 기준이다. 실제 참가자·보호자 정보, 연락처, 건강·부상자료, 개인별 결제·출석·결과 원장, 미확정 행사, 내부 금액, 승인 대기 Instagram 미디어·캡션을 포함하지 않는다.
