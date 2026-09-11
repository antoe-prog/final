# Progress Visibility & Grading Readiness Loop v1

- 상태: 설계 완료, 회원 노출·고객 연락·승급/승단 판정·가격 변경 전
- 작성 기준일: 2026-09-12
- 목적: 기존회원 만족·재등록 보호를 중심으로 신규 체험 전환, 휴면 복귀, 매출 기여를 함께 계측
- 공개 저장 원칙: 개인 식별정보, 보호자 연락처, 개인별 출석·건강·결제 원장, 실제 심사 대상자 명단은 저장하지 않는다.

## 1. 오늘의 가설

파이널유도멀티짐의 회원경험에서 `현재 무엇을 배우고 있는지`, `다음에 무엇을 배우는지`, `승급·승단과 어떤 관계가 있는지`가 회원에게 충분히 보이지 않는다면 출석만 관리하는 것보다 진도 가시성을 제공하는 편이 만족·활성·재등록에 도움이 될 수 있다.

이 문서는 승급을 빨리 시키거나 심사비를 매출수단으로 밀어 넣기 위한 안이 아니다. 핵심은 **진도 가시화 → 코치 확인 → 공식 심사요건 별도 확인 → 실제 활성·재등록 검증**이다.

## 2. 왜 지금 검토하는가

### 2.1 서울 유도 환경

서울특별시유도회는 2026-09-04 `2026년도 4차 정기승단 심사 안내`를 공지했고 신청원서·유의사항·구비서류 안내를 함께 게시했다. 따라서 서울 지역 도장은 공식 승단 일정과 서류 준비를 회원 경험과 연결할 필요가 있다.

중요: 이 문서에서는 첨부파일의 세부 심사일·자격요건을 임의로 해석하지 않는다. 실제 적용 시 최신 공식 공지와 도장 코치 검토를 거친다.

출처: https://www.seouljudo.com/bbs_shop/read.htm?board_code=bbs4_11&cate_sub_idx=0&idx=911644

### 2.2 국제 유도 운영 변화

Judo Australia는 2026-05-25 국가 디지털 Grading System 도입을 발표했고, 2026년 9월 말 이전 클럽의 온라인 기록 전환을 목표로 하고 있다. 백띠부터 Dan까지 진급 이력을 투명하게 기록하고 클럽 행정부담과 회원경험을 개선하는 것이 목적이다.

출처: https://www.ausjudo.com.au/creating-a-transparent-trackable-digitised-national-grading-system/

### 2.3 도장 소프트웨어 시장 변화

2026년 현재 유도·무도 운영 소프트웨어는 단순 출석·결제뿐 아니라 회원이 직접 다음 단계와 진급 이력을 확인하는 방향으로 확장되고 있다.

- Kimono: 회원 앱에서 등급 이력, 다음 등급 목표, 관련 기술 프로그램·요건을 표시한다.
- Toki: QR 출석, 일정, 커뮤니케이션과 함께 Belt Progression을 회원 앱 핵심 기능으로 둔다.
- IpponBoard: 출석·결제와 함께 Belt Progression 및 Grading Records를 관리한다.

출처:
- https://join-kimono.com/docs/en/members/track-grades
- https://www.jointoki.be/
- https://ipponboard.com/

외부 제품의 기능 존재는 파이널유도에서 유지율이 개선된다는 증거가 아니다. 구현 방향과 시장 기대수준을 확인하는 참고자료로만 사용한다.

## 3. CMO 우선순위

1. 기존회원: `현재 위치 → 다음 학습목표 → 코치 확인`을 보이게 해 만족·재등록에 미치는 영향을 검증한다.
2. 휴면회원: 과거 진도 기록이 있다면 `어디서 다시 시작할지`를 내부 코치가 확인해 복귀 마찰을 줄일 수 있는지 본다.
3. 신규회원: 상담·체험에서 `무엇을 배우게 되는지`를 투명하게 설명하되 빠른 승급을 판매 포인트로 삼지 않는다.
4. 매출: 재등록·활성·정상적인 공식 심사 참여와의 관계를 관찰하되 승급 빈도·심사비를 성장 KPI로 두지 않는다.

## 4. Growth Analyst — 필요한 퍼널

### 4.1 기존회원

`active_member`
→ `progress_viewed`
→ `next_goal_understood`
→ `coach_reviewed`
→ `active_next_30d`
→ `renewed`

### 4.2 휴면 복귀

