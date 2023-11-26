# 📚 PCB와 Context Switching: 프로세스 정보 저장 및 전환

## 🌟 PCB(Process Control Block)란?

![Untitled (9)](https://github.com/do-sopt-cs-study/CS-Yeonseo/assets/77691829/4c35167d-5ac8-4cc6-b667-355216709600)

### 1. Process Management & Process Metadata

- `Process Management`
  : CPU 프로세스가 여러 개일 때, CPU 스케줄링을 통해 관리하는 것을 말한다.
  → 이때, CPU는 각 프로세스들이 누군지 알아야 관리가 가능하다!
  ⇒ 이를 알려주는 지표인, 각 프로세스들의 특징이 바로 **Process Metadata**이다!
- `Process MetaData`
  - Process ID
  - Process State
  - Process Priority
  - CPU Registers
  - Owner
  - CPU Usage
  - Memeory Usage
  **⇒ 이러한 process metadata는 프로세스가 생성되면, PCB에 저장된다!**

### 2. PCB(Process Control Block)

> 프로그램 실행 → 프로세스 생성 → 프로세스 주소 공간에 (코드, 데이터, 스택) 생성→ 이 프로세스의 메타데이터들이 PCB에 저장

- 특정 프로세스에 대한 중요한 정보를 저장하고 있는 운영체제의 자료구조
- **운영체제는 프로세스를 관리하기 위해 프로세스의 생성과 동시에 고유한 PCB를 생성한다.**
- `PCB가 필요한 이유` : 앞으로 다시 수행할 대기 중인 프로세스에 관한 저장 값을 저장하기 위해 !
  - 프로세스는 CPU를 할당받아 작업을 처리하다가도 프로세스 전환이 발생하면 진행하던 작업을 저장하고 CPU를 반환하는데, 이 때 작업의 진행상황을 모두 PCB에 저장한다.
  - 그리고 다시 CPU를 할당받게 되면 PCB에 저장되어 있던 내용을 불러와 이전에 종료됐던 시점부터 다시 작업을 수행한다.

---

## 🌟 Context Switiching이란?

### 1. Context Switiching

- **CPU가 이전의 프로세스 상태를 PCB에 보관하고, 또 다른 프로세스의 정보를 PCB에 읽어 레지스터에 적재하는 과정**
- `Context Switching이 발생하는 경우?`
  - 실행되고 있는 프로세스가 빠지고 새로운 프로세스가 CPU를 받을 때 발생한다.
  - 보통 인터럽트가 발생하거나, 실행 중인 CPU 사용 허가 시간을 모두 소모하거나, 입출력을 위해 대기 해야 하는 경우 Context Switiching이 발생한다.
- `Context Switching이 필요한 이유?`
  - CPU는 한 번에 하나의 프로세스만 수행할 수 있지만 실생활에서 우리는 여러 개의 프로세스를 동시에 수행하는 것처럼 보이게 하기 위해서 Context Switching을 사용한다.
- `Context Switiching 과정`
  1. Task의 대부분 정보는 Register에 저장되고 PCB(Process Control Block)로 관리된다.
  2. 현재 실행하고 있는 Task의 PCB 정보를 저장한다. (Process Stack, Ready Queue)
  3. 다음 실행할 Task의 PCB 정보를 읽어 Register에 적재하고 CPU가 이전에 진행했던 과정을 연속적으로 수행할 수 있다.

### 2. Context Switching OverHead

- Context Switching을 하면, overhead가 발생한다.
  → BUT 프로세스 작업 중에는 Overhead를 감수해야 하는 상황이 있다.
  ```
  프로세스를 수행하다가 입출력 이벤트가 발생해서 대기 상태로 전환시킴
  => 이때, CPU를 그냥 놀게 놔두는 것보다 다른 프로세스를 수행시키는 것이 효율적
  ```
- 즉, CPU에 계속 프로세스를 수행시키도록 하기 위해서 다른 프로세스를 실행시키고 Context Switching 하는 것이다.
- 이를 통해 사용자에게 빠르게 일처리를 제공해주기 위함을 목적으로 가진다.
