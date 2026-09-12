# Adult Flex Pass & Cannibalization Guardrail v1

- 작성일: 2026-09-13 (KST)
- 상태: 전략·계측·실험 설계 / 상품 생성·가격 변경·결제 설정·고객 발송·광고 집행 전
- 목표: 신규 성인 체험회원 확보, 휴면 성인 복귀, 기존회원 재등록 보호, 저약정 유료 진입상품의 순기여 검증
- 내부 회원수·매출·전환율: **데이터 미연결**

## 오늘의 판단

파이널유도멀티짐의 다음 성장 과제는 체험을 더 많이 뿌리는 것이 아니라, **체험 이후 또는 복귀 의향이 있으나 월회원권 약정·일정 적합성 때문에 즉시 등록하지 않는 성인에게 저약정 유료 진입경로가 실제로 필요한지 검증하는 것**이다.

이번 문서는 기존 전략과 범위가 다르다.

- `trial-pricing-free-vs-paid-credit-pilot-v1.md`: 첫 체험 자체의 무료/유료 가격 설계
- `second-visit-trial-bridge-v1.md`: 첫 체험 뒤 두 번째 방문을 별도 전환단계로 관리
- `q4-schedule-fit-renewal-protection-v1.md`: 시간대 충돌을 반 이동·복귀 슬롯으로 해결
- 본 문서: **체험/복귀 이후 월회원권 전환 전의 저약정 유료 패스가 증분매출과 30일 활성을 만들고 기존 월회원권을 잠식하지 않는지 검증**

핵심은 `3회권/5회권을 만들자`가 아니다. 실제 회차, 유효기간, 가격, 대상은 모두 대표 승인 전 미정이다. 먼저 어떤 고객의 어떤 장벽을 해결하는 상품인지, 월회원권 다운그레이드가 생기는지, 좌석과 코치시간을 잠식하는지를 측정한다.

## 발견한 변화와 공개 사례

### 1. 2026년 9월 격투기 시설에서 drop-in / class pack을 정식 상품군으로 운영하는 사례가 확인된다

Jaiyen MMA는 2026년 9월 현재 무에타이·BJJ·MMA·복싱을 운영하면서 월회원권과 함께 `Drop-in / packs`를 별도 상품군으로 공개하고 있다. 사이트는 고객이 관심 패키지로 월회원권뿐 아니라 `Drop-in / packs`도 직접 선택할 수 있게 하고 있다.

출처: https://jaiyenmma.com/

이는 파이널유도가 동일 가격이나 동일 구조를 복제해야 한다는 뜻이 아니다. 참고 포인트는 **정규 월회원권과 저약정 이용권을 서로 다른 수요에 대응하는 상품으로 분리**한다는 점이다.

### 2. 서울에서도 1회·3회·5회 패스를 앱 예약과 결합한 피트니스 상품이 실제 판매된다

REVL Training Gangnam의 현재 판매 페이지는 1회, 3회, 5회 패스를 구분하고, 1회권과 다회권의 사용기한을 별도로 두며, 패스 구매 후 앱에서 실제 수업을 예약하도록 한다.

출처: https://creatrip.com/en/spot/15816

이는 유도 가격의 근거가 아니다. 파이널유도에 필요한 참고점은 **패스 구매와 실제 수업 좌석 예약을 분리하고, 유효기간을 명확히 두어 무제한 이용권처럼 변질되지 않게 하는 운영 구조**다.

### 3. 서울 주짓수 시장에서도 `무료 첫 체험 + 유료 드롭인 + 월 이용횟수별 회원권` 구조가 확인된다

Way of Yawara가 2026년 7~9월 공개페이지를 조사한 서울 신촌권 사례에는 무료 체험과 유료 drop-in, 월 8회·12회·무제한 이용 구조가 함께 정리돼 있다. 원자료는 각 도장 공개정보를 바탕으로 한 2차 집계이므로 가격 자체보다 구조만 참고한다.

출처: https://wayofyawara.com/de/train/hongdae-sinchon

### 4. 강서구 로컬 유도장에서는 장기 약정 할인과 1회 체험이 이미 경쟁요소로 노출된다

