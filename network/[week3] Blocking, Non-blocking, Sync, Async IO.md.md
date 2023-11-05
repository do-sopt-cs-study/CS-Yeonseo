# 📚 Blocking, Non-blocking I/O: 동기식과 비동기식 I/O의 차이와 활용

# 🌟 1. I/O란?

### `1. I/O란?`

- **I/O란 입력(Input)과 출력(Output)을 함께 일컫는 말이다.**
  - ex) 소켓의 read/Send 과정 : 두 대 이상의 컴퓨터끼리 서로 네트워크 통신을 한다고 가정할 때, 한 컴퓨터에서 출력(send)하고, 다른 한 컴퓨터에서 입력(read)를 받는 과정을 통해 통신한다.
- I/O 작업은 User 레벨에서 직접 수행할 수 없고, 실제 IO 작업을 수행하는 위치는 Kernal(커널 = 운영체제)에서만 가능하다.
  - cf) User process(or Thread)는 Kernal에게 요청을 하고, 작업 완료 후 Kernal에 반환하는 결과를 기다릴 뿐이다!

### `2. I/O가 성능에 미치는 영향`

- I/O에서 발생하는 시간은 CPU를 사용한 시간과 대기 시간 중, `대기시간`에 속하기 때문에 I/O가 많아진다는 것은 애플리케이션이 연산을 할 때까지 CPU가 아무것도 못하고 대기하는 시간이 길어진다는 의미이다.
  → 즉, **애플리케이션의 처리 속도 저하로 이어진다.**

<br/>

---

# 🌟 2. Blocking vs Non-Blocking

> **Blocking vs Non-Blocking 구분의 관심사는 `제어권 = 즉, 호출 되는 함수가 바로 리턴 되는지, 아닌지` 이다!**

### **`1. Blocking I/O Model`**

