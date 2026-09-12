# Inbound Inquiry Response & Routing SLA v1

- 기준일: 2026-09-12
- 목적: 네이버 톡톡·인스타 DM·당근 채팅·전화/폼 등으로 들어오는 신규 문의가 예약 전 단계에서 누락되거나 중복 응대되는 문제를 줄이고, 문의→예약→실방문→등록→30일 활성까지 연결되는 계측 구조를 만든다.
- 상태: 설계 완료. 고객 메시지 발송, 자동응답 변경, 계정 설정 변경, 광고 집행 전.
- 공개 범위: 회원·보호자 식별정보, 전화번호, 개인별 상담내용, 출석·건강·결제 원장, 비밀키·토큰을 포함하지 않는다.

## 1. 이번 변화

2026년 9월 현재 무도·피트니스 신규획득 페이지에서 `무료 체험 CTA + 즉시 메시지/문자/WhatsApp + 실제 수업시간 공개`가 함께 사용되는 사례가 반복 확인된다.

- Masters Academy Plymouth는 2026-08-29 Back-to-School 캠페인에서 무료 체험 CTA와 WhatsApp 연락경로를 함께 제공한다.
- London Kettlebell Club은 2026년 9월 실제 체험 가능일을 날짜별로 공개하면서 온라인 예약 외에 문자·WhatsApp 직접 문의 경로를 병행한다.
- Jack Hwang's Martial Arts는 2026년 현재 무료 입문 세션 폼에서 관심 프로그램과 연락처를 받고 SMS 수신 동의를 명시한다.
- 네이버 스마트플레이스는 2026-05-28 플레이스 스쿨 교육에서 스마트플레이스 기본설정, 예약, 쿠폰, 톡톡 마케팅 메시지, 플레이스광고를 `고객 유입 → 예약 전환 → 재방문 유도` 흐름으로 함께 다룬다.
- 네이버 톡톡 마케팅메시지는 최근 방문 고객 또는 혜택알림받기 고객에게 네이버 톡톡과 앱 푸시로 소식을 전달할 수 있다고 공식 안내하고 있다.

이 사례들은 `빠른 답변이 등록률을 높인다`는 파이널유도 내부 성과 증거가 아니다. 다만 현재 로컬 서비스·무도시설에서 문의 채널과 체험 예약을 짧게 연결하는 운영이 일반적으로 사용되고 있음을 보여준다.

파이널유도멀티짐의 채널별 문의량, 최초응답시간, 문의→예약률, 중복문의율, 미응답 건수, 실방문·등록·30일 활성은 모두 **데이터 미연결**이다.

## 2. CMO 판단

새 광고비를 투입하기 전에 `들어온 문의를 얼마나 빨리, 정확하게, 중복 없이 예약 가능한 반으로 연결하는가`를 먼저 측정한다.

핵심 원칙:

1. 모든 문의를 하나의 리드로 합치지 않는다. 같은 사람이 네이버·인스타·당근에서 중복 문의할 수 있다.
2. `첫 응답`과 `해결`을 구분한다. 자동 인사만 전송된 상태를 상담 완료로 보지 않는다.
3. 가격·시간만 물은 사람에게 장문의 상담 폼을 강요하지 않는다.
4. 실제 반별 체험 가능시간과 `trial_capacity`가 확인되기 전 예약 가능하다고 약속하지 않는다.
5. 휴면·기존회원 문의를 신규리드로 자동 분류하지 않는다.

## 3. Growth Analyst — 퍼널과 병목

권장 퍼널:

`inquiry_received → first_human_response → qualified_minimally → slot_offered → booking_confirmed → verified_attendance → paid_within_14d → active_30d → renewal`

최소 집계 필드:

- `channel`: NAVER_TALKTALK / INSTAGRAM_DM / DAANGN_CHAT / PHONE / WEBFORM / OTHER
- `inquiry_received_at`
- `first_human_response_at`
- `first_response_minutes`
- `lead_type`: NEW_TRIAL / RETURNER / EXISTING_MEMBER / UNKNOWN
- `program_interest`: YOUTH / TEEN / ADULT / COMPETITION / UNKNOWN
- `preferred_slot_1`
- `preferred_slot_2`
- `slot_offered_at`
- `booking_status`
- `verified_attendance_status`
- `paid_within_14d_status`
- `active_30d_status`
- `duplicate_suspected`: YES / NO / UNKNOWN

