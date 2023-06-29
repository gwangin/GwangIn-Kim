# OS


------
## 4 The Abstraction: The Process
<br>

### 4.1 Introduction<br>
프로세스는 컴퓨터에서 실행 중인 프로그램.<br>
프로그램은 SSD에 저장되어 있지만
프로세스는 메모리에 로드되어 CPU에서 실행됨.<br>
프로레스간의 상호작용을 OS가 담당함.

프로세스를 이해하기위해서는 프로세스의 Maching state를 이해해야 한다.<br>
1. Memory <br>
명령어와 실행 중에 읽고 쓰이는 데이터도 메모리에 있다.<br>

2. Resister
많은 명령어들이 레지스터를 읽거나 업데이트함.<br>
Machine State의 일부인 특별한 Resister도 있다.
(Program Counter)
(Stack Counter)

3. Persistent storage devices (영구 저장 장치)
입출력 정보는 프로세스가 현재 열려 있는 파일 목록에 포함.



### 4.2 Process<br>
#### 운영체제의 Interface에 무조건 포함해야 하는 내용
1. Create : 새로운 프로세스를 생성하는 방법을 제공해야 함.
2. Destroy : 프로세스 생성을 위한 인터페이스가 있을 때, 시스템은 프로세스를 강제로 종료시키는 인터페이스를 제공.
3. Wait : 때로는 프로세스가 실행을 멈출 때까지 기다리는 것이 유용할 수 있음.
4. Miscellaneous Control : 일지정지, 재개
5. Status : 프로세스에 대한 상태정보를 얻는 인터페이스(실행된 시간, 어떤 상태?)


### 4.3 Process Creation<br>
#### Process Creation<br>
1. 운영체제가 코드와 Static Data(초기화된 변수)를 메모리에 로드한다.<br>
= 운영체제가 디스크에서 해당 바이트를 읽고 메모리의 어딘가에 배치한다.
2. 코드와 Static Data가 로드되면 프로그램의 실행 시간 스택 (Running Time Stack)을 위한 메모리를 할당한다.
>main()힘수의 매개변수인 argc 와 argv 배열을 채워 넣는다.
>프로그램의 heap을 위해 일부 메모리를 할당할수도 있다.
3. I/O 입출력 관련 초기화 작업 진행.
>UNIX 시스템에서는 표준입력, 출력, 오류에 대한 세 개의 개방된 파일 디스크립터를 가진다.

- Code와 Static Data를 메모리에 로드하고 , Runtime Stack을 생성하고 초기화하며, I/O와 관련된 다른 작업을 수행.

4. main()함수에서 프로그램을 실행시킨다.

>C / C++ 프로그래밍에서 main() 함수는 프로그램의 entry point로 사용되는 함수이다.<br>
> main()함수는 일반적으로 다음 형식
```c

int main(int argc, char *argv[])
{
    /* code */
    return 0;
}
```
>단순히 argc는 argument count이고 argv는 argument vector의 약자이다.<br>

>heap은 동적으로 할당되는 메모리 영역을 가리킴.
Stack과 달리 프로그램에서 필요에 따라 동적으로 메모리를 할당하고 해제할 수 있다.<br>

### 4.4 Process States<br>

#### Process States<br>
1. Running : 프로세스가 CPU를 사용하고 있는 상태
2. Ready : 프로세스가 실행될 준비가 되어있는 상태
3. Blocked : 다른 이벤트가 발생할 때까지 실행 준비가 되지 않은 상태.

> Running - > Ready : Scheduled <br>
> Ready - > Running : Deschduled <br>

#### Process가 I/O작업을 시작하면 그 Process가 Block된다.
I/O작업은 일반적으로 시간이 소요되는 작업이며, 외부 장치나 파일 시스템과의 상호 작용이 필요합니다.<br>
그래서 I/O 작업이 끝날때까지 프로세스는 실행될 준비가 되지 않은 상태가 된다.<br>
#### Process가 I/O작업을 마치면 그 Process가 Ready된다.

### 4.5 Data Structures<br>

