# 🧹 ThreeJ 팀 코드 컨벤션

> 코드를 쓸 때 이 문서 하나만 보면 됩니다.
> 한 줄 원칙 : **원래 핀토스 코드와 똑같은 모양으로 쓴다.** 그래야 PR diff 에 내가 바꾼 줄만 남는다.
> 커밋 · PR 은 **[커밋 컨벤션](./Commit_Convention.md)**, 브랜치는 **[GitHub 전략 문서](./Github_Strategy.md)** 를 보세요.

**목차**
[1. 한눈에 보기](#1-한눈에-보기) ·
[2. 모양](#2-모양) ·
[3. 이름 (snake_case)](#3-이름-snake_case) ·
[4. 주석](#4-주석) ·
[5. 인터럽트를 끄는 곳](#5-인터럽트를-끄는-곳) ·
[6. 상수 · 리스트 · ASSERT](#6-상수--리스트--assert) ·
[7. 편집기 설정](#7-편집기-설정) ·
[8. PR 전 체크](#8-pr-전-체크)

---

## 1. 한눈에 보기

| 항목           | 규칙                                              | 예                                |
| ------------ | ----------------------------------------------- | -------------------------------- |
| 들여쓰기         | **탭**                                           |                                  |
| 함수 호출 · 정의   | 이름과 `(` 사이 **한 칸**                              | `thread_yield ();`               |
| 함수 정의        | 반환형은 **윗줄**, 이름은 아랫줄 첫 칸, `{` 는 같은 줄            | 아래 2절 양식                         |
| 이름           | **`snake_case`** · 상수 · 매크로는 `UPPER_SNAKE_CASE` | `wakeup_tick` · `PRI_MAX`        |
| 파일 안에서만 쓰는 것 | `static`                                        | `static struct list sleep_list;` |
| 주석           | `/* */` · 새 주석은 **한국어** · 원래 영어 주석은 그대로         | `/* 깨울 시각 (tick). */`            |
| 인터럽트 끄는 곳    | **왜 끄는지 한 줄 주석 필수**                             | 5절                               |
| 원래 코드        | 고칠 일이 없는 줄은 **공백 하나도 안 건드린다**                   |                                  |
| 편집기          | 자동 포맷 **끔** · 저장소 `.vscode/settings.json` 으로 셋이 똑같이 | 7절                               |

---

## 2. 모양

원래 `threads/thread.c` 와 같은 모양이다.

**함수 정의 양식**
```c
/* 한국어로 : 이 함수가 무엇을 하고, 왜 필요한가.
   부르는 쪽이 지켜야 할 조건이 있으면 적는다. (예 : 인터럽트가 꺼진 채로 불러야 한다) */
반환형
함수_이름 (인자_형 인자_이름, ...) {
	지역_변수_선언;

	ASSERT (지킬_조건);

	/* 본문 */
}
```

**원래 코드 예** (`thread.c` 의 `thread_unblock`)
```c
void
thread_unblock (struct thread *t) {
	enum intr_level old_level;

	ASSERT (is_thread (t));

	old_level = intr_disable ();
	...
	intr_set_level (old_level);
}
```

| ✅ 이렇게 | ❌ 이렇게 말고 | 왜 |
|---|---|---|
| `thread_yield ();` | `thread_yield();` | 원래 코드는 이름 뒤 한 칸 |
| 반환형 윗줄 · 이름 아랫줄 | `void thread_unblock (struct thread *t) {` 한 줄 | 원래 코드 모양 |
| `if (a > b) {` | `if(a>b){` | 키워드 뒤 · 연산자 양옆 한 칸 |
| 탭 들여쓰기 | 스페이스 4칸 | 섞이면 diff 전체가 바뀐다 |

- 에디터 자동 포맷은 **끈다** — 각자 끄지 않고 **저장소의 `.vscode/settings.json` 으로 맞춘다** ([7절](#7-편집기-설정))
  - 켜 두면 원래 코드 수백 줄의 모양이 바뀌어 diff 가 커진다

---

## 3. 이름 (snake_case)

**snake_case** = 소문자 단어를 밑줄 `_` 로 잇는다. (`wakeupTick` 처럼 대문자로 잇는 camelCase 는 쓰지 않는다)

| 종류        | 규칙                               | ✅ 예                                              | ❌ 예                           |
| --------- | -------------------------------- | ------------------------------------------------ | ----------------------------- |
| 함수        | `모듈_동작` (원래처럼 파일 · 모듈 이름을 앞에)    | `thread_sleep` · `thread_wakeup` · `timer_ticks` | `sleepThread` · `ThreadSleep` |
| 지역 변수     | 소문자 + `_`                        | `old_level` · `cur` · `next_thread`              | `oldLevel` · `NextThread`     |
| 구조체 필드    | 소문자 + `_` · 무엇을 세는 값인지 이름에       | `wakeup_tick` · `init_priority` · `recent_cpu`   | `wakeupTick` · `t2`           |
| 전역 변수     | 소문자 + `_` · 파일 밖에서 안 쓰면 `static` | `static struct list sleep_list;`                 | `struct list SleepList;`      |
| 상수 · 매크로  | 대문자 + `_`                        | `PRI_MAX` · `TIME_SLICE`                         | `priMax` · `pri_max`          |
| 참 / 거짓 함수 | 질문 모양 (`is_` · `has_`)           | `is_thread` · `is_idle`                          | `check_thread`                |
| 리스트 비교 함수 | `cmp_` + 기준                      | `cmp_priority`                                   | `compare1` · `my_less`        |
| 고정소수점 함수  | `fp_` + 연산                       | `fp_mul` · `fp_div`                              | `mulFixed`                    |

- 줄임말은 원래 코드에 있는 것만 (`cur` · `elem` · `pri` · `intr`)
- 한 글자 이름은 반복문 `i` · 원래 코드의 `t` (스레드) 정도만

**구조체 필드 양식** (`include/threads/thread.h`)
```c
struct thread {
	/* Owned by thread.c. */
	tid_t tid;                          /* Thread identifier. */
	...
	int64_t wakeup_tick;                /* 깨울 시각 (tick). alarm. */
	...
};
```
- 새 필드는 끝에 **한국어 한 줄 주석 + 어느 기능이 쓰는지** (`alarm` · `donation` …)
- 필드 이름 · 위치는 기능마다 **뼈대 이슈에서 먼저 정한다** (셋이 같이 고치는 곳)

---

## 4. 주석

| 규칙 | ✅ 예 |
|---|---|
| `/* */` 만 쓴다 (원래 코드에 `//` 가 없다) | `/* 인터럽트가 꺼진 채로 불러야 한다. */` |
| 새 주석은 **한국어** | |
| 원래 영어 주석은 번역 · 삭제하지 않는다 | diff 가 커진다 |
| **무엇** 보다 **왜** | ❌ `/* old_level 에 저장한다. */` → ✅ `/* 부른 쪽이 이미 인터럽트를 껐을 수도 있어, 끝나면 원래 상태로 되돌리려고 저장한다. */` |
| 함수 위 주석 = 하는 일 + 부르는 조건 | 2절 양식 |
| 필드 끝 주석 = 무엇을 세는 값 + 쓰는 기능 | 3절 양식 |

- 주석 끝에 마침표를 찍는다 (원래 코드 모양)
- 여러 줄 주석은 둘째 줄부터 `   ` (공백 3칸) 들여서 맞춘다 (원래 코드 모양)

---

## 5. 인터럽트를 끄는 곳

- **인터럽트를 끄는 곳마다 무엇을 지키려고 끄는지 한 줄 주석을 단다**
  - 리뷰어는 이 주석을 보고 "끌 필요가 있나 · 너무 길게 끄지 않나" 를 본다
- 끄는 구간은 **짧게** (끄는 동안 타이머가 멈춰 다른 스레드가 못 돈다)
- 끄고 켜는 짝은 원래 코드 그대로 : `old_level = intr_disable ();` … `intr_set_level (old_level);`

**양식**
```c
	enum intr_level old_level;

	/* sleep list 는 타이머 인터럽트도 읽으므로, 고치는 동안 인터럽트를 막는다. */
	old_level = intr_disable ();
	/* 같은 목록을 고치는 짧은 구간 */
	intr_set_level (old_level);
```

> 왜 끄나 : 내가 목록 연결을 반쯤 바꾼 순간 타이머 인터럽트가 와서 같은 목록을 읽으면, 반쯤 바뀐 연결을 따라가다 커널이 멈춘다.

---

## 6. 상수 · 리스트 · ASSERT

| 항목 | 규칙 | 예 |
|---|---|---|
| 고정된 숫자 | 이름 붙인 상수로 (`#define`) | `#define DONATION_DEPTH_MAX 8` (기부 깊이) |
| 리스트 | 원래 `lib/kernel/list.h` 함수만 쓴다 (직접 연결 고치지 않기) | `list_insert_ordered` · `list_sort` · `list_push_back` |
| 비교 함수 | `list_less_func` 모양 그대로 · 이름은 `cmp_` + 기준 | `cmp_priority` |
| `ASSERT` | 꼭 지켜야 할 조건은 `ASSERT` 로 적는다 (어기면 바로 멈춰 찾기 쉽다) | `ASSERT (PRI_MIN <= p && p <= PRI_MAX);` |
| 디버그 출력 | `printf` 는 **PR 전에 지운다** (테스트가 출력을 비교해서 남으면 FAIL) | |

---

## 7. 편집기 설정

> 셋 다 같은 개발 컨테이너에서 VS Code C/C++ 확장 (`cpptools`) 을 쓴다. 이 확장의 포맷터는 핀토스와 모양이 다르다 (스페이스 들여쓰기 · 중괄호 다음 줄).
> 한 명이라도 저장할 때 자동 포맷이 켜져 있으면 `thread.c` 전체 모양이 바뀌어 PR diff 가 수백 줄이 되고, 다른 둘과 거의 확실히 충돌한다.

- 그래서 **각자 설정에 맡기지 않고** 저장소에 `.vscode/settings.json` 을 넣는다
  - 작업 영역 설정이라 **개인 설정보다 우선**한다 → 저장소를 열면 셋이 자동으로 같아진다
  - 받자마자 적용된다 (`devcontainer.json` 의 `settings` 는 컨테이너를 다시 빌드해야 적용)

**`.vscode/settings.json`**
```json
{
    "editor.formatOnSave": false,
    "editor.formatOnPaste": false,
    "editor.formatOnType": false,
    "C_Cpp.formatting": "disabled",
    "editor.insertSpaces": false,
    "editor.detectIndentation": false,
    "files.trimTrailingWhitespace": false,
    "files.insertFinalNewline": false,
    "files.eol": "\n"
}
```

| 설정 | 값 | 왜 |
|---|---|---|
| `editor.formatOnSave` · `formatOnPaste` · `formatOnType` | `false` | 저장 · 붙여넣기 · 타이핑 때 자동 포맷 끔 |
| `C_Cpp.formatting` | `"disabled"` | C/C++ 확장 포맷터 자체를 끔 (Format Document 를 눌러도 안 바뀜) |
| `editor.insertSpaces` · `editor.detectIndentation` | `false` · `false` | 탭 키가 늘 **탭** 을 넣는다 |
| `files.trimTrailingWhitespace` | `false` | 원래 코드의 줄 끝 공백을 지우면 그 줄이 바뀐 걸로 잡힌다 |
| `files.insertFinalNewline` | `false` | 파일 끝 줄바꿈을 멋대로 더하지 않는다 |
| `files.eol` | `"\n"` | 새 파일도 줄 끝 LF (리눅스 컨테이너 · 핀토스 스크립트와 같게) |

- 확인법 : 아무것도 고치지 않고 `thread.c` 를 열어 저장 → `git status` 에 아무것도 안 나오면 된다

---

## 8. PR 전 체크

- [ ] 탭 들여쓰기 · 이름 뒤 한 칸 · 반환형 윗줄
- [ ] 이름은 모두 `snake_case` (상수만 대문자)
- [ ] 고칠 일 없는 원래 줄은 안 건드렸다 (diff 에 공백 · 줄 끝만 바뀐 줄이 없다 → 있으면 [7절](#7-편집기-설정) 설정 확인)
- [ ] 새 주석은 한국어 `/* */` · "왜" 가 들어 있다
- [ ] 인터럽트를 끄는 곳마다 이유 주석이 있다
- [ ] 디버그 `printf` 를 지웠다