`dormant_or_returner`
→ `prior_progress_available`
→ `coach_reentry_review`
→ `return_visit`
→ `active_30d`
→ `renewed`

### 4.3 신규 체험

`trial_attended`
→ `learning_path_explained`
→ `visit_2`
→ `paid_within_14d`
→ `active_30d`

### 4.4 공식 심사

`official_notice_detected`
→ `internal_candidate_review`
→ `official_requirements_verified`
→ `member_informed_if_appropriate`
→ `application_status`
→ `result_recorded`

`internal_candidate_review`는 공식 응시자격 판정이 아니다. 도장 내부 검토 상태일 뿐이다.

## 5. 최소 데이터 구조

개별 데이터는 내부 운영시스템에서만 처리하고 공개 저장소에는 넣지 않는다.

```text
member_id_internal
branch_id
current_internal_grade
last_grade_date
progress_stage
next_skill_focus
coach_review_status
coach_reviewed_at
official_grade_verified
official_notice_source
application_status
progress_viewed_at
active_30d
renewal_status
```

권장 상태값:

- `LEARNING`
- `REVIEW_NEEDED`
- `COACH_CONFIRMED_INTERNAL`
- `OFFICIAL_REQUIREMENTS_CHECK_NEEDED`
- `OFFICIAL_REQUIREMENTS_VERIFIED`
- `APPLICATION_READY`
- `NOT_YET_READY`

`COACH_CONFIRMED_INTERNAL`을 `승단 가능`, `합격 예정`, `심사 자격 확정`으로 외부 표시하지 않는다.

## 6. Acquisition Marketer — 신규체험 적용

신규획득 콘텐츠는 `몇 개월 만에 몇 띠`를 약속하지 않는다. 대신 학습경로의 투명성을 보여준다.

권장 메시지 구조:

1. 첫 방문에서 배우는 것
2. 첫 4~6주에 익히는 기본 움직임
3. 이후 코치가 확인하는 기술·안전·수업적응 요소
4. 승급·승단은 도장 내부진도와 공식 규정을 구분해 안내

채널별 검증안:

- 네이버: `유도를 시작하면 무엇부터 배우나요?` 형태의 학습경로형 콘텐츠
- Instagram: `첫 낙법 → 첫 연결동작 → 코치 피드백 → 다음 목표` 과정형 숏폼
- 당근: 실제 체험 가능 슬롯이 확인될 때 `처음 시작하는 사람의 수업 진행 방식` 중심 안내

`최단기간 승급`, `빠른 단증`, `몇 개월이면 검은띠` 같은 표현은 금지한다.

## 7. CRM Marketer — 기존회원·휴면회원

### 기존회원

회원에게 보여줄 수 있는 정보 후보:

- 현재 내부 진도 단계
- 최근 코치 피드백 요약
- 다음 학습 포인트 1~3개
- 공식 심사와 무관한 `다음 수업 목표`
- 공식 승단 관련 내용은 최신 규정 확인 후 별도 표시

### 휴면회원

복귀 시 과거 띠·단을 곧바로 현재 실력으로 간주하지 않는다.

`과거 기록 확인 → 현재 움직임·안전·수업 적응 확인 → 코치 재진입 판단 → 다음 목표 설정` 순으로 운영한다.

### 보호자

유소년 진도는 다른 회원과 비교하지 않는다. `반에서 몇 등`, `누구보다 늦다` 같은 표현 대신 본인 기준의 학습항목과 코치 피드백만 제공한다.

## 8. Content Strategist — 공개 콘텐츠 원칙

진도 콘텐츠의 목적은 우월감보다 `학습이 실제로 쌓이고 있다`는 신뢰 형성이다.

가능한 공개 소재:

- 기술명을 개인 식별 없이 설명하는 교육 콘텐츠
- 승급·심사 준비 과정의 일반 체크리스트
- 공식 공지 출처 안내
- 동의 받은 성인회원의 성장 후기
- 보호자 동의와 내부 초상권 기준을 충족한 경우에만 유소년 사진·영상

금지 또는 별도 승인 필요:

- 미성년자 이름+학교+등급+얼굴 조합 공개
- 개인별 승단 예정일 공개
- 공식기관이 확정하지 않은 합격·자격 보장
- 경쟁회원과의 진도 비교
- 심사비 결제를 승급 보장처럼 표현

## 9. Experiment Manager

### EXP-219 Progress Visibility vs Attendance-Only