공개 저장소에는 개인 이름, 연락처, 메시지 원문을 저장하지 않는다.

## 4. Acquisition Marketer — 신규 체험 문의 구조

신규 문의에서 처음부터 많은 질문을 하지 않는다.

1차 최소 확인 후보:

- 대상: 유소년 / 중고등 / 성인
- 유도 경험: 처음 / 경험 있음
- 희망 시간 1순위
- 희망 시간 2순위

그 다음 실제 `trial_capacity`를 확인한 뒤 가능한 시간만 제시한다.

`체험 신청` CTA는 채널마다 문구가 달라도 최종 데이터 구조는 동일해야 한다.

## 5. CRM Marketer — 기존·휴면 문의 분리

문의자가 과거 회원 또는 체험자라고 확인되면 신규체험 퍼널에서 제외하고 기존 CRM 상태로 라우팅한다.

- `RETURNER`: 복귀 가능 반 확인 후 복귀 퍼널
- `EXISTING_MEMBER`: 수업·결제·일정 문의 등 목적별 운영 대응
- `TRIAL_NO_CONVERSION`: 기존 체험후미등록 후속 정책 적용

네이버 `혜택알림받기` 고객을 기존회원 또는 휴면회원으로 추정하지 않는다.

## 6. Content Strategist — 채널별 CTA 원칙

### 네이버

- 플레이스 핵심정보와 실제 수업시간을 최신 상태로 유지한다.
- 톡톡 문의 CTA가 있다면 `대상 + 희망시간 1·2순위` 정도로 진입 마찰을 낮춘다.
- 자동 인사는 업무시간·답변 방식 안내 수준으로 제한하고, 사람 답변 전 상담완료 처리하지 않는다.

### 인스타그램

- 프로필/스토리 CTA를 `DM 주세요`에서 끝내지 말고 `성인/유소년 + 희망시간`을 함께 보내도록 설계한다.
- 게시물 조회수를 예약 성과로 간주하지 않는다.

### 당근

- 비즈프로필/게시물에서 지점·대상·체험 가능시간을 실제 운영정보와 맞춘다.
- 채팅이 들어오면 가능한 지점과 시간을 빠르게 구분하되, 다른 지점으로 임의 전환하지 않는다.

## 7. Experiment Manager

### EXP-251 — First Human Response SLA Pilot

가설: 업무시간 내 신규 문의에서 사람의 첫 실질응답까지 걸리는 시간을 줄이면 문의→예약 과정의 누락을 줄일 수 있다.

핵심 KPI:

- 채널별 `first_response_minutes` 중앙값
- 미응답 문의 비율
- 문의→`slot_offered`
- 문의→예약
- 예약→실방문
- 실방문→14일 등록

성공 기준:

- 대표가 승인한 응답시간 상한을 지키면서 문의→예약 또는 실방문이 기준선 대비 개선되고, 코치 업무부담이 승인된 범위 안에 있을 때 확대한다.

중단 기준:

- 속도 때문에 잘못된 시간·가격·혜택 안내 증가
- 수업 중 코치의 실시간 상담 부담 증가
- 응답속도는 개선됐지만 예약·실방문에 변화 없음

응답시간 상한과 최소 개선폭은 **데이터 미연결**이며 대표 승인 전 숫자를 확정하지 않는다.

### EXP-252 — Minimal Qualification Card

가설: 긴 문의 폼보다 `대상 + 경험 + 희망시간 1·2순위`만 먼저 받는 구조가 예약 가능한 반으로의 라우팅을 쉽게 만든다.

측정:

- 문의 시작→최소정보 확보율
- 최소정보 확보→slot_offered
- slot_offered→예약
- 예약→실방문

중단 기준:

- 안전에 필요한 정보가 누락되어 수업운영 위험 증가
- 질문 수는 줄었지만 잘못된 반 배정 증가