방화동 무지개유도관의 당근 공개 프로필은 1회 체험과 1개월·3개월·6개월·1년 가격, 장기 등록 혜택을 함께 노출한다. 마지막 수정일은 2026년 9월 4일이다.

출처: https://www.daangn.com/kr/local-profile/%EB%AC%B4%EC%A7%80%EA%B0%9C%EC%9C%A0%EB%8F%84%EA%B4%80-8w9ug18m38v8/

파이널유도는 더 큰 할인으로 맞서지 않는다. 현재 로컬 시장에서 `체험 → 월/장기등록` 계단이 이미 보편적으로 노출되는 상황에서, **월회원권까지 바로 결제하지 않는 성인 수요를 잃고 있는지 데이터로 확인할 필요**가 있다는 신호로만 사용한다.

## CMO — 우선순위

1. Flex Pass를 전회원 상품으로 만들지 않고 `일정·약정 장벽이 확인된 성인`에 한정한 파일럿 후보로 둔다.
2. 월회원권·장기회원권의 가격가치를 훼손하지 않도록 회당 단가는 정규회원보다 유리하게 자동 설계하지 않는다.
3. 인기시간대의 기존회원 좌석을 잠식하지 않도록 `capacity gate`를 선행한다.
4. 단순 패스 판매액이 아니라 `실사용 → 정규등록 → 30일 활성 → 순기여`까지 본다.
5. 기존 월회원의 패스 다운그레이드가 발생하면 증분매출이 아니라 매출잠식으로 별도 집계한다.

## Growth Analyst — 퍼널과 병목

### 신규 성인 퍼널

`qualified_adult_inquiry → trial_attended → commitment_or_schedule_barrier_confirmed → flex_pass_offer_eligible → flex_pass_purchased → first_pass_use → pass_usage_completed → monthly_membership_30d → active_30d → renewal`

### 휴면 성인 복귀 퍼널

`returner_self_opt_in → return_slot_matched → flex_reentry_eligible → pass_purchased → verified_return → second_use → active_30d → renewal`

### 잠식 감시 퍼널

`active_monthly_member → flex_pass_exposed → downgrade_requested → downgrade_completed → revenue_delta`

필수 집계 필드 후보:

- `branch_id`
- `source_channel`
- `segment` — NEW_ADULT / RETURNER / ACTIVE_MEMBER / UNKNOWN
- `barrier_type` — SCHEDULE / COMMITMENT / PRICE / UNKNOWN
- `flex_offer_eligible`
- `flex_offer_shown`
- `flex_pass_type`
- `pass_price`
- `pass_uses_included`
- `pass_expiry_days`
- `pass_uses_consumed`
- `verified_attendance`
- `monthly_membership_within_30d`
- `active_30d`
- `downgrade_from_monthly`
- `coach_minutes_used`
- `gross_revenue`
- `variable_cost`
- `net_contribution`

회원 이름·연락처·개별 결제원장·출석원장·건강정보는 공개 GitHub에 저장하지 않는다.

## Acquisition Marketer — 신규 체험 확보 캠페인 후보

실제 상품을 만들기 전에는 `회차권 있음`을 광고하지 않는다. 대표 승인 후 파일럿이 시작될 경우에도 주 타깃은 아래처럼 좁힌다.

### 대상 후보

- 성인 첫 체험 후 유도 자체에는 긍정적이나 월 고정일정이 부담이라고 명시한 사람
- 출장·교대근무·불규칙 일정 때문에 정기등록 판단을 미루는 성인
- 장기 약정보다 몇 차례 실제 수업을 더 경험한 뒤 결정하고 싶다고 밝힌 성인

### 제외 후보

- 인기시간대 정원이 이미 빠듯한 반
- 유소년·보호자 대상 자동제안
- 현재 정상적으로 월회원권을 이용 중인 회원에게 일괄 노출
- 가격 할인만 요구하고 일정·약정 장벽이 확인되지 않은 경우

## CRM Marketer — 휴면·체험후미등록·기존회원 액션

### 체험후미등록

