# xv6 CFS Scheduler Project Wrap-up

## 1. 프로젝트 목표

기존 xv6의 round-robin scheduler를 CFS(Completely Fair Scheduler) 방식으로 변경했다. 실행 가능한 프로세스는 `vruntime`을 기준으로 red-black tree에 저장하고, scheduler는 항상 가장 작은 `vruntime`을 가진 프로세스를 선택한다.

이 과제에서 time slice는 nice/weight에 따라 달라지지 않는다. 모든 runnable process는 다음 식으로 동일한 길이의 time slice를 받는다.

```text

time slice = max(CFS_LATENCY / number of runnable processes,

                 CFS_MIN_GRAN)

```

nice 값에 따른 weight의 차이는 time slice가 아니라 실행 후 증가하는 `vruntime`에 반영된다. weight가 큰 프로세스는 같은 tick을 실행해도 `vruntime`이 더 천천히 증가하므로 더 자주 선택된다.

---

## 2. Step 1 — CFS와 red-black tree 자료구조 정의

### `rbtree.h`

Red-black tree가 사용할 자료구조와 공개 함수들을 정의했다.

- `struct rb_node`

  - 각 프로세스에 포함되는 tree node이다.

  - `parent`, `left`, `right`: tree 연결 정보

  - `key`: 프로세스의 `vruntime`

  - `tie_breaker`: 동일한 `vruntime`을 구분하기 위한 PID

  - `color`: `RB_RED` 또는 `RB_BLACK`

  - `linked`: 현재 tree에 연결되어 있는지 표시

  - `owner`: 이 node를 소유하는 `struct proc`를 가리킴

  - `tree`: 현재 node가 속한 tree를 가리킴

  

- `struct rb_root`

  - `root`: tree의 root node

  - `leftmost`: 최소 node를 O(1)에 찾기 위한 cache

  - `size`: tree에 저장된 node 수

  

- `rb_init()`

  - 빈 red-black tree를 초기화한다.

  

- `rb_node_init()`

  - tree에서 분리된 node를 초기화하고 owner process와 연결한다.

  

- `rb_insert()`

  - `(vruntime, pid)` 복합 key를 기준으로 node를 삽입하고 red-black property를 복구한다.

  

- `rb_remove()`

  - node를 제거하고 red-black property를 복구한다.

  

- `rb_first()`

  - cached `leftmost`를 이용하여 가장 작은 key의 node를 O(1)에 반환한다.

  

- `rb_next()`

  - 주어진 node의 in-order successor를 반환한다.

  

- `rb_empty()`

  - tree가 비어 있는지 확인한다.

  

- `rb_linked()`

  - node가 현재 tree에 연결되어 있는지 확인한다.

  

### `cfs.h`

  

CFS run queue와 scheduler가 사용하는 공개 인터페이스를 정의했다.

  

- `CFS_LATENCY = 16`

  - 기본 scheduling period를 tick 단위로 나타낸다.

  

- `CFS_MIN_GRAN = 1`

  - 한 프로세스가 받을 수 있는 최소 time slice이다.

  

- `struct cfs_rq`

  - `tree`: runnable process를 보관하는 red-black tree

  - `nr_runnable`: 현재 scheduling population에 포함된 프로세스 수

  - `min_vruntime`: run queue가 추적하는 최소 `vruntime`

  

### `proc.h`

  

`struct proc`에 다음 CFS 관련 필드를 추가했다.

  

- `nice`: 프로세스의 nice 값

- `weight`: nice 값에 대응하는 scheduling weight

- `vruntime`: CFS가 사용하는 가상 실행 시간

- `node`: run queue에 삽입되는 red-black tree node

- `slice_ticks`: 현재 dispatch에서 허용된 time slice

- `used_ticks`: 현재 dispatch에서 실제로 사용한 tick 수

- `cfs_active`: 프로세스가 CFS runnable population에 포함되어 있는지 표시

  

---

  

## 3. Step 2 — Red-black tree 구현

  

### `cfs.c`

  