![Untitled (6)](https://github.com/do-sopt-cs-study/CS-Yeonseo/assets/77691829/e44ef68f-f768-4af8-a85f-286c87945086)

- CPU의 기본적인 I/O 모델, 리눅스에서 모든 소켓 통신은 기본적으로 blocking 방식으로 동작한다.
- 호출된 함수가 자신의 작업을 모두 마칠 때까지 **호출한 함수에게 제어권을 넘겨주지 않고 대기하게 하는 방식**
- 자신의 작업을 진행하다가 다른 주체의 작업이 시작되면 **다른 작업이 끝날 때까지 기다렸다가 자신의 작업을 시작하는 것**
- Blocking I/O 과정 요약
  - 1. Process(Thread)가 Kernel에게 I/O를 요청하는 함수를 호출한다.
  - 2. Process(Thread)는 작업 결과를 반환 받을 때까지 대기한다/
  - 3. Kernel이 작업을 완료하면 작업 결과를 반환 받는다
       ⇒ _ex. 카카오톡이 사용자가 메시지를 전송할 때까지 대기하고 있는 상태_
- **말 그대로 block이 되고, 어플리케이션에서 다른 작업을 수행하지 못하고 대기하게 되므로 Resource 낭비가 심하다.**

### **`2. Non-Blocking I/O Model`**

![Untitled (7)](https://github.com/do-sopt-cs-study/CS-Yeonseo/assets/77691829/82a6f5fa-386d-4d35-afbd-a31c5e698a03)

- 호출된 함수가 바로 리턴해서 호출한 함수에게 **제어권을 넘겨주고, 호출한 함수가 다른 일을 할 수 있는 기회를 줄 수 있는 방식**
- 다른 주체의 작업에 **관련없이 자신의 작업을 하는 것**
- Non-Blocking I/O 과정 요약
  - 1. read I/O를 하기 위해 system call 한다.
  - 2. 커널의 I/O 작업 완료 여부와 관계없이 즉시 응답한다.
    - 이는 커널이 system call을 받자마자 CPU 제어권을 다시 애플리케이션에 넘겨주는 작업이다.
  - 3. 애플리케이션은 I/O 작업이 완료되기 전에 다른 작업을 수행 가능하다.
  - 4. 애플리케이션은 다른 작업 수행 중간중간에 system call을 보내 I/O가 완료되었는지 커널에 요청하고, 완료되면 I/O 작업을 완료한다.
- 그러나 **반복적으로 system call이 발생하기 때문에 이것 또한 Resource 낭비**가 된다.
  - Non-Blocking I/O는 데이터를 입력할 때만 전송하는 게 아니라 주기적으로 계속 반복하기 때문이다.
    ⇒ **`Non-Blocking I/O 문제인 반복적인 system call 호출을 해결하기 위해, I/O 이벤트 통지 모델이 도입되었다!!`**

<br/>

---

# 🌟 3. Synchronous vs Asynchronous

> I/O 이벤트 통지 방식에는 동기/비동기(synchronus/asynchronus) 모델로 분류 가능하다.
>
> ⇒ I/O 작업 상황(결과) 반환 방식에 따라 sync, async 방식으로 분류된다.
>
> 즉, Sync vs Async 구분의 관심사는 `결과의 처리 = 즉, 호출되는 함수의 작업 완료 여부를 누가 신경쓰는지!`이다.

### **`1. Synchronous(동기)`**

![Untitled (8)](https://github.com/do-sopt-cs-study/CS-Yeonseo/assets/77691829/96e6b9c8-371f-4468-940d-8a95e84de68b)

- **I/O 작업이 진행되는 동안 유저 프로세스는 결과를 기다렸다가 이벤트(결고)를 직접 처리하는 방식이다.**
  - 이때, 유저 프로세스는 blocking 방식처럼 작업이 완료될 때까지 기다릴수 있고, non-blocking 방식처럼 커널에 계속 요청하는 방식으로 기다릴 수 있다.
    → **결론은 `기다린다!!`**
- 결국 notify를 유저 프로세스가 담당하여 주체적으로 진행하며, 커널은 유저 프로세스의 요청에 수동적으로 응답한다.

### **`2. **Asynchronous(비동기)`\*\*

![Untitled (9)](https://github.com/do-sopt-cs-study/CS-Yeonseo/assets/77691829/fdd372f5-8280-436e-9e14-c55ac1459a43)

- **I/O 작업이 진행되는 동안 유저 프로세스는 관심이 없고, 그저 자신의 일을 하다가 이벤트 핸들러에 의해 알림이 오면 처리하는 방식이다.**
- 결국 notify를 커널이 담당하여 주체적으로 진행하며, 유저 프로세스는 수동적인 입장에서 통지가 오면 그때 I/O 처리를 한다.
  - 즉, 이벤트 핸들러, callback에 의해 운영체제에서 처리 결과 통지받는다.

---

# 🌟 4. Blocking/Non-Blocking & Sync/Async 방식의 크로스오버

### ❓요약

![Untitled (10)](https://github.com/do-sopt-cs-study/CS-Yeonseo/assets/77691829/7a5a253a-8e00-4fb5-bf06-21dfea10005e)

> `**Blocking`: I/O 작업 기다림 O / 대기 큐 stay O\*\*
>
> `**Non-Blocking`: I/O 작업 기다림 O / 대기 큐 stay X\*\*
>
> **→ _(일반적으로 아무런 언급 없이 Blocking/Non-Blocking I/O를 말한다면 Synchronous 방식을 말한다!)_**
>
> `**Synchronous`: I/O 작업 기다림 O / 대기 큐 stay X\*\*
>
> `**Asynchronous` : I/O 작업 기다림 X / 대기 큐 stay X\*\*

### `1. Sync Blocking` & `2. Sync Non-Blocking`

⇒ 일반적으로 구체적 언급 없이 Blocking / Non-Blocking I/O를 말하는 것은 Synchronous 방식과 같으므로, Blocking / Non-Blocking 방식을 정리할 때의 자료를 참고하면 된다.

### 3. `Async Blocking I/O`

![Untitled (11)](https://github.com/do-sopt-cs-study/CS-Yeonseo/assets/77691829/a2fab79e-1aa8-46e0-a36a-a0436096e977)

- 이 모델에서는 I/O는 non-blocking이지만, 통지(notification)는 blocking 방식으로 하도록 되어있다.
  - select() 는 유저 프로세스를 block 한다.
  - select() 는 데이터가 사용이 가능해지면, 통지를 받게됨. 통지를 받으면 block을 푼다.
- 즉, I/O 작업 자체에 의해 block 되는 것이 아니라, system call에 대한 커널의 응답이 block된다.
- 사실 의도적으로 이 모델을 쓰는 경우는 거의 없다고 할 수 있고, Async Non-Blocking I/O 방식을 사용하는데, 그 과정 중 하나가 Blocking 방식으로 동작하는 경우 Aysnc Blocing I/O로 동작할 수 있다.
  - IBM에서의 이 분류(aysnc blocking)에 오류가 있다는 의견도 있다!

### 4. `Async Non-Blocking I/O`

![Untitled (12)](https://github.com/do-sopt-cs-study/CS-Yeonseo/assets/77691829/100ad67d-5041-467e-85b8-e513ea54ba03)

- 시스템 콜이 들어오면, 커널은 I/O 작업의 완료 여부와는 무관하게 **즉시 응답을 해준다.**
  → **유저 프로세스는 I/O 가 완료 되기 전에도 다른 작업을 할수 있다.**
- IO 처리는 백그라운드에서 실행되다가, **완료되면 커널이 유저 프로세스에게 알려준다.**
  - 이 점이 sync non-blocking I/O와의 차별점!!
    ⇒ sync nonblocking I/O는 유저 프로세스가 I/O 완료 여부를 커널에게 계속 물어봐야 했지만,
    **async non-blocking I/O는 IO 완료가 되면 그때 커널이 유저프로세스에게 알려주는 방식이다.**
  - 시스템 호출이 적어 성능과 자원의 효율적 사용 관점에서 가장 유리한 모델이라고 할 수 있다.

---

출처

https://junshock5.tistory.com/148

https://didu-story.tistory.com/307

[https://limdongjin.github.io/concepts/blocking-non-blocking-io.html#운영체제-교재에-기반한-분류](https://limdongjin.github.io/concepts/blocking-non-blocking-io.html#%E1%84%8B%E1%85%AE%E1%86%AB%E1%84%8B%E1%85%A7%E1%86%BC%E1%84%8E%E1%85%A6%E1%84%8C%E1%85%A6-%E1%84%80%E1%85%AD%E1%84%8C%E1%85%A2%E1%84%8B%E1%85%A6-%E1%84%80%E1%85%B5%E1%84%87%E1%85%A1%E1%86%AB%E1%84%92%E1%85%A1%E1%86%AB-%E1%84%87%E1%85%AE%E1%86%AB%E1%84%85%E1%85%B2)