#### Process Control Block (PCB)<br>
운영체제는 각 프로세스에 대한 정보를 저장하기 위해 PCB를 사용한다.<br>

-----------
## 5. Interlude: Process API<br>
실용적인 측면을 다룸, OS API 및 사용방법에 초점을 맞추고 있음.


# UNIX systems
fork() : Create a new process
exit() : Terminate the current process

```C
#include <stdio.h> // printf
#include <stdlib.h> //exit()
#include <unistd.h> //프로세스 관련 함수인 getpid()와 fork()
//PID는 운영체제에서 프로세스를 식별하기 위해 사용되는 고유한 숫자.
int main(int argc, char *argv[])
{
    printf("hello world (pid:%d)\n", (int) getpid());//getpid : process ID , 현재의 process ID를 출력
    int rc = forc(); // 현재의 프로세스를 복제하여 새로운 프로세스를 생성
    // fork()는 부모 프로세스에 자식 프로세스의 PID를, 자식 프로세스에는 0을 반환, 실패시 -1을 반환
    if (rc < 0) 
    {
        //fork failed
        fprintf(stderr, "fork failed\n");
        exit(1);
    }
    else if (rc == 0)
    {
        //child (new process)
        printf("hello, I am child (pid:%d)\n, (int) getpid()");
    }
    else
    {
        //parent gose down this path (main)
        printf("hello, I am parent of %d (pid:%d)\n", rc, (int) getpid());
    }
    
    return -;
}
```

- A의 PID가 12 , A의 자식프로세스 B의 PID가 23 , B의 자식 프로세스 C의 PID가 34이고 A를 실행중인경우

```C
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>

int main(int argc, char *argv[])
{
    pid_t child_pid_b = fork();
    
    if (child_pid_b < 0) 
    {
        // fork failed
        fprintf(stderr, "fork failed\n");
        exit(1);
    }
    else if (child_pid_b == 0)
    {
        // child process B
        printf("hello, I am child B (pid:%d)\n", getpid());
        
        pid_t child_pid_c = fork();
        
        if (child_pid_c < 0) 
        {
            // fork failed
            fprintf(stderr, "fork failed\n");
            exit(1);
        }
        else if (child_pid_c == 0)
        {
            // child process C
            printf("hello, I am child C (pid:%d)\n", getpid());
        }
    }
    else
    {
        // parent process A
        printf("hello, I am parent A (pid:%d)\n", getpid());
        printf("child B process ID: %d\n", child_pid_b);
    }
    
    return 0;
}
```
```C
hello, I am parent A (pid:12)
child B process ID: 23
```

#### fork() 시스템 호출은 부모 프로세스의 복사본을 생성하여 두 개의 독립적인 프로세스를 만들고, 반환 값을 통해 부모와 자식 프로세스를 구별할 수 있게 합니다.



#### exec() system Call
exec() 시스템 호출은 새로운 프로세스를 실행하는데 사용됩니다.<br>

```C
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/wait.h>
int main(int argc, char *argv[]) 
{
printf("hello world (pid:%d)\n", (int) getpid());
int rc = fork();
if (rc < 0) 
{         // fork failed; exit
    fprintf(stderr, "fork failed\n");
    exit(1);
}
    else if (rc == 0) { // child (new process)
    printf("hello, I am child (pid:%d)\n", (int) getpid());
    char *myargs[3];
    myargs[0] = strdup("wc");   // program: "wc" (word count)
    myargs[1] = strdup("p3.c"); // argument: file to count
    myargs[2] = NULL;           // marks end of array
    execvp(myargs[0], myargs);  // runs word count
    printf("this shouldn’t print out");
    }

    else 
    {              // parent goes down this path (main)
    int rc_wait = wait(NULL);
    printf("hello, I am parent of %d (rc_wait:%d) (pid:%d)\n",
            rc, rc_wait, (int) getpid());
    }
return 0;
}
```
1. 프로그램이 시작되면 "hello world" 메시지와 함께 해당 프로세스의 프로세스 ID(PID)가 출력됩니다.
2. 프로세스는 fork() 시스템 콜을 사용하여 자식 프로세스를 생성합니다.
3. fork()가 성공하면 부모 프로세스는 자식 프로세스의 PID를 반환받고, 자식 프로세스는 0을 반환받습니다.
4. 부모 프로세스는 wait() 시스템 콜을 사용하여 자식 프로세스가 실행을 완료할 때까지 대기합니다.
5. 자식 프로세스는 execvp()를 호출하여 "wc"라는 단어 수를 세는 프로그램을 실행합니다. 실제로 이 프로그램은 소스 파일 p3.c에 대해 wc를 실행하여 파일에 포함된 줄 수, 단어 수 및 바이트 수를 알려줍니다.
execvp()는 새로운 프로그램을 실행하고, 현재 프로세스의 메모리를 새로운 프로그램의 메모리로 덮어씁니다. 따라서
자식 프로세스의 execvp() 호출 후에는 "hello, I am child" 메시지가 출력되지 않습니다. exec()가 성공하면 호출자에게 반환되지 않기 때문입니다.
6.자식 프로세스의 실행이 완료되면 부모 프로세스가 wait() 시스템 콜에서 반환됩니다. 이후 부모 프로세스는 자신의 메시지를 출력합니다. bb

