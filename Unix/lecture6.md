# Unix Lecture 6

## Process Termination

#### 2023.12.18.(월)

[[운영체제] Processes (2) - Process Creation & Termination](https://jooona.tistory.com/6)

1. Process Creation

모든 process는 자신만의 정수 값으로 표현되는 number를 가집니다. 이를 Process Identifier, 줄여서 PID라고 부릅니다.
참고로 최초에 존재하는 process는 swapper라고 불리며 pid는 0입니다. 하지만 swapper는 사용자 모드가 아닌 kernel의 일부로 페이징을 담당하기 때문에 보통 root parent process로 취급하지 않고, 대신 시스템의 시작과 종료에 사용되는 process, 즉 init process를 root parent process라고 부르며 이 init process의 pid는 1입니다. (현재는 Systemd라고 한다고 합니다.) 그리고 process는 자기 자신을 복제하여 새로운 process를 만들 수 있습니다. 이 과정에서 원래의 process는 부모 process, 그리고 새롭게 복제된 process는 자식 process라고 불리게 됩니다. 이렇게 자기 복제를 통해 process들을 만들고 나면 아래와 같이 tree 모양의 자료 구조가 탄생하게 됩니다.

Process를 생성하는 데 있어서 가장 중요하게 사용되는 함수들이 몇 가지 있는데, 바로 fork(), exec(), 그리고 wait() 입니다.

fork() system call은 새로운 process를 생성하고, 자신의 코드와 데이터들을 복제하여 넘겨줍니다. 물론 자식 process는 자신의 pid를 할당 받게 됩니다. 이렇게 생성된 부모 process와 자식 process는 동시에 실행되게 됩니다.

exec() system call은 process의 메모리를 새로운 프로그램으로 덮는 역학을 합니다. 새로운 프로그램을 불러와서 실행시키는 것이죠. 따라서 원래 부모 process로부터 복제되었던 프로그램 코드는 새로운 코드로 덮어져 사라지게 됩니다.

마지막으로 wait() system call은 부모 process가 실행을 마친 뒤 자식 process가 종료될 때까지 기다릴 때 사용합니다. 이때 부모 process는 wait 상태가 되어 wait queue에서 대기하게 됩니다.

### 2. Process Termination

Process는 종료를 위해 exit() system call을 호출합니다. exit() system call이 호출되게 되면 process가 가지고 있던 (메모리와 같은) resource들을 풀어주게 되고, 부모 process에게 자신의 상태를 전달합니다. 그와 별개로 부모 process는 다음과 같은 특수한 상황에서 자식 process를 종료시킬 수 있습니다.

1. 자식 process가 과도하게 많은 resource를 사용할 때.
2. 자식 process의 작업이 더 이상 필요 없을 때.
3. 부모 process가 작업을 종료했고, 해당 운영체제가 orphan process를 허용하지 않을 때.

그렇다면 orphan process란 뭘까요? 우선 Orphan Process란 부모 process가 이미 죽었는데 자식 process가 살아 있는 경우를 말합니다. 부모가 사라졌기 때문에 고아 process라고 부르는 것인데요, 이 경우 Systemd (init process) 라는 process가 대신 부모의 역할을 맡게 되어 wait() system call을 호출하게 됩니다.

비슷한 개념으로 `Zombie Process`라는 것도 있습니다. Zombie process는 반대로 자식 process는 실행이 종료되었는데 아직 부모 process가 wait() system call을 호출하지 않은 경우를 말합니다. 자식 process가 exit() system call을 실행하면 모든 resource들을 해제시키지만 부모 process가 자식 process의 상태를 알고 싶어 할 상황을 대비하여 최소한의 정보는 kernel에 저장하고 있게 됩니다. 물론 부모 process가 wait() system call을 호출하면, zombie process는 제거됩니다.