기존 `trial-followup-review-v1.md`와 중복 메시지를 보내지 않는다. 후속상담에서 실제 장벽이 `시간/약정`으로 확인된 경우에만 Flex Pass 후보를 검토한다. `가격이 비싸다`만 확인된 경우 할인상품처럼 자동 제안하지 않는다.

### 휴면회원

기존 `adult-returner-reentry-lane-v1.md`의 복귀 경로를 우선한다. 실제 복귀 가능한 시간이 있으나 장기 약정이 장벽인 성인만 별도 파일럿 후보로 분리한다.

### 기존회원

정상 이용 중인 기존회원에게 Flex Pass를 적극 노출하지 않는다. 기존회원이 일정 문제로 회원권 변경을 문의한 경우에도 먼저 `class switch / pause policy / existing membership option`을 확인하고, Flex Pass 전환은 승인된 정책이 있을 때만 검토한다.

## Content Strategist — 채널별 실행 후보

### 네이버

승인 후에만 `성인 유도 시작 방법` 페이지에 월회원권 외에 승인된 저약정 진입상품이 있음을 사실대로 표시한다. 가격·회차·유효기간·이용 가능 수업을 한 화면에서 명확히 보여준다.

### 인스타그램

`주 2~3회 꼭 나와야 하나요?` 같은 실제 일정 불안을 설명하는 정보형 콘텐츠를 우선한다. 상품이 승인되기 전에는 특정 패스 판매 CTA를 넣지 않는다.

### 당근

로컬 경쟁처럼 장기등록 할인만 강조하기보다 `성인 초보 / 실제 가능시간 / 이용방식`을 먼저 보여준다. Flex Pass가 실제 출시된 뒤에도 `무조건 저렴`, `월회원보다 이득` 같은 문구는 사용하지 않는다.

## Experiment Manager

### EXP-271 Adult Flex Pass Bridge

**가설:** 첫 체험 후 유도에 대한 관심은 있으나 일정·약정 장벽으로 월회원권 등록을 미루는 성인에게 제한된 저약정 유료경로를 제공하면, 단순 미등록 상태보다 `유료 첫 진입 → 반복출석 → 30일 활성`이 증가할 수 있다.

**대상:** 사전에 정의된 barrier 조건을 충족한 성인만.

**주 KPI:**

- eligible → flex_pass_purchased
- purchased → first_pass_use
- pass uses consumed / included
- flex_pass → monthly_membership_within_30d
- active_30d
- net_contribution_per_eligible_lead
- coach_minutes_per_pass_user

**중단 기준:**

- 기존 정규반 과밀 또는 코치부담 증가
- 패스 구매는 늘지만 반복출석·월회원 전환·30일 활성 개선 없음
- 환불·유효기간·예약조건 민원 증가
- 월회원권보다 사실상 더 유리한 가격구조가 되어 정규상품 가치를 훼손

기준선·최소개선폭·표본수·가격·회차·유효기간은 **데이터 미연결**이다.

### EXP-272 Adult Returner Flex Re-entry

**가설:** 복귀 의사는 있으나 즉시 월회원권 재등록을 망설이는 성인 휴면회원 중 일정·약정 장벽이 명확한 경우, 제한된 Flex Re-entry가 실제 복귀와 30일 활성의 중간다리가 될 수 있다.

**주 KPI:**

- returner_self_opt_in → pass_purchase
- pass_purchase → verified_return
- first_use → second_use
- active_30d
- renewal_or_monthly_conversion
- net_contribution

**중단 기준:** 기존 복귀경로보다 실제 복귀가 개선되지 않거나, 할인 기대만 만들어 정상 재등록을 늦추면 중단한다.

### EXP-273 Cannibalization Guardrail Audit

**목적:** 신규 패스 매출이 기존 월회원권 매출을 옮겨 담은 것인지 증분매출인지 구분한다.

**필수 지표:**

- 신규/복귀자 패스 매출
- active monthly → flex downgrade 건수
- downgrade 전후 월평균 결제액
- capacity usage by pass users
- monthly membership conversion after pass
- net contribution after variable cost