> 이러한 과정을 통해 현재 실행 중인 프로세스를 다른 프로그램으로 변환하고 해당 프로그램을 실행한다. exec()는 새로운 프로세스를 생성하지 않고 현재 실행중인 프로그램을 다른 프로그램으로 교체한다.

>fork() , exec()모두 한 프로세스가 다른 프로세스를 실행시키기 위해 사용된다.<br>
>exec에는 execl, execv등 다양한 함수군을 가지고 있다.<br>
>차이점<br>
>fork() 시스템 호출은 새로운 프로세스를 위한 메모리를 할당한다는 것이다.<br>
> fork()를 호출한 프로세스를 새로운 공간으로 전부 복사하게 되고, 원래 프로세스는 원래 프로세스대로 작업을 실행하고 fork()를 이용해서 생성된 프로세스도 fork() 시스템 콜이 수행된 라인의 다음 라인부터 실행된다.<br>

>exec() 는 새로운 프로세스를 위한 메모리를 할당하지 않고, exec()를 호출한 프로세스가 아닌 exec()에 의해 호출된 프로세스만 메모리에 남게 된다.<br>
- >fork() : 새로운 프로세스를 위한 메모리 할당 O , 프로세스가 하나 더 생김!
- >exec() : exec()를 호출한 프로세스가 아닌 exec()에 의해 호출된 프로세스만 메모리에 남게 된다. exec()를 호출한 프로세스의 PID가 새로운 프로세스에 그대로 사용!



- Process API term
>1. PID : process의 이름
>2. fork() 시스템 호출은 새로운 프로세스를 생성하는 데 사용됩니다. 생성자는 부모(parent)라고 불리고, 새로 생성된 프로세스는 자식(child)이라고 불립니다. 실생활에서 종종 일어나는 것처럼, 자식 프로세스는 부모의 거의 동일한 복사본입니다.
>3. wait() 시스템 호출은 부모가 자식 프로세스의 실행이 완료될 때까지 기다릴 수 있게 합니다.
>4. exec() 시스템 호출 계열은 자식 프로세스가 부모와의 유사성에서 벗어나 새로운 프로그램을 실행할 수 있게 합니다.
>5. UNIX 셸은 일반적으로 fork(), wait(), exec()를 사용하여 사용자 명령을 실행합니다. fork와 exec의 분리로 인해 입력/출력 리디렉션, 파이프 및 다른 멋진 기능을 프로그램을 변경하지 않고도 사용할 수 있습니다.
>6. 프로세스 제어는 작업을 일시 정지, 계속 또는 종료시킬 수 있는 신호의 형태로 제공됩니다.
>7. 특정 사용자가 제어할 수 있는 프로세스는 사용자의 개념에 캡슐화되어 있습니다. 운영 체제는 여러 사용자가 시스템에 로그인할 수 있도록 허용하고, 사용자는 자신의 프로세스만 제어할 수 있도록 합니다.
>8. 슈퍼유저는 모든 프로세스를 제어할 수 있으며(그리고 실제로 많은 다른 작업도 수행할 수 있습니다), 보안상의 이유로 드물고 신중하게 이 역할을 맡아야 합니다.