과제에서 수정 가능한 파일 범위에 맞추어 red-black tree의 실제 구현을 `cfs.c`에 작성했다.

  

#### 내부 helper 함수

  

- `node_color()`

  - null leaf를 black으로 취급하여 node의 색을 반환한다.

  

- `set_color()`

  - null이 아닌 node의 색을 변경한다.

  

- `key_less()`

  - `(vruntime, pid)` 복합 key의 순서를 비교한다.

  

- `subtree_minimum()`

  - subtree의 가장 왼쪽 node를 찾는다.

  

- `rotate_left()`, `rotate_right()`

  - red-black tree 균형 복구에 필요한 left/right rotation을 수행한다.

  

- `insert_fixup()`

  - 삽입 후 발생할 수 있는 red-black property 위반을 복구한다.

  

- `transplant()`

  - 삭제 과정에서 한 subtree를 다른 subtree로 교체한다.

  

- `delete_fixup()`

  - black node 삭제 후 발생할 수 있는 red-black property 위반을 복구한다.

  

#### 공개 함수

  

`rbtree.h`에 선언한 `rb_init()`, `rb_node_init()`, `rb_insert()`, `rb_remove()`, `rb_first()`, `rb_next()`, `rb_empty()`, `rb_linked()`를 구현했다.

  

중복 삽입과 잘못된 tree에서의 삭제를 막기 위해 `linked`와 `tree` 정보를 검사하도록 예외처리도 추가했다.

  

---

  

## 4. Step 3 — CFS run queue 인터페이스 구현

  

### `cfs.c`

  

- `cfs_refresh_min()`

  - tree의 `leftmost` node를 이용하여 `min_vruntime`을 갱신한다.

  

- `cfs_insert_proc()`

  - 프로세스의 `(vruntime, pid)`를 이용하여 내부 tree에 삽입한다.

  

- `cfs_rq_init()`

  - red-black tree, runnable process 수, `min_vruntime`을 초기화한다.

  

- `cfs_weight_for_nice()`

  - nice 범위 `-20`부터 `19`까지의 weight를 반환한다.

  - 범위를 벗어난 nice 값에는 `0`을 반환한다.

  

- `cfs_account_runtime()`

  - 현재 dispatch에서 사용한 tick을 다음과 같이 `vruntime`으로 변환한다.

  

```text

delta_vruntime = used_ticks * 1024 * 1024 / weight

vruntime += delta_vruntime

```

  

  - 첫 번째 `1024`는 nice 0의 기준 weight이고, 두 번째 `1024`는 정수 연산의 정밀도를 유지하기 위한 fixed-point scale이다.

  - 반영 후 `used_ticks`를 0으로 초기화한다.

  

- `cfs_enqueue()`

  - 새로 runnable이 된 프로세스를 tree에 넣고 `nr_runnable`을 증가시킨다.

  

- `cfs_dequeue()`

  - sleep 또는 exit 등으로 runnable population을 떠나는 프로세스를 제거하고 `nr_runnable`을 감소시킨다.

  

- `cfs_pick_next()`

  - 최소 `vruntime` 프로세스를 tree에서 꺼내 scheduler에 반환한다.

  - 프로세스는 여전히 runnable population의 구성원이므로 `nr_runnable`은 감소시키지 않는다.

  

- `cfs_requeue()`

  - 실행 후 RUNNABLE 상태로 돌아온 프로세스를 갱신된 `vruntime`으로 다시 삽입한다.

  - 기존 runnable population의 구성원이므로 `nr_runnable`은 변경하지 않는다.

  

- `cfs_min_vruntime()`

  - run queue가 추적 중인 최소 `vruntime`을 반환한다.

  

- `cfs_slice_ticks()`

  - 과제에서 요구한 동일 time-slice 정책을 적용한다.

  - runnable process가 많을 때도 최소 1 tick을 보장한다.

  

---

  

## 5. Step 4 — 프로세스 lifecycle과 CFS run queue 연결

  

### `proc.c`

  

