# ✍️ ThreeJ 팀 커밋 컨벤션

> 커밋 · PR 제목을 쓸 때 이 문서 하나만 보면 됩니다.
> 브랜치 · 승인 · 충돌 규칙은 **[GitHub 전략 문서](./Github_Strategy.md)** 에 있습니다.

**목차**
[1. 한눈에 보기](#1-한눈에-보기) ·
[2. 제목](#2-제목) ·
[3. 기능 (괄호 안)](#3-기능-괄호-안) ·
[4. 종류](#4-종류) ·
[5. 본문](#5-본문) ·
[6. 이슈와 잇는 법](#6-이슈와-잇는-법) ·
[7. 커밋 크기](#7-커밋-크기) ·
[8. 고치기](#8-고치기) ·
[9. 머지 · 충돌 커밋](#9-머지--충돌-커밋) ·
[10. PR 제목](#10-pr-제목) ·
[11. 예시 — 이슈부터 PR 까지](#11-예시--이슈부터-pr-까지) ·
[12. 좋은 예 · 나쁜 예](#12-좋은-예--나쁜-예)

---

## 1. 한눈에 보기

```
종류(기능): 설명                          ← 제목 (한국어, 50자 안쪽, 마침표 X)
                                          ← 빈 줄
왜 바꿨나                                  ← 본문 (필요할 때만)
make check : PASS 0/27 → 2/27
```

| 규칙 | 한 줄 요약 |
|---|---|
| 제목 | `종류(기능): 설명` |
| 기능 | 기능 브랜치 이름에서 `feature-` 를 뗀 것 (= 기능 이슈의 「기능 이름」) |
| 종류 | `feat` · `fix` · `refactor` · `docs` · `chore` |
| 본문 | 왜 + `make check` 숫자. 필요할 때만 |
| 이슈 번호 | 커밋에는 **안 적는다**. PR 에서 잇는다 |
| 크기 | 한 커밋 = 한 가지 일 |
| 고치기 | push 전에만. push 후엔 고치지 않는다 (force push 금지) |
| PR 제목 | 커밋 제목과 같은 형식 |

---

## 2. 제목

- 형식 : `종류(기능): 설명`
- 설명은 **한국어**, **50자 안쪽**
- 끝에 마침표를 찍지 않는다
- "무엇을 했나" 가 보이게 쓴다 (함수 이름을 넣으면 좋다)

```
feat(alarm): timer_sleep 이 바쁜 대기 대신 thread_sleep 호출
fix(donation): lock 해제 때 그 lock 의 기부만 지우기
```

---

## 3. 기능 (괄호 안)

- 괄호 안 = **기능 브랜치 이름에서 `feature-` 를 뗀 것**
  - 기능 이슈 템플릿의 「기능 이름」 칸과 같다
  - 그래서 이슈 · 브랜치 · 커밋이 한 이름으로 이어진다

| 기능 이슈 「기능 이름」 | 기능 브랜치 | 커밋 |
|---|---|---|
| `alarm` | `feature-alarm` | `feat(alarm): …` |
| `priority` | `feature-priority` | `feat(priority): …` |
| `sync` | `feature-sync` | `feat(sync): …` |
| `donation` | `feature-donation` | `feat(donation): …` |
| `fixed-point` | `feature-fixed-point` | `feat(fixed-point): …` |
| `mlfqs` | `feature-mlfqs` | `feat(mlfqs): …` |

- 셋이 같이 고치는 공통 부분은 `thread`
  - 예 : `struct thread` 필드, `thread_init` 초기화
- 기능이 없는 일은 **괄호를 뺀다**
  - 예 : `docs: 커밋 컨벤션 문서 추가`, `chore: .gitignore 에 build 추가`

> 이 6개는 팀 깃 프로젝트의 부모 기능 이슈 · 「기능」 칸과 같다 ([깃 프로젝트 사용법 3절](./Project_Guide.md#3-부모-기능-이슈-6개)).
> 기능 이름이 새로 생기면 기능 이슈를 만들 때 정한 「기능 이름」 을 그대로 쓴다.

---

## 4. 종류

| 종류 | 언제 | 예 |
|---|---|---|
| `feat` | 기능을 더한다 | `feat(priority): ready list 를 우선순위 순으로 넣기` |
| `fix` | 버그를 고친다 | `fix(alarm): 깨울 시각이 같은 스레드를 모두 깨우기` |
| `refactor` | 동작은 그대로, 구조만 바꾼다 | `refactor(priority): 비교 함수를 thread.c 로 옮기기` |
| `docs` | 문서 · 주석 | `docs(donation): lock_acquire 기부 순서 주석` |
| `chore` | 설정 · 빌드 · 필드 추가처럼 동작이 안 바뀌는 일 | `chore(thread): struct thread 에 wakeup_tick 추가` |

- `test` 는 쓰지 않는다
  - 핀토스 테스트는 이미 주어져 있어서 우리가 테스트 코드를 쓰지 않는다
  - 테스트 결과는 커밋 본문 · PR 「테스트 결과」 칸에 적는다

---

## 5. 본문

- **필요할 때만** 쓴다 (작은 커밋은 제목 한 줄로 충분)
- 제목과 본문 사이에 **빈 줄** 하나
- 적을 것
  1. **왜** 바꿨나 (코드만 봐서는 모르는 이유)
  2. **`make check` 숫자** (바뀌었을 때)

```
feat(alarm): timer_sleep 이 바쁜 대기 대신 thread_sleep 호출

바쁜 대기는 CPU 를 계속 써서 idle tick 이 0 으로 나온다.
make check : PASS 0/27 → 2/27 (alarm-single, alarm-multiple)
```

- 숫자는 PR 템플릿 「테스트 결과」 칸에 그대로 옮겨 쓰면 된다

---

## 6. 이슈와 잇는 법

> **커밋에는 이슈 번호를 적지 않는다.** 이슈와는 **PR 에서** 잇는다.

| 방법 | 어떻게 | 보이는 곳 |
|---|---|---|
| ① PR 본문에 `#번호` | PR 템플릿 「관련 이슈」 칸에 번호만 (`Closes` 없이) | 이슈 타임라인에 "이 PR 에서 언급됨" |
| ② PR 오른쪽 **Development** 에서 이슈 연결 | PR 화면 오른쪽 칸에서 이슈 고르기 | 이슈의 Development 칸 · 팀 깃 프로젝트의 「Linked pull requests」 칸 |

- **둘 다 한다.**
- `Closes #번호` 로는 닫히지 않는다
  - GitHub 는 닫기 키워드를 **기본 브랜치(`main`)를 향한 PR 에서만** 읽는다
  - 개인 → 기능, 기능 → dev PR 에서는 무시된다
- 이슈는 **머지한 뒤 담당자가 직접 닫는다**

---

## 7. 커밋 크기

- **한 커밋 = 한 가지 일**
- 공백 · 들여쓰기 정리를 기능 코드와 섞지 않는다
- `struct thread` 필드를 더하거나 바꾸면 **커밋을 따로** 만든다 → `chore(thread): …`
  - 셋이 모두 고치는 곳이라 리뷰에서 바로 보여야 한다
- 왜 작게 나누나
  - 우리 팀은 **merge commit 만** 쓴다 (squash 안 함) → 커밋이 전부 그대로 남는다
  - 그래서 커밋 하나하나가 읽혀야 하고, 문제가 생기면 커밋 단위로 되돌릴 수 있어야 한다

---

## 8. 고치기

| 언제 | 해도 되나 | 어떻게 |
|---|---|---|
| push **전** | ✅ | 마지막 커밋 메시지 · 내용 고치기 : `git commit --amend` |
| push **후** | ❌ | 고치지 않는다. 필요하면 새 커밋을 더한다 |

- push 후 `--amend` · rebase 를 하면 force push 가 필요하다 → **금지**
  - 다른 팀원이 이미 받은 기록과 어긋난다

---

## 9. 머지 · 충돌 커밋

- GitHub 가 만드는 머지 커밋 메시지는 **그대로 둔다**
  - 예 : `Merge pull request #20 from picky232/feature-alarm-kim`
- `fixed-` 브랜치에서 충돌을 풀 때 생기는 머지 커밋
  - 제목은 git 이 만든 것 그대로 (`Merge branch 'feature-alarm' into fixed-alarm-kim`)
  - **본문에 무엇을 어떻게 풀었는지 한 줄** 적는다
  - PR 「충돌 해결」 칸에도 같은 내용을 적는다

```
Merge branch 'feature-alarm' into fixed-alarm-kim

thread.h 함수 선언 목록 : 둘 다 살림 (thread_sleep, thread_wakeup)
```

---

## 10. PR 제목

- **커밋 제목과 같은 형식** : `종류(기능): 설명`
  - 머지 커밋 기록에 PR 제목이 남는다
- `gh pr create --fill` 은 커밋 제목을 PR 제목으로 가져온다 → 커밋 제목을 잘 쓰면 그대로 쓰면 된다

| PR 종류 | 제목 예 |
|---|---|
| 개인 → 기능 | `feat(alarm): timer_sleep 재우기` |
| 기능 → dev | `feat(alarm): Alarm Clock 완성` |
| dev → main | `feat: Alarm Clock · Priority Scheduling 반영` |

---

## 11. 예시 — 이슈부터 PR 까지

Alarm 을 셋이 나눠 만들고, kim 이 「재우기」 를 맡았을 때.

**① 이슈** (🛠 기능 구현 템플릿)
```
#12  [기능] alarm - timer_sleep 재우기          라벨: enhancement
  기능 이름       : alarm
  구현할 함수     : devices/timer.c: timer_sleep()
                   threads/thread.c: thread_sleep()
  통과시킬 테스트 : alarm-single, alarm-multiple
  작업 브랜치     : feature-alarm-kim
```

**② 커밋** (`feature-alarm-kim` 에서, 이슈 번호 없이)
```
chore(thread): struct thread 에 wakeup_tick 추가
feat(alarm): thread_sleep 으로 sleep list 에 넣고 block
feat(alarm): timer_sleep 이 바쁜 대기 대신 thread_sleep 호출

    바쁜 대기는 CPU 를 계속 써서 idle tick 이 0 으로 나온다.
    make check : PASS 0/27 → 2/27 (alarm-single, alarm-multiple)
```

**③ PR** (`feature-alarm-kim` → `feature-alarm`)
```
제목 : feat(alarm): timer_sleep 재우기

## 관련 이슈
#12                       ← Closes 없이 번호만
## PR 종류
- [x] 개인 → 기능
## 테스트 결과 (make check)
- 이번 PR 전: PASS 0 / 27
- 이번 PR 후: PASS 2 / 27
```
그리고 PR 오른쪽 **Development** 에서 `#12` 연결.

**④ 그러면 보이는 것**

| 어디 | 무엇이 |
|---|---|
| #12 이슈 타임라인 | "kim mentioned this in PR #20" |
| #12 이슈 Development 칸 | PR #20 |
| 팀 깃 프로젝트 | #12 의 「Linked pull requests」 칸에 #20 |
| 커밋 기록 | `feat(alarm): …` 세 줄 + `Merge pull request #20 from …/feature-alarm-kim` |

**⑤ 닫기**
- PR #20 머지 → #12 는 열린 채로 남는다
- kim 이 #12 를 직접 닫는다

> 커밋 = 무엇을 했나, PR = 어느 이슈의 일인가. 커밋에 번호가 없어도 이슈 → PR → 커밋이 모두 이어진다.

---

## 12. 좋은 예 · 나쁜 예

| ❌ 나쁜 예 | 왜 | ✅ 좋은 예 |
|---|---|---|
| `수정` | 무엇을 고쳤나 모름 | `fix(alarm): 깨울 시각이 같은 스레드를 모두 깨우기` |
| `feat: alarm 구현` | 기능이 괄호 밖 · 너무 큼 | `feat(alarm): thread_sleep 으로 sleep list 에 넣고 block` |
| `feat(alarm): timer_sleep 구현하고 공백 정리하고 주석 추가` | 한 커밋에 세 가지 | 기능 · 공백 · 주석을 커밋 세 개로 |
| `Feat(Alarm): Add timer sleep.` | 대문자 · 영어 · 마침표 | `feat(alarm): timer_sleep 재우기` |
| `fix(alarm): 버그 수정 Closes #12` | 커밋에 이슈 번호 · 닫히지도 않음 | 번호는 PR 「관련 이슈」 칸에 `#12` |
| `feat(feature-alarm-kim): …` | 괄호 안은 기능 이름만 | `feat(alarm): …` |
| `wip` · `ㅁㄴㅇㄹ` 을 push | 기록이 전부 남는다 | push 전에 `--amend` 로 정리 |
