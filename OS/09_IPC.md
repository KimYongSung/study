# IPC ( InterProcess Communication)

## 프로세스간 커뮤니케이션

* 프로세스는 다른 프로세스의 공간을 접근할 수 없다
* 프로세스간의 통신 방법 제공
  * 성능을 높이기 위해 여러 프로세스를 만들어서 동시 실행
  * 이 때 프로세스간 상태 확인 및 데이터 송수신이 필요

## 다양한 IPC 기법

* **저장매체** 사용
  * file 사용
    * 간단히 다른 프로세스에 전달할 내용을 파일에 쓰고, 다른 프로세스가 해당   파일을 읽으면 됨
    * 성능이 느리다.
    * 실시간성도 떨어진다.

* **커널** 사용
  * 대부분의 IPC 기법은 결국 *커널 공간을 활용*한다.
  * 기법 종류
    * Message Queue
    * Shared Memory
    * Pipe
    * Signal
    * Semaphore
    * Socket  

### 파이프 (Pipe)

![파이프](./img/pipe.png)

* 기본 파이프는 단방향 통신
* fork()로 자식 프로세스를 만들었을 때, 부모와 자식간의 통신
* 2개의 공간을 가지는 int 배열을 파라미터로 받음.
  * 부모는 int[1] 으로 write
  * 자식은 int[0] 으로 read

### 메시지 큐 (message queue)

![메시지 큐](./img/message_queue.jpg)

* FIFO 정책으로 데이터 전송
* 부모/자식이 아니라, 어느 프로세스간에도 데이터 송수신이 가능
* 먼저 넣은 데이터가, 먼저 읽혀진다.
* 단방향 및 양방향도 가능하다.
  * A,B 프로세스가 각각 A->B , B->A로 전송하는 queue를 생성하여 가능.

### 공유 메모리 (shared memory)

![공유 메모리](https://www.softprayog.in/images/xshared-memory.png.pagespeed.ic.lPvwNQSSC_.webp)

* 노골적으로 kernel space에 메모리 공간을 만들고, 해당 공간을 변수처럼 쓰는 방식
* message quque 처럼 FIFO 방식이 아니라, 해당 메모리 주소를 마치 변수처럼 접근하는 방식
* 공유메모리 key를 가지고, 여러 프로세스가 접근 가능