#### exit() 
exit() 함수는 프로그램을 종료할 때 사용.<br>
종료 상태 (exit status)를 지정할 수 있는데, 이 상태는 프로그램이 성공적으로 종료되었는지(return 0), 아니면 오류로 종료되었는지를(return 다른 값 예를 들어 -1) 알려준다.<br>




#### fork() 실습 :
```c
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>

int main() {
    
    pid_t pid;
    
    int x;
    x = 0;
    
    pid = fork();
    
    if(pid > 0) {  // 부모 코드
        x = 1;
        printf("부모 PID : %ld,  x : %d , pid : %d\n",(long)getpid(), x, pid);
    }
    else if(pid == 0){  // 자식 코드
        x = 2;
        printf("자식 PID : %ld,  x : %d\n",(long)getpid(), x);
    }
    else {  // fork 실패
        printf("fork Fail! \n");
        return -1;
    }
    
    return 0;

}
```
#### 실행 결과:
부모 PID : 5036,  x : 1 , pid : 5038<br>
자식 PID : 5038,  x : 2<br>

#### exit() 실습:
-  나누기 기능을 하는 프로그램 만들기
```c

#include <stdio.h>
#include <stdlib.h>

float divide(int a , int b)
{
    if (b == 0)
    {
        return -1;
    }


    return a/b;
}

int main()
{
    int a , b;
    float ans;
    printf("a/b , a와b 를 입력하시오");

    scanf("%d%d", &a, &b);

    ans = divide(a,b);
    if (ans == -1)
    {
        printf("Error!!\n");
    }
    else
    {
        printf("%f\n" , ans);
    }

    return 0;
}
```
#### 실행 결과:
a/b , a와b 를 입력하시오10 5
2.000000
<br>
a/b , a와b 를 입력하시오5 0
Error!!<br>

--------
## 6. Mechanism: Limited Direct Execution

### 6.1 Basic Technique: Limited Direct Execution
우선 제한 없는 Direct Execution : 
CPU에서 프로그램을 직접 실행.<br>
프로세스 목록에서 행당 프로그램의 Tree를 만들고, 메모리 할당하고 , 프로그램 코드를 디스크에서 메모리로 Load 하고, main()함수를 찾아서 실행시키고, 프로그램이 끝나면 메모리를 해제하고, 프로세스 목록에서 제거한다.<br> 

OS<br>
Create entry for process list Allocate memory for program Load program into memory Set up stack with argc/argv Clear registers
Execute call main() Free memory of process
Remove from process list
Figure 6.1: Direct Execution Protocol (Without Limits)

Program<br>
Run main()
Execute return from main


OS<br>
Free memory of process
Remove from process list

하지만 이러한 방법은 다음과 같은 문제가 발생한다.<br>
1. 비효율적이다. 원하지 않는 동작을 수행할 수 있다.
2. 프로세스를 실행하는 동안, OS가 그것을 중지하고 다른 프로세스를 전환하여 CPU를 가상화하는 Time Sharing을 어떻게 구현할까? 이다.<br>

###  6.2 Problem #1: Restricted Operations
- 프로세스가 디스크에 I/O요청을 보내거나 CPU나 메모리와 같은 시스템 지원에 접근하는 등의 제한된 작업을 수행하려는 경우<br>

1. 모든 프로세스가 I/O와 관련된 작업을 원하는 대로 수행할 수 있도록 하는 것이다
이렇게 한다면 시스템을 구축하는 것이 불가능해짐. 프로세스가 디스크를 읽거나 쓰는 , 모든 보호기능이 상실될 수 있다.

따라서 User Mode 라는 프로세서 모드를 도입
- User Mode : 프로세스가 제한된 작업만 수행할 수 있도록 하는 모드
예를 들어 User mode에서 실행 중인 프로세스느 I/O 요청을 보낼 수 없다.