가설: 출석·결제만 보이는 회원경험보다 `현재 진도 + 다음 목표 + 코치 확인상태`가 보이는 회원경험이 이후 활성·재등록에 더 유리할 수 있다.

측정:

- progress_view_rate
- next_goal_acknowledged_rate
- active_next_30d
- renewal_rate
- support_questions_per_member
- coach_admin_minutes_per_member

성공 기준:

- 최소개선폭: 데이터 미연결
- 표본수: 데이터 미연결
- 활성·재등록 중 최소 하나가 개선되고 코치 업무량·불만이 허용범위 안일 때만 확대

중단 기준:

- 회원이 내부진도를 공식 승급자격으로 오인
- 코치 업무량 과도 증가
- 공개 비교·서열화 민원
- 재등록 개선 없이 화면 조회만 증가

### EXP-220 Official Grading Readiness Check

가설: 공식 승단 공지가 나온 뒤 `공식 공지 확인 → 내부 후보 검토 → 요건 검증 → 필요한 사람에게만 안내` 순서를 표준화하면 누락·중복문의·서류 준비 마찰을 줄일 수 있다.

측정:

- official_notice_to_internal_review_time
- verified_candidate_count
- application_error_count
- missed_deadline_count
- member_question_count
- admin_minutes_per_application

성공 기준:

- 기준선: 데이터 미연결
- 누락·서류오류·관리시간 중 하나 이상이 실질적으로 감소하고 오안내가 없어야 함

중단 기준:

- 공식 규정 자동판정 오류
- 최신 공지와 내부규칙 불일치
- 개인정보·서류가 공개 저장소 또는 비승인 채널에 노출

## 10. 필요한 대표 승인·입력

실제 적용 전 다음이 필요하다.

- 파이널유도 내부 띠/급/단 진도 기준
- 현재 승급심사 운영 방식과 주기
- 공식 승단심사와 도장 내부 승급의 구분
- 코치가 회원별 진도를 기록하는 현재 방식
- 회원/보호자에게 공개 가능한 진도 범위
- 승급·승단 관련 비용 항목과 회계처리 방식
- 현재 재등록률·30일 활성률·심사 참여율의 비식별 기준선
- 지점별 기준이 다른 경우 본사 공통값과 지점 재량값의 경계

확인되지 않은 값은 모두 `데이터 미연결`로 유지한다.

## 11. KPI 상태

현재 다음 값은 연결되지 않았다.

- 회원별 진도 조회율: 데이터 미연결
- 코치 진도검토 완료율: 데이터 미연결
- 다음 목표 인지율: 데이터 미연결
- 기존회원 30일 활성률: 데이터 미연결
- 재등록률: 데이터 미연결
- 휴면 복귀 후 30일 활성률: 데이터 미연결
- 공식 승단 후보 검토수: 데이터 미연결
- 심사 신청 오류·누락: 데이터 미연결
- 승급/승단 관련 문의량: 데이터 미연결
- 코치 행정시간: 데이터 미연결
- 귀속매출·순기여: 데이터 미연결

## 12. Auditor 체크

- 디지털 진도표시는 공식 승급·승단 자격을 대신하지 않는다.
- 공식 심사자격은 최신 서울특별시유도회·대한유도회 규정 및 공지를 기준으로 별도 확인한다.
- 외국 연맹의 Grading System은 한국의 승단규칙으로 전용하지 않는다.
- 소프트웨어 공급사의 기능·성과 주장을 파이널유도의 예상 유지율·매출효과로 사용하지 않는다.
- 회원의 진도를 비교·압박·공포 마케팅에 사용하지 않는다.
- 승급·승단이 매출을 만들기 때문에 심사대상을 늘리는 구조를 금지한다.
- 미성년자 진도·얼굴·학교 등 식별조합은 공개 콘텐츠로 자동 전환하지 않는다.

## 13. 실행 상태

이번 문서는 공개 가능한 전략·계측안만 기록한다.

- 회원에게 진도 화면 노출: 미실행
- 승급/승단 후보 자동판정: 미실행
- 고객 메시지 발송: 미실행
- 네이버·인스타·당근 게시: 미실행
- 가격·심사비 변경: 미실행
- 공식 승단 신청: 미실행
- 운영DB 마이그레이션: 미실행

대표 승인과 내부 데이터 연결 전에는 위 항목을 실행 완료로 주장하지 않는다.