기존 round-robin scan을 제거하고 프로세스 상태 변화가 CFS run queue와 항상 일치하도록 수정했다.

  

- `pinit()` 수정

  - `cfs_rq_init()`을 호출하여 전역 CFS run queue를 초기화한다.

  

- `allocproc()` 수정

  - 재사용되는 process table slot의 CFS 필드를 초기화한다.

  - 기본값은 `nice = 0`, weight는 nice 0의 weight이다.

  

- `userinit()` 수정

  - 최초 init process를 RUNNABLE로 만든 뒤 CFS run queue에 삽입한다.

  

- `fork()` 수정

  - child가 parent의 `nice`와 `weight`를 상속한다.

  - child의 `vruntime`을 현재 `min_vruntime`으로 설정하여 새 프로세스가 과도한 이득이나 불이익을 받지 않게 한다.

  - child를 CFS run queue에 삽입한다.

  

- `scheduler()` 수정

  - process table을 순서대로 검색하던 round-robin loop를 제거했다.

  - `cfs_pick_next()`로 최소 `vruntime` 프로세스를 선택한다.

  - 선택된 프로세스의 `slice_ticks`와 `used_ticks`를 설정한 뒤 context switch한다.

  

- `yield()` 수정

  - 실행한 tick을 `cfs_account_runtime()`으로 반영한다.

  - 프로세스를 RUNNABLE로 바꾸고 `cfs_requeue()`로 tree에 다시 삽입한다.

  

- `sleep()` 수정

  - sleep 전에 남은 실행 시간을 `vruntime`에 반영한다.

  - `cfs_dequeue()`로 runnable population에서 제거한 뒤 SLEEPING 상태로 전환한다.

  

- `wakeup1()` 수정

  - 깨어나는 프로세스의 `vruntime`을 현재 `min_vruntime`으로 맞춘다.

  - RUNNABLE 상태로 바꾼 뒤 `cfs_enqueue()`로 삽입한다.

  

- `kill()` 수정

  - SLEEPING 프로세스를 깨우는 경우 `wakeup1()`과 동일하게 `min_vruntime`을 적용하고 run queue에 삽입한다.

  

- `exit()` 수정

  - 마지막으로 사용한 runtime을 반영한다.

  - CFS run queue에서 제거한 뒤 ZOMBIE 상태로 변경한다.

  

- `wait()` 수정

  - ZOMBIE 프로세스가 CFS tree에 남아 있지 않은지 검사하여 lifecycle 오류를 조기에 발견하도록 했다.

  

---

  

## 6. Step 5 — Timer interrupt와 time slice 적용

  

### `trap.c`

  

timer interrupt가 발생할 때 현재 RUNNING 프로세스의 `used_ticks`를 증가시키도록 수정했다.

  

프로세스가 다음 조건을 만족하면 `yield()`를 호출한다.

  

```text

used_ticks >= slice_ticks

```

  

따라서 프로세스는 할당받은 time slice만큼 실행한 후 scheduler로 돌아가고, `yield()`가 runtime 반영과 tree 재삽입을 담당한다.

  

---

  

## 7. Step 6 — `setnice` 시스템콜 구현

  

### `proc.c`

  

- `setnice(int pid, int nice)` 추가

  - `pid == 0`이면 현재 프로세스를 대상으로 한다.

  - nice 범위가 `-20..19`가 아니면 `-1`을 반환한다.

  - process table을 lock으로 보호하면서 대상 PID를 찾는다.

  - PID가 존재하지 않으면 panic하지 않고 `-1`을 반환한다.

  - 대상이 현재 RUNNING이면 기존 weight로 이미 사용한 runtime을 먼저 반영한다.

  - 그 후 `nice`와 `weight`를 변경한다.

  - 성공 시 `0`, 실패 시 `-1`을 반환한다.

  

nice/weight 변경은 red-black tree의 정렬 key인 `(vruntime, pid)`를 변경하지 않으므로 RUNNABLE 프로세스를 제거하거나 다시 삽입할 필요가 없다.

  