### EXP-253 — Cross-Channel Duplicate Guard

가설: 채널별 문의 건수를 그대로 합산하지 않고 중복 가능성을 `UNKNOWN/YES/NO`로 별도 관리하면 신규획득 성과 과대계상을 줄일 수 있다.

측정:

- 채널별 총 문의수
- 중복 의심 건수
- 비식별 unique lead count
- unique lead→예약→실방문

중단 기준:

- 중복 확인을 위해 과도한 개인정보를 수집해야 하는 경우
- 직원이 개인 대화를 임의로 외부 도구에 복사해야 하는 경우

## 8. 필요한 대표 승인·입력

- 현재 사용 중인 문의 채널 목록
- 각 채널의 담당자와 실제 응대 가능 시간
- 본관·지점별 체험 가능한 반과 `trial_capacity`
- 현재 네이버 톡톡 사용 여부 및 자동응답 설정 여부
- 인스타 DM·당근 채팅 담당 방식
- 업무시간 내 목표 첫응답시간 상한
- 수업 중 코치가 상담에 사용할 수 있는 시간 상한
- 상담 후 예약 링크/자체 앱/수동예약 중 실제 최종 경로

승인 전에는 자동응답 변경, 고객 메시지 발송, 계정 권한 변경, 광고 집행을 수행하지 않는다.

## 9. KPI 상태

- 채널별 신규문의: **데이터 미연결**
- 최초 사람응답시간: **데이터 미연결**
- 미응답률: **데이터 미연결**
- 중복문의율: **데이터 미연결**
- 문의→시간 제안률: **데이터 미연결**
- 문의→예약률: **데이터 미연결**
- 예약→실방문률: **데이터 미연결**
- 실방문→14일 등록률: **데이터 미연결**
- 등록→30일 활성률: **데이터 미연결**
- 휴면 복귀율: **데이터 미연결**
- 기존회원 재등록률: **데이터 미연결**
- 귀속매출·순기여: **데이터 미연결**

## 10. Auditor 규칙

1. `빠른 응답 = 높은 등록률`로 단정하지 않는다.
2. 자동인사 발송을 사람의 실질 응답으로 계상하지 않는다.
3. 고객 동의 없이 SMS·카카오·DM 등 다른 채널로 임의 이동하지 않는다.
4. 신규 문의를 위해 건강·학교·회사·가족정보를 불필요하게 과수집하지 않는다.
5. 반 여석이 확인되지 않은 상태에서 `예약 가능`, `마감 임박`을 사용하지 않는다.
6. 고객 메시지 원문이나 연락처는 공개 GitHub에 저장하지 않는다.
7. 여러 채널의 동일 문의를 각각 신규리드로 계상하지 않는다.

## 근거 출처

- NAVER SmartPlace, `플레이스 스쿨 교육 신청`, 2026-05-28: https://new.smartplace.naver.com/notices/1023
- NAVER SmartPlace Solution Market, `마케팅메세지`, 2026-09-12 확인: https://new.smartplace.naver.com/introduction/solution-market/marketingMessage
- Masters Academy Plymouth, Back-to-School / Free Trial + WhatsApp, 2026-08-29: https://www.schoolandcollegelistings.com/GB/Plymouth/202276886483236/Masters-Academy-Plymouth
- London Kettlebell Club, September 2026 trial dates + Text/WhatsApp: https://londonkettlebellclub.com/book-a-trial-session/
- Jack Hwang's Martial Arts, Free Intro Session + SMS consent, 2026-09-12 확인: https://jackhwangmartialarts.com/

## 11. 중복 방지 메모

기존 `booking-attendance-evidence-v1.md`는 `예약 접수 이후 동기화·실출석 증거`를 다룬다. 이번 문서는 그 앞 단계인 `문의 접수 → 첫 사람응답 → 최소 자격확인 → 시간 제안 → 예약` 구간에만 범위를 둔다.

기존 `trial-pricing-free-vs-paid-credit-pilot-v1.md`는 체험 가격·경제성을 다룬다. 이번 문서는 가격을 변경하지 않는다.