**중단 기준:** 신규·복귀 매출보다 기존 월회원 다운그레이드로 인한 손실이 커지거나, 인기시간대 기존회원의 예약·출석 기회를 침해하면 확대하지 않는다.

## 필요한 대표 승인·입력

실험을 실제 실행하려면 아래 내부값이 필요하다.

- 현재 성인 월회원권·횟수제·장기권 존재 여부와 실제 가격
- 현재 체험 정책과 체험 후 미등록 사유 집계
- 성인반별 `member_capacity`와 `trial_capacity`
- 패스 이용을 허용할 수 있는 반·요일·시간
- 후보 회차와 유효기간
- 환불·양도·연장·노쇼 정책
- 현재 월회원권 다운그레이드·해지 기준선
- 최근 비식별 `체험 → 등록 → 30일 활성` 집계
- 휴면 성인의 복귀 기준선
- 코치가 추가 처리할 수 있는 시간 상한
- 최소 허용 순기여와 매출잠식 허용 한도

위 값이 연결되기 전 실제 가격·회차를 정하지 않는다.

## KPI 상태

- 성인 신규문의: **데이터 미연결**
- 성인 체험 실방문: **데이터 미연결**
- 체험후미등록 사유 중 일정·약정 비중: **데이터 미연결**
- Flex Pass 대상 가능 인원: **데이터 미연결**
- Flex Pass 구매율: **데이터 미연결**
- Pass 첫 사용률: **데이터 미연결**
- Pass 소진율: **데이터 미연결**
- Pass → 월회원 30일 전환: **데이터 미연결**
- 30일 활성률: **데이터 미연결**
- 휴면 성인 복귀율: **데이터 미연결**
- 기존회원 재등록률: **데이터 미연결**
- 월회원 → Flex 다운그레이드율: **데이터 미연결**
- 체험/패스별 코치 추가시간: **데이터 미연결**
- 귀속매출·순기여: **데이터 미연결**

## Auditor — 근거·중복·과장·브랜드 리스크

1. 외부 시설의 회차·가격을 파이널유도 가격근거로 복제하지 않는다.
2. `회차권이 있으면 신규회원이 늘어난다`, `불규칙 근무자는 회차권을 선호한다`를 검증 전 사실처럼 표현하지 않는다.
3. 정규 월회원권보다 회당 가격이 과도하게 낮아지는 구조를 기본안으로 두지 않는다.
4. 유효기간·예약가능시간·환불조건을 결제 후에 알리는 방식은 사용하지 않는다.
5. 패스 이용자가 기존회원의 안전정원·수업품질을 침해하면 판매를 중단한다.
6. 기존 월회원에게 다운그레이드 상품을 일괄 홍보해 자기잠식을 유발하지 않는다.
7. 체험후미등록자·휴면회원에게 기존 CRM과 중복 접촉하지 않는다.
8. 실제 승인된 상품이 생기기 전 네이버·인스타·당근에서 `회차권 운영`을 주장하지 않는다.

## 이번 실행에서 하지 않은 외부 행동

- Flex Pass 상품 생성 또는 판매: 하지 않음
- 가격·회차·유효기간 확정: 하지 않음
- 결제/PG 설정 변경: 하지 않음
- 고객 메시지 발송: 하지 않음
- 네이버·인스타·당근 게시: 하지 않음
- 광고 집행·비용 지출: 하지 않음

## 공개 출처

- Jaiyen MMA — current pricing and flexible passes/class packs: https://jaiyenmma.com/
- REVL Training Gangnam — 1/3/5 class passes and app booking: https://creatrip.com/en/spot/15816
- Way of Yawara — Seoul/Hongdae-Sinchon BJJ first visit, drop-in and monthly structures, checked 2026: https://wayofyawara.com/de/train/hongdae-sinchon
- Muji Judo Club, Gangseo-gu Daangn profile — trial and term pricing, modified 2026-09-04: https://www.daangn.com/kr/local-profile/%EB%AC%B4%EC%A7%80%EA%B0%9C%EC%9C%A0%EB%8F%84%EA%B4%80-8w9ug18m38v8/