이와 대조적으로 Kernel Mode 라는 모드를 도입
- Kernel Mode : 프로세스가 모든 작업을 수행할 수 있도록 하는 모드
실행되는 코드는 I/O요청을 포함한 권한이 있는 작업을 수행할 수 있다.


- user process가 디스크에서 읽기와 같은 privileged operation을 수행하려고 하면, OS는  user program이 system call을 수행할 수 있다.

system call을 수행하려면 trap 명령어를 실행해야함.

> trap instruction(명령어) : trap 명령어는 프로세스가 현재 실행 중인 명령 흐름을 일시적으로 중단하고, 운영 체제 또는 커널 모드로 전환하여 privileged operation을 수행할 수 있게 해주는 명령어이다.

커널로 점프하고  privilege level to kernel mode 권한 수준을 커널 모드로 올림.
이렇게 되면 privileged operation을 수행할 수 있게 되고 작업이 끝나면 OS는 return-from-trap instruction을 호출한다.
호출한 user program을 반환하면서 privilege level을 user mode로 다시 낮춘다.


#### User Mode  와 Kernel Mode
>유저모드 (User Mode)와 커널모드 (Kernel mode)
>커널에서 중요한 자원을 관리하기 때문에, 사용자가 그 중요한 자원에 접근하지 못하도록 모드를 >2가지로 나눈 것이다.

>- User Mode
>유저(사용자)가 접근할 수 있는 영역을 제한적으로 두고, 프로그램의 자원에 함부로 침범하지 못하는 >모드이다.
>우리는 여기서 코드를 작성하고, 프로세스를 실행하는 등의 행동을 할 수 있다.
>간단하게 "유저 어플리케이션 코드가 유저모드에서 실행된다." 라고 말할 수 있다.

>- Kernel Mode
>모든 자원(드라이버, 메모리, CPU 등)에 접근, 명령을 할 수 있다.
>유저모드와는 비교가 안되게 컴퓨터 내부에서 모든 짓(?)을 할 수 있다고 생각하면 된다.


To specify the exact system call ,시스템 호출을 정확히 하기 위해
각 시스템 콜에 번호가 할당됨.OS안의 Trap handler는 이 번호를 검사하고 유효한 경우 해당 코드를 실행한다.

Hardware에 Trap-Table의 위치를 알려주는 명령을 실행할 수 있다.
(Privileged instruction)


### 6.3 Problem #2: Switching Between Processes

####  process가 CPU에서 실행중이라면 OS는 실행중이 아니다.
그렇다면 어떻게 CPU 를 Regain할 수 있을까?

1. Cooperative Approach : Wait For system calls
The OS trusts the processes of the system to behave reasonably.

너무 오래 실행된 프로세스는 주기적으로 CPU를 양보하여 다른 작업을 수행할 수 있도록 한다.
Process가 자발적으로 양도!
system call이나 illegal instruction을 실행하면 OS가 CPU를 얻을 수 있다.(Trap을 통해)

#### full of bugs or malicious process가 inifinite loop에 빠지면 어떻게 될까?
기계재부팅을 해야한다.

2. Non-Cooperative Approach : The OS takes control

일정한 타임 간격으로 OS가 CPU를 얻을 수 있도록 한다.
OS는 timer interrupt가 발생할때까지 어떤 코드를 실행할지 하드웨어에 알려줘야한다.
OS가 강제로 CPU를 얻는다.

OS Regain control - > 실행중인 프로세스를 계속 실행할지 or 다른 프로세스로 변환할지 정해야한다.

Context Switching : OS가 실행중인 프로세스를 중단하고 다른 프로세스를 실행하는 것
1. OS는 현재 실행중인 프로세스의 레지스터값을 저장한다.
2. OS는 다음 실행할 프로세스의 레지스터값을 복원한다.
3. OS는 다음 프로세스를 실행한다.

#### 6.4 Worried about Concurrency?
동시성 주의

system call중에 timer interrupt가 발생하면 어떻게 될까?
interrupt를 처리하는 중에 다른 interrupt가 발생하면 어떻게 될까?