### `defs.h`

  

- 커널 내부에서 사용할 `int setnice(int, int)` 선언을 추가했다.

  

### `sysproc.c`

  

- `sys_setnice()` 추가

  - `argint()`로 `pid`와 `nice`를 읽는다.

  - 잘못된 인자에는 `-1`을 반환한다.

  - `proc.c`의 `setnice()` 결과를 사용자에게 그대로 전달한다.

  

### `syscall.h`

  

- `SYS_setnice`를 시스템콜 번호 `24`로 정의했다.

  

### `syscall.c`

  

- `sys_setnice()`를 선언했다.

- 시스템콜 테이블의 `[SYS_setnice]` 위치에 등록했다.

  

### `user.h`

  

- 사용자 프로그램용 `int setnice(int, int)` 선언을 추가했다.

  

### `usys.S`

  

- `SYSCALL(setnice)` stub을 추가하여 사용자 프로그램이 kernel trap을 발생시킬 수 있게 했다.

  

---

  

## 8. 예외처리와 불변조건

  

구현 과정에서 다음 오류가 발생하지 않도록 검사했다.

  

- 동일한 red-black tree node의 중복 삽입

- 다른 tree에 속한 node의 잘못된 삭제

- RUNNABLE이 아닌 프로세스의 enqueue/requeue

- runnable process 수의 중복 증가 또는 감소

- invalid nice 값에서 lock을 해제하지 않고 반환하는 문제

- 존재하지 않는 PID 때문에 kernel panic이 발생하는 문제

- RUNNING 프로세스를 tree에 직접 requeue하는 문제

- sleep, exit, wakeup, kill 과정에서 process state와 run queue 상태가 달라지는 문제

- process table slot 재사용 시 이전 CFS 값이 남는 문제

  

모든 주요 함수에는 함수가 수행하는 작업을 설명하는 영어 주석을 추가했다.

  

---

  

## 9. 최종 검증 결과

  

### 정적 검사와 빌드

  

- `git diff --check`: 통과

- `make -B fs.img xv6.img`: 성공

- scheduler 전용 프로그램인 `schedsmoke`와 `schedticks`가 file-system image에 포함되는 것 확인

  

### `schedsmoke`

  

모든 테스트를 통과했다.

  

- `setnice(0, nice)` 성공

- 존재하지 않는 PID에 대해 `-1` 반환

- 서로 다른 nice 값을 가진 child가 모두 정상 진행 및 종료

- sleeping child의 wakeup과 `wait()` 정상 동작

  

결과: `schedsmoke: PASS (0 failed)`

  

### `schedticks`

  

모든 scheduling trace를 통과했다.

  

- 동일 nice 프로세스의 공정한 CPU 분배

- nice/weight에 따른 CPU share 비율

- 32개 runnable process 모두의 진행성

- 반복적으로 sleep/wakeup하는 프로세스의 정상 동작

  

결과: `schedticks: PASS (0 failed)`

  

### `usertests`

  

기존 xv6 기능에 대한 전체 회귀 테스트도 통과했다.

  

결과: `ALL TESTS PASSED`

  

---

  

## 10. 최종 실행 흐름 요약

  

```text

fork / wakeup / userinit

        |

        v

    cfs_enqueue

        |

        v

red-black tree ordered by (vruntime, pid)

        |

        v

  cfs_pick_next

        |

        v

     RUNNING

        |

        +--- time slice expired ---> yield ---> account runtime ---> cfs_requeue

        |

        +--- sleep ----------------> account runtime ---> cfs_dequeue

        |

        +--- exit -----------------> account runtime ---> cfs_dequeue ---> ZOMBIE

```

  

최종적으로 scheduler는 가장 작은 `vruntime`의 프로세스를 선택하고, nice에 따른 weight는 각 프로세스의 `vruntime` 증가 속도에 반영된다. 이로써 동일 nice 프로세스 사이의 공정성과 서로 다른 nice 프로세스 사이의 가중치 기반 CPU share를 모두 구현했다.