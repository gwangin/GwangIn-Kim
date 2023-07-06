# OS

>OS 란?<br>
>컴퓨터 시스템의 자원들을 효율적으로 관리하며, 사용자가 켬퓨터를 편리하고, 효과적으로 사용할 수 있도록 환경을 제공하는 여러 프로그램의 모임.<br>
>컴퓨터 사용자와 컴퓨터 하드웨어 간의 인터페이스로서 동작하는 시스템 소프트웨어의 일종으로, 다른 응용프로그램이 유용한 작업을 할 수 있도록 환경을 제공해준다.

>사용자 <-> 응용프로그램 <-> OS <-> 하드웨어




------
## 4 The Abstraction: The Process
<br>

### 4.1 Introduction<br>
프로세스는 컴퓨터에서 실행 중인 프로그램.<br>
프로그램은 SSD에 저장되어 있지만
프로세스는 메모리에 로드되어 CPU에서 실행됨.<br>
프로레스간의 상호작용을 OS가 담당함.

>#### 프로세스란?
>프로세스란 OS가 실행중인 프로그램을 의미한다.<br>
>OS가 실행하기전까지는 디스크에 상주하는 instruction의 묶음이다.<br>
>스케줄링의 대상이 되는 작업(Task)와 같은 의미이다.<br>
>프로세스 내부에는 최소 하나 이상의 Tread를 가지고 있는데, 이 Tread 단위로 스케줄링을 한다.<br>
>HardDisk에 있는 프로그램을 실행하면, 실행을 위해 메모리 할당이 이루어지고, 할당된 메모리 공간으로 Binary Code가 올라가게 된다.<br>
>이 순간부터 프로세스라고 한다.<br>
>#### 프로세스의 메모리 구조
>1. Code 영역 : 프로그램을 실행시키는 실행 파일 내의 명령어들이 올라간다.
>2. Data 영역 : 전역변수, Static 변수의 할당이 이루어진다.
>3. Heap 영역 : 동적 할당 시 사용되는 메모리영역이다.
>4. Stack 영역 : 함수의 호출과 관계되는 지역변수, 매개변수, 리턴 값 등 Parameter들이 저장되는 영역이다.<br>

프로세스를 이해하기위해서는 프로세스의 Machine state를 이해해야 한다.<br>
#### 프로세스의 Machine state
1. Memory <br>
명령어와 실행 중에 읽고 쓰이는 데이터들이 메모리에 있다.<br>

2. Resister
많은 명령어들이 레지스터를 읽거나 업데이트함.<br>
Machine State의 일부인 특별한 Resister도 있다.
>(Program Counter) : 다음에 실행할 프로그램 명령어를 지시한다.<br>
>(Stack Counter) : Function parameter , local variables가 담긴 Stack을  관리하고 Address를 반환하기 위해 사용된다.

3. Persistent storage devices (영구 저장 장치)<br>
입출력 정보는 프로세스가 현재 열려 있는 파일 목록에 포함.
>영구적으로 데이터가 저장되는 저장소.<br>
>프로세스는 때때로 file open 또는 I/O시에 PSD에 access한다.



### 4.2 Process<br>
#### 운영체제의 Interface에 무조건 포함해야 하는 내용
#### 대표적인 Process API
1. Create : 새로운 프로세스를 생성하는 방법을 제공해야 함.
>운영체제가 새로운 프로세스를 생성하는 메소드. Shell에 명령어를 입력하거나, 어플리캐이션 아이콘을 더블클릭할 때, 운영체제는 사용자가 지시한 프로그램을 실행하기 위한 새로운 프로세스를 생성한다.
2. Destroy : 프로세스 생성을 위한 인터페이스가 있을 때,프로세스를 강제로 종료시키는 인터페이스를 제공.
>운영체제가 프로세스르 강제적으로 종료하도록 하는 메소드이다.
>프로세스가 자발적으로 종료되는 경우도 있지만, 종료되지 않는 경우도 있다. 그때 사용한다.
3. Wait : 프로세스의 실행이 종료될 때까지 기다린다.
4. Miscellaneous Control : 일지정지, 재개
5. Status : 프로세스에 대한 상태정보를 얻는 인터페이스(실행된 시간, 어떤 상태?)


### 4.3 Process Creation<br>
#### Process Creation<br>

>메모리란?<br>
>메모리는 프로그램이 실행되기 위해 필요한 데이터와 명령어들이 저장되는 공간으로 주기억장치이다.<br>
>주기억장치느 RAM , Cache Memory , SSD , Hard Disk등이 있다.<br>
>CPU는 주기억장치에 저장된 데이터와 명령어를 읽어와서 처리한다.<br>
>SSD or Hard Disk -> RAM -> CPU<br>



1. 운영체제가 Program Code와 Static Data를 메모리에 로드한다.<br>
= 운영체제가 디스크에서 해당 바이트를 읽고 메모리의 어딘가에 배치한다.
2. Program Code와 Static Data가 로드되면 프로그램의 실행 시간 스택 (Running Time Stack)을 위한 메모리를 할당한다.
>main()힘수의 매개변수인 argc 와 argv 배열을 채워 넣는다.<br>
>프로그램의 heap을 위해 일부 메모리를 할당할수도 있다.<br>
>Heap은 요청받은 동적 할당 데이터들을 위해 사용된다.<br>
>예를 들어 malloc()으로 할당하고, free()로 할당 해제한다.<br>
3. 초기화 작업 진행 (I/O작업 관련 초기화 작업도 한다.)
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

1. Ready : 프로세스가 실행될 준비가 되어있는 상태
2. Running : 프로세스가 CPU를 사용하고 있는 상태
3. Blocked : 다른 이벤트가 발생할 때까지 실행 준비가 되지 않은 상태.

> Running - > Ready : Scheduled <br>
> Ready - > Running : Deschduled <br>

#### Process가 I/O작업을 시작하면 그 Process가 Block된다.
I/O작업은 일반적으로 시간이 소요되는 작업이며, 외부 장치나 파일 시스템과의 상호 작용이 필요합니다.<br>
그래서 I/O 작업이 끝날때까지 프로세스는 실행될 준비가 되지 않은 상태가 된다.<br>
#### Process가 I/O작업을 마치면 그 Process가 Ready된다.

### 4.5 Data Structures<br>
>운영체제도 프로그램이며, 다른 프로그램들과 마찬가지로 다양한 관련 정보를 추적하는 주요 데이터 구조를 가지고 있다.<br>
>예를 들어 각각 프로세스의 상태를 추적하기 위해 운영체제는 Process List를 가지고 있다.<br>
>이 프로세스 List는 Ready 상태의 모든 프로세르를 포함하며, 어떤 프로세스가 실행되고 있는지를 추적하기 위한 정보들을 포함한다.<br>
>또란 Blocked 상태의 프로세스 또한 추적해야한다. 에를 들어 I/O event가 완료되었을 때 운영체제는 올바른 프로세스를 깨워 다시 실행하기 위해 ready 상태로 정환해야 한다.


#### Process Control Block (PCB)<br>
운영체제는 각 프로세스에 대한 정보를 저장하기 위해 PCB를 사용한다.<br>

-----------
## 5. Interlude: Process API<br>
운영체제가 프로세스의 생성 및 제어를 위해서 제공하는 API다.



#### UNIX systems
fork() : Create a new process<br>


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
#### 결과
```C
hello, I am parent A (pid:12)
child B process ID: 23
```

#### fork() 시스템 호출은 부모 프로세스의 복사본을 생성하여 두 개의 독립적인 프로세스를 만들고, 반환 값을 통해 부모와 자식 프로세스를 구별할 수 있게 합니다.



#### exec() system Call
exec() 시스템 호출은 새로운 프로세스를 실행하는데 사용된다.<br>
새로운 프로세스를 생성하기보다는 현재 실행중안 프로그램을 해당 프로그램으로 변환한다고 보면 된다.


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

이러한 과정을 통해 현재 실행 중인 프로세스를 다른 프로그램으로 변환하고 해당 프로그램을 실행한다. exec()는 새로운 프로세스를 생성하지 않고 현재 실행중인 프로그램을 다른 프로그램으로 교체한다.

fork() , exec()모두 한 프로세스가 다른 프로세스를 실행시키기 위해 사용된다.<br>
exec에는 execl, execv등 다양한 함수군을 가지고 있다.<br>
차이점<br>
fork() 시스템 호출은 새로운 프로세스를 위한 메모리를 할당한다는 것이다.<br>
 fork()를 호출한 프로세스를 새로운 공간으로 전부 복사하게 되고, 원래 프로세스는 원래 프로세스대로 작업을 실행하고 fork()를 이용해서 생성된 프로세스도 fork() 시스템 콜이 수행된 라인의 다음 라인부터 실행된다.<br>

exec() 는 새로운 프로세스를 위한 메모리를 할당하지 않고, exec()를 호출한 프로세스가 아닌 exec()에 의해 호출된 프로세스만 메모리에 남게 된다.<br>
- fork() : 새로운 프로세스를 위한 메모리 할당 O , 프로세스가 하나 더 생김!
- exec() : exec()를 호출한 프로세스가 아닌 exec()에 의해 호출된 프로세스만 메모리에 남게 된다. exec()를 호출한 프로세스의 PID가 새로운 프로세스에 그대로 사용!



- Process API term
1. PID : process의 이름
2. fork() 시스템 호출은 새로운 프로세스를 생성하는 데 사용됩니다. 생성자는 부모(parent)라고 불리고, 새로 생성된 프로세스는 자식(child)이라고 불립니다. 실생활에서 종종 일어나는 것처럼, 자식 프로세스는 부모의 거의 동일한 복사본입니다.
3. wait() 시스템 호출은 부모가 자식 프로세스의 실행이 완료될 때까지 기다릴 수 있게 합니다.
4. exec() 시스템 호출 계열은 자식 프로세스가 부모와의 유사성에서 벗어나 새로운 프로그램을 실행할 수 있게 합니다.
5. UNIX 셸은 일반적으로 fork(), wait(), exec()를 사용하여 사용자 명령을 실행합니다. fork와 exec의 분리로 인해 입력/출력 리디렉션, 파이프 및 다른 멋진 기능을 프로그램을 변경하지 않고도 사용할 수 있습니다.
6. 프로세스 제어는 작업을 일시 정지, 계속 또는 종료시킬 수 있는 신호의 형태로 제공됩니다.
7. 특정 사용자가 제어할 수 있는 프로세스는 사용자의 개념에 캡슐화되어 있습니다. 운영 체제는 여러 사용자가 시스템에 로그인할 수 있도록 허용하고, 사용자는 자신의 프로세스만 제어할 수 있도록 합니다.
8. 슈퍼유저는 모든 프로세스를 제어할 수 있으며(그리고 실제로 많은 다른 작업도 수행할 수 있습니다), 보안상의 이유로 드물고 신중하게 이 역할을 맡아야 합니다.


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

--------------------
## 6. Mechanism: Limited Direct Execution

>CPU Virtualization 이란?<br>
>하나의 CPU를 여러 개의 CPU처럼 사용하는 것을 말한다. <br>
>하나의 프로세스를 잠시 실행했다가 다른 프로세스를 실행하는 방식으로 진행한다.<br>




### 6.1 Basic Technique: Limited Direct Execution (LDE)
- Limited Direct Execution
CPU에서 프로그램을 직접 실행.<br>
프로세스 목록에서 행당 프로그램의 Tree를 만들고, 메모리 할당하고 , 프로그램 코드를 디스크에서 메모리로 Load 하고, main()함수를 찾아서 실행시키고, 프로그램이 끝나면 메모리를 해제하고, 프로세스 목록에서 제거한다.<br> 

#### 과정
OS<br>
Create entry for process list <br>
Allocate memory for program <br>
Load program into memory <br>
Set up stack with argc/argv <br>
Clear registers<br>
Execute call main() Free memory of process<br>
Remove from process list<br>


Program<br>
Run main()<br>
Execute return from main<br>


OS<br>
Free memory of process<br>
Remove from process list<br>


f
하지만 이러한 방법은 다음과 같은 문제가 발생한다.<br>
1. 비효율적이다. 원하지 않는 동작을 수행할 수 있다.
2. 프로세스를 실행하는 동안, OS가 그것을 중지하고 다른 프로세스를 전환하여 CPU를 가상화하는 Time Sharing을 어떻게 구현할까? 이다.<br>

이러한 문제들을 해결하기위해 프로그램을 제한한다. <br>


###  6.2 Problem #1: Restricted Operations
- 프로그램이 CPU에서 실행중에 프로그램이 디스크에 I/O요청을 보내거나 CPU나 메모리와 같은 시스템 지원에 접근하는 등의 제한된 작업을 수행하려는 경우<br>

- Restricted operation에 대해서는 프로세스가 원하는 것을 모두 허용하는 것.<br>
이렇게 한다면 프로세스가 디스크를 읽거나 쓰는 , 모든 보호기능이 상실될 수 있다.

따라서 User Mode 라는 프로세서 모드를 도입
- User Mode : 프로세스가 할 수 있는 작업이 제한됨<br>
예를 들어 User mode에서 실행 중인 프로세스느 I/O 요청을 보낼 수 없다.<br>

이와 대조적으로 Kernel Mode 라는 모드를 도입
- Kernel Mode : 프로세스가 모든 작업을 수행할 수 있도록 하는 모드
실행되는 코드는 I/O요청을 포함한 권한이 있는 작업을 수행할 수 있다.


- user process가 디스크에서 읽기와 같은 privileged operation을 수행하려고 하면, user program이 system call을 수행할 수 있다. System Call 을 하면 커널이 file system에 access하고, 프로세스를 생성 및 파괴하고, 다른 프로세스와 통신하고, 더 많은 메모리를 할당하는 것과 같은 특정한 핵심 기능들을 사용자 프로그램에 노출한다.

system call을 수행하려면 trap 명령어를 실행해야함.

> trap instruction(명령어) : trap 명령어는 프로세스가 현재 실행 중인 명령 흐름을 일시적으로 중단하고, 운영 체제 또는 커널 모드로 전환하여 privileged operation을 수행할 수 있게 해주는 명령어이다.

커널로 점프하고  privilege level to kernel mode 권한 수준을 커널 모드로 올림<br>이렇게 되면 privileged operation을 수행할 수 있게 되고 작업이 끝나면 OS는 return-from-trap instruction을 호출한다.<br>
호출한 user program을 반환하면서 privilege level을 user mode로 다시 낮춘다.


#### User Mode  와 Kernel Mode
>유저모드 (User Mode)와 커널모드 (Kernel mode)
>커널에서 중요한 자원을 관리하기 때문에, 사용자가 그 중요한 자원에 접근하지 못하도록 모드를 2가지로 나눈 것이다.

>- User Mode
>유저(사용자)가 접근할 수 있는 영역을 제한적으로 두고, 프로그램의 자원에 함부로 침범하지 못하는 모드이다.
>우리는 여기서 코드를 작성하고, 프로세스를 실행하는 등의 행동을 할 수 있다.
>간단하게 "유저 어플리케이션 코드가 유저모드에서 실행된다." 라고 말할 수 있다.

>- Kernel Mode
>모든 자원(드라이버, 메모리, CPU 등)에 접근, 명령을 할 수 있다.
>유저모드와는 비교가 안되게 컴퓨터 내부에서 모든 짓(?)을 할 수 있다고 생각하면 된다.


To specify the exact system call ,시스템 호출을 정확히 하기 위해
각 시스템 콜에 번호가 할당됨.OS안의 Trap handler는 이 번호를 검사하고 유효한 경우 해당 코드를 실행한다.

Hardware에 Trap-Table의 위치를 알려주는 명령을 실행할 수 있다.
(Privileged instruction)


> 1. (부팅 시) 커널은 Trap Table을 초기화하며 CPU느 ㄴ이후 사용하기 위해 위치를 기억하낟. 이러한 과정은 Privileged Instruction을 통해 이루어진다.
> 2. (프로세스 실행 시) 커널은 프로세스를 실행하기 전 몇가지 사항을 설정하고 , CPU를 사용자 모드로 전환하여 프로세스를 실행하기 시작한ㄷ 프로세스가 시스템 호출을 하면 운영체제는 Trap에 걸려 이를 처리하고 return-from-trap 명령을 통해 프로세스 제어 권한을 반환한다. 그 후 프로세스가 작업을 완료하면 main()에서 return()된다. 이를 통해 프로그램이 정상적으로 exit()되고, 운영체제는 프로세스를 종료한다. 


### 6.3 Problem #2: Switching Between Processes

####  process가 CPU에서 실행중이라면 OS는 실행중이 아니다.
그렇다면 어떻게 CPU 를 Regain할 수 있을까?<br>
> 프로세스를 수행하는 주체는 CPU이지 OS가 아니다.<br>
> 그렇기에 Switch Between Process를 수행하기 위해 CPU에 대한 제어권, 즉 Regain control을 얻어야한다.

1. Cooperative Approach : Wait For system calls
The OS trusts the processes of the system to behave reasonably.

너무 오래 실행된 프로세스는 주기적으로 CPU를 양보하여 다른 작업을 수행할 수 있도록 한다.<br>
즉, Process가 자발적으로 양도!<br>
system call이나 illegal instruction을 실행하면 OS가 CPU를 얻을 수 있다.(Trap을 통해)<br>

하지만 full of bugs or malicious process가 inifinite loop에 빠질 수 있다는 문제점이 발생한다.

이러한 문제점이 발생했을 때 유일한 방법은 기계재부팅이다.

2. Non-Cooperative Approach : The OS takes control

일정한 타임 간격으로 OS가 CPU를 얻을 수 있도록 한다.<br>
-> Timer 장치는 milliseconds 단위로 interrupt를 발생시킨다.<br>
Timer interrupt가 발생하면 OS는 CPU를 얻을 수 있다.<br>
OS는 timer interrupt가 발생할때까지 어떤 코드를 실행할지 하드웨어에 알려줘야한다.<br>
즉, OS가 강제로 CPU를 얻는다.

OS Regain control - > 실행중인 프로세스를 계속 실행할지 or 다른 프로세스로 변환할지 정해야한다.

- Context Switching : OS가 실행중인 프로세스를 중단하고 다른 프로세스를 실행하는 것
1. OS는 현재 실행중인 프로세스의 레지스터값을 저장한다.
2. OS는 다음 실행할 프로세스의 레지스터값을 복원한다.
3. OS는 다음 프로세스를 실행한다.

>레지스터란?<br>
>CPU 내에 있는 고속 메모리 유닛<br>
>CPU가 명령어를 실행하고 데이터를 처리하는데 사용되는 저장 공간으로, 매우 빠른 Acess 속도를 가지고 있다.
>1. 데이터 레지스터(Data Register): 데이터를 저장하고 처리하는 데 사용됩니다. 산술 연산이나 논리 연산, 데이터 이동 등에 필요한 데이터를 임시로 저장합니다.
>2. 주소 레지스터(Address Register): 주기억장치의 주소를 저장하는 데 사용됩니다. 메모리 주소를 지정하거나, 명령어나 데이터를 읽어오거나 쓸 때 사용됩니다.
>3. 명령어 레지스터(Instruction Register): 현재 실행 중인 명령어를 저장하는 데 사용됩니다. CPU는 명령어 레지스터에서 현재 실행 중인 명령어를 읽어와 해당 동작을 수행합니다.
>4. 상태 레지스터(Status Register): CPU의 상태 정보를 저장하는 데 사용됩니다. 예를 들어, 조건부 분기나 예외 처리 등에서 사용되며, 프로그램 상태, 오류 플래그 등을 저장합니다.



#### 6.4 Worried about Concurrency?
동시성 주의

system call중에 timer interrupt가 발생하면 어떻게 될까?
interrupt를 처리하는 중에 다른 interrupt가 발생하면 어떻게 될까?




-----------
## 7 Scheduling: Introduction

- 스케줄링 정책에 대한 기본적인 프레임워크를 구출할 때 고려해야할 사항

1. 프로세스들이 서로 독립적인가? 의존성이 있는가? 우선순위가 있는가?<br>
2. 어떤 기준이 중요한가? round-robin, CPU 사용량 , 응답 시간, 처리량, 우선순위<br>
3. 초기 컴퓨터의 기본 접근 방식 : 기존의 기본적인 스케줄링 정책 사용

### 7.1 Workload Assumptions
Job이라고 불리는 프로세스들의 가정
1. 각 작업은 동일한 시간 동안 실행됩니다.
2. 모든 작업들은 동시에 도착합니다.
3. 시작되면 각 작업은 완료될 때까지 계속 실행됩니다.
4. 모든 작업들은 CPU만 사용합니다. (입출력을 수행하지 않습니다.)
5. 각 작업의 실행 시간은 알려져 있습니다.

이 가정들은 비현실적이다.

### 7.2 Scheduling Metrics
Workload Assumtion 이외에도, 다른 스케줄링 정책을 비교하기 위해 필요하다.

> Metric이란?<br>
> 시간이 지남에 따라 보고된 숫자 측정값<br>


일단 간단하게 , Turnaound time 만 사용하도록 하자.<br>
T_turnaround = T_completion - T_arrival<br>
작업이 완료된 시간에서 작업이 시스템에 도착한 시간을 뺀 값으로 정의.

모든 작업이 동시에 도착한다고 가정했기 때문에, 현재 Trarrival = 0이고 따라서 T_turnaround = T_completion 이다.


### 7.3 First In, First Out (FIFO) Scheduling (Alos known as First come, First Served)

먼저 도착한 작업을 먼저 실행하는 방식<br>
하지만 A B C 순으로 작업이 들어왔다고 가정할 때<br>
A가 매우 긴시간동안 작동한다면 B C는 짧은 작업임에도 불구하고 오랫동안 기다려야한다. = Convey effect<br>
즉 세 작업의 Turnaround Time은 길어진다.


### 7.4 Shortest Job First (SJF) Scheduling
가장 짧은  작업을 먼저 실행하는 방식<br>
SJF는 평균 회전 시간 측면에서 성능이 좋음.<br>

하지만 이것은 작업이 모두 동시에 도착한다는 가정하에 성립한다.<br>
실행시간이 긴 A가 시작되기전에, A보다 짧은 작업이 계속해서 도착한다면, A는 계속해서 밀릴것이기 때문.<br>

### 7.5 Shortest Time-to-Completion First (STCF) Scheduling
시작되면 각 작업이 완료될 때까지 계속 실행된다는 가정을 완화하자.<br>
SJF는 비선점 스케줄러이므로 A를 실행하다가 멈추고 B, C를 완료한 후에 다시 A를 실행시키면 문제가 발생한다.<br>
이를 해결하기 위해 STCF는 선점 스케줄러이다.<br>

> Context Switching이란?<br>
> 현재 실행중인 프로세스 또는 Tread의 상태를 저장하고, 다음에 실행할 프로세스의 상태를 복원하여 CPU의 실행을 전환하는 과정을 말한다.<br>
> 컨텍스트란 프로세스의 실행에 필요한 모든 정보를 포함하는 상태.<br>


### 7.6 A New Metric: Response Time
STCF는 job length를 알고 있고 오직 CPU만 사용하는 경우, 유일한 matric(지표)가 turnaround time인 경우 훌륭한 policy이다.<br>

하지만 Time-sharing system이 등장하면서 user는 더이상 터미널에 앉아 시스템에서 interactive하게 작업을 수행하지 않는다.<br>

응답시간이란, 작업이 처음으로 CPU를 얻어서 결과를 내기까지 걸리는 시간을 말한다.<br>

T_response = T_first - T_arrival<br>

> 응답시간이 좋아야 하는 이유는 사용자의 상호 작용성과 시스템의 대화형 성능에 직접적인 영향을 미치기 때문.<br>
> 사용자가 시스템과 상호 작용하는 경우, 실시간으로 결과를 확인하고 응답을 받는 것이 중요하다.<br>
> 따라서 응답시간이 빠를수록 사용자는 빠른 응답을 통해 실시간으로 시스템의 상태를 확인하고 작업을 진행할 수 있다.


### 7.7 Round Robin (RR) Scheduling
작업을 완료하는 대신에 작업을 Time slice라고 불리는 작은 단위로 나누어서 실행하는 방식.<br>
타임 슬라이스의 길이는 타이머 인터럽트 주기의 배수이다.<br>
타입 슬라이스가 짧을수록 Response time이 빨라지지만, 너무 작으면 Context Switching이 너무 자주 발생하여 오버헤드가 발생한다.<br>

Round Robin 과 같이 작은 시간 단위에서 CPU를 Active Process 사이에 공정하게 분배하는 Policy는 Turnaround time관점에서는 성능이 좋지 않을 것이다.<br>

첫 번째 유형 SJF, STCF : 회전 시간 Good - 응답 시간 Bad.<br>
두 번째 유형 RR : 응답 시간을 Good -  회전 시간 Bad <br>

### 7.8 Incorporating I/O
4. 모든 작업들은 CPU만 사용합니다. (입출력을 수행하지 않습니다.)
를 제거해보자.
작업이 I/O 요청을 하면 현재 실행중인 해당 작업은 I/O 도중 CPU를 사용하지 않을 것이며 I/O가 완료되기 전까지 Blocked 상태가 될 것이다.<br>

I/O가 완료되면 Interrupt가 발생하여 CPU가 다른 작업을 수행하게 되고, I/O가 완료되면 Ready Queue에 들어가게 된다.<br>


그렇다면 마지막 가정인 각 작업의 길이를 스케줄러가 알고 있다는 가정을 제거해보자.

------------------
## 13. The Abstraction: Address Spaces

### 13.2 Multiprogramming and Time Sharing
>Batch computing이란(일괄처리방식)?<br>
>각 프로세스가 순차적으로 실행되며, 하나의 프로세스가 실행을 완료한 후 다음 프로세스가 실행된다.<br>
>프로세스간 전환 시에는 현재 프로세스 상태를 디스크에 저장하고, 다음 프로세스의 상태를 로드하는 방식이다.<br>
>이 방식은 메모리가 커질수록 디스크 I/O가 빈번하게 발생하므로, 시간이 오래 걸리는 성능저하 단점이 있다.<br>

초기 컴퓨터에는 이러한 방식이 문제가 없었지만, 메모리가 커지고 프로세스가 많아지면서 디스크 I/O가 빈번하게 발생하게 되었다.<br>

레지스터 수준의 상태(PC , General-purpose-register)를 저장하고 복원하는 것은 빠르지만, 메모리 전체 내용을 디스크에 저장하는 것은 성능이 매우 저하된다.<br>
따라서 프로세스를 메모리에 남겨두고 그들 사이를 전환하여 OS가 효율적으로 Time-Sharing을 할 수 있도록 해야한다.<br>

>최근사용한 프로세스를 디스크에 저장한다면 , 다시 프로세스를 실행할 때 디스크 I/O가 발생하므로 성능이 저하된다.<br>
>즉, 디스크가 아니라 주기억장치인 RAM , Cache Memory에 저장해야한다.<br>
>Disk, SSD는 보조기억장치이다. 대용량의 저장공간을 제공하지만, 접근속도가 느리다.<br>

### 13.3 The Address Space

OS는 물리적 메모리의 사용을 위한 abstraction을 생성한다.<br>
이 추상화를 Address Space라고 하며, 실행 중인 프로그램이 시스템의 메모리를 볼 때의 개념이다.<br>

프로세스의 주소 공간에는 실행 중인 프로그램의 모든 메모리 상태가 포함된다.<br>
예를 들어, 프로그램의 코드는 주소 공간에 있다. 프로그램이 실행되는 동안 함수 호출 체인의 위치를 추적하고 지역 변수를 할당하며, 루틴에 매개변수를 전달하고 반환 값을 받기 위해 Stack을 사용한다.<br>
프로그램은 Heap을 사용하여 동적으로 할당된 메모리를 관리한다.<br>

Heap은 메모리의 위쪽에 저장하고 Stack은 메모리의 아래쪽에 저장한다.<br>
이렇게 주소 공잔의 양 끝에 배치함으로써 Heap과 Stack이 서로  커지면서 충돌하는 것을 방지한다.<br>


### 13.4 Goals

1. Virtual Memory System의 주요 목표 중 하나는 transparency(투명성)이다.<br>

OS는 실행 중인 프로그램에게 보이지 않는 방식으로 가상 메모리를 구현해야 한다.<br>
따라서 프로그램은 메모리가 가상화된 사실을 인식하지 않아야하며, 대신 프로그램은 자체 개인적인 물리적 메모리를 갖고 있는 것처럼 동작해야한다.<br>

2. Virtual Memory System의 두 번째 목표는 efficiency(효율성)이다.<br>

느리지 않게 가능한 메모리는 적게 사용하도록 해야한다.<br>

3. Virtual Memory System의 세 번째 목표는 protection(보호)이다.<br>

프로세스 간에 서로를 보호하고 프로세스로부터 OS 자체를 보호해야한다.<br>


-----------
## 14 Interlude : Memory API

### 14.1 Types of Memory
C프로그램을 실행할 때에, 두 가지 유형의 메모리가 할당된다.<br>

1. Stack Memory<br>
할당과 해제는 컴파일러에 의해 관리된다.<br>
다음과 같이 간단하게 선언할 수 있다.<br>


```C
void func() {
  int x; // 스택에 정수형 변수 선언
  ...
}

```
컴파일러는 나머지 작업을 처리하여 func()을 호출할 때 Stack에 공간을 할당합니다.<br>
함수에서 return하면 컴파일러가 메모리를 해제한다.<br>

그래서 long-lived memory 가 필요하다면 Heap Memory를 사용한다.<br>

2. Heap Memory<br>
Heap Memory는 프로그래머가 직접 관리해야한다.<br>
다음과 같이 선언한다.<br>

```C
void func() {
  int *x = (int *) malloc(sizeof(int));
  ...
}
```
Stack과 Heap 할당이 모두 발생하며 컴파일러는 포인터 (int *x)를 선언할 때 정수를 가리키기 위한 공간을 할당해야 함을 인식합니다.<br>
그런 다음 프로그램이 malloc()을 호출할 때  heap에 정수를 위한 공간을 요청하며, 이 함수는 해당 정수의 주소(성공)나 NULL(실패)를 반환한다.<br>

### 14.2 The malloc() Call

heap에 일정한 공간을 요청하는 size를 전달하면 성공 시 새롭게 할당된 공간을 가리키는 포인터를 반환하고 실패 시 NULL을 반환한다.<br>

```C
#include <stdlib.h>
...
void *malloc(size_t size);
double *d = (double *) malloc(sizeof(double));

```

### 14.3 The free() Call
사용하지 않는 heap memory는 free()를 통해 해제한다.

```C
int *x = malloc(10 * sizeof(int));
...
free(x);
```

### 14.4 Common Errors

1.  메모리 할당을 잊어버리는 경우<br>
잘못된 경우 : <br>

```C
char *src = "hello";
char *dst;        // 잘못됨! 할당되지 않음
strcpy(dst, src); // 세그멘테이션 폴트(메모리 접근 오류) 발생
```

 
이 코드를 실행하면 Segmentation Fault가 발생할 가능성이 높다.<br>
>Segmentation Fault는 프로그램이 잘못된 메모리에 접근할 때 발생한다.<br>


올바른 경우 : <br>

```C
char *src = "hello";
char *dst = (char *) malloc(strlen(src) + 1);
strcpy(dst, src); // 올바르게 동작
```

>문자열은 마지막에 \0 즉 null 문자를 만날 때까지 문자열로 인식한다.<br>
>strlen(src)는 null문자를 만날 때까지 문자열을 순회하며 길이를 측정한다. 즉 null문자를 포함하지 않은 문자열의 길이를 반환한다.<br>

2. 충분한 메모리를 할당하지 않는 경우 Buffer Overflow: <br>

```C
char *src = "hello";
char *dst = (char *) malloc(strlen(src)); // 너무 작음!
strcpy(dst, src); // 올바르게 동작
```
이 프로그램은 보통 정상적으로 실행될 수 있다.<br>
일부 경우에는 문자열 복사가 실행될 때 할당된 공간의 끝을 한 바이트 넘게 쓰지만, 이는 경우에 따라서는 해롭지 않을 수 있다.<br>

3. 할당된 메모리를 초기화하지 않는 실수<br>
malloc()을 올바르게 호출한 이후, 새로 할당된 데이터 형식에 값을 채우지 않았을 때 발생한다.<br>
Heap에서 알 수 없는 값의 데이터를 읽기 때문에 오류가 발생할 수 있다.<br>

4. 메모리 해제를 잊는 실수<br>
장기간 실행되는 응용 프로그램이나 시스템에서 큰 문제가 발생한다.<br>
메모리가 점차 부족해지고 재시작이 필요하게 된다.<br>

5. 사용이 끝나지 않은 메모리를 해제하기<br>
이러한 실수를 Dangling Pointer라고 한다.<br>
이후 사용하는 부분에서 프로그램이 충돌하거나 유효한 메모리를 덮어쓸 수 있다.<br>
예를 들어, free()를 호출한 후 malloc()을 다시 호출하여 잘못 해제된 메모리를 재사용하는 경우.<br>

6. 메모리를 중복해서 해제하기<br>
메모리를 두번 이상 해제 double free라고 한다.<br>
이를 수행하는 결과는 정의되지 않는다.<br>
메모리 할당 라이브러리가 혼란스러워지고 이상한 동작을 수행할 수 있다.<br>

7. free()를 잘못 호출하기<br>
free()는 malloc()으로 할당된 메모리만 해제할 수 있다.<br>
다른 값을 전달하면 오류를 발생시킬 수 있다.<br>

### 14.5 Underlying OS Support
malloc()은 시스템 호출이 아니라 라이브러리 호출이다.<br>
따라서 malloc 라이브러리는 가상 주소 공간 내에서 공간을 관리하지만, 내부적으로는 시스템 호출을 사용하여 추가 메모리를 요청하거나 시스템에 메모리를 반환한다.<br>

>System Call이란?<br>
>운영체제 커널에 직접적으로 서비스를 요청하는 인터페이스이다.<br>
>운영체제 커널은 하드웨어 자원 및 기능에 접근하고 프로세스 간의 상호작용을 관리하는 부분이다.<br>

>Library Call이란?<br>
>프로그래밍 언어에서 제공하는 함수 또는 라이브러리 함수를 호출하여 특정 작업을 수행하는 것이다.<br>
>라이브러리 호출은 일반적으로 운영체제의 기능에 접근하는데 사용되는 시스템 호출과 달리, 프로그래밍 언어 자체에서 제공하는 기능을 활용한다.<br>

>시스템 호출은 운영 체제의 기능을 직접 사용하여 프로세스가 하드웨어 자원 및 운영 체제 기능에 접근할 수 있게 해주는 인터페이스이고, 라이브러리 호출은 프로그래밍 언어에서 제공하는 함수나 라이브러리 함수를 호출하여 특정 작업을 수행하는 인터페이스이다.

------------------------------------------------
## 15. Mechanism: Address Translation
CPU를 virtualization 가상화하는 과정에서 LDE(limited direct execution)이라고 하는 일반적인 메커니즘에 초점을 맞췄다.<br>
LDE는 대부분의 경우는 프로그램을 하드웨어 위에서 직접 실행하되, 특정한 중요한 시점에 운영 체제가 개입하여 올바른 동작이 이루어지도록 합니다.<br>
따라서, OS는 약간의 하드웨어 지원을 통해 실행 중인 프로그램을 가장 효율적으로 가상화하기 위해 최선을 다하지만, 중요한 시점에서 개입하여 하드웨어 제어권을 유지한다.<br>

즉, 효율성과 제어권은 OS의 주요 목표이다.<br>

- 효율성을 위해서는 하드웨어를 사용해야한다.
- 제어권은 응용 프로그램이 자신의 메모리 외의 어떤 메모리에도 접근하지 못하도록 운영체제가 보장해야 함을 의미한다.<br>

Hardware-based address translation (하드웨어 기반의 주소변환):<br>
Address Translation은 하드웨어가 각 메모리 접근(명령어 가쟈오기, Load , Store)마다 수행되며, 명령어에서 제공된 가상 주소를 실제로 정보가 위치한 물리 주소로 변환한다.<br>
따라서 모든 Memoey Reference에서 하드웨어에 의해 주소 변환이 수행되어 응용 프로그램의 메모리 참조를 실제 메모리 위치로 재지정한다.<br>
물론, 하드웨어만으로는 메모리를 가상화할 수 없다. 하드웨어는 메모리를 효율적으로 가상화하기 위한 저수준 메커니즘을 제공할 뿐이다.<br>
즉 프로그램이 자체적인 개인 메모리를 가지고 있는 것처럼 보이는 것이다.<br>


### 15.1 Assumtion

일단 지금은 사용자의 주소 공간이 물리 메모리에 연속적으로 배치되어야 한다고 가정한다.<br>
또한 간단함을 위해 주소 공간의 크기가 너무 크지 않다고 가정한다.구체적으로는 주소 공간의 크기가 물리 메모리의 크기보다 작아야한다는 것이다.<br>


### 15.3 Dynamic (Hardware-based) Relocation

- Base and Bounds , Dynamic relocation<br>

CPU 내에 2개의 하드웨어 레지스터가 필요하다. 
1. Base Register : 가상 주소 공간의 시작 주소를 저장한다.
2. Bounds Register : 가상 주소 공간의 크기를 저장한다.

이 Register 쌍은 주소 공산을 물리 메모리의 원하는 위치에 배치할 수 있게 해주며, 동시에 프로세스가 자신의 주소 공간에만 접근할 수 있도록 보장한다.<br>

이러한 설정에서 각 프로그램은 주소 0에서 Load된 것처럼 작성되고 Compile 된다.<br>

#### physical address = virtual address + base 
프로세스에 의해 생성된 모든 Memory Reference는 가상 주소이다.<br>
하드웨어는 이 가상 주소에 Base Register의 내용을 더하고 그 결과를 메모리 시스템에 제공할 수 있는 물리적 주소로 변환한다.<br>

가상 주소를 물리적 주소로 변환하는 것이 바로 Address Translation이다.<br> 즉, 프로세스가 참조하는 것으로 생각하는 가상 주소르르 실제 데이터가 위치한 물리적 주소로 변환하는 것이다.<br>

### 15.4 Hardware Support: A Summary
OS는 커널모드 에서 실행되고, 응용 프로그램은 사용자 모드에서 실행된다.<br>

하드웨어는 Base Register 와 Bounds Register를 제공한다. 따라서 각 CPU에는 메모리 관리 유닛(MMU)의 일부인 추가적인 레지스터 쌍이 있다.<br>
사용자 프로그램이 실행되는 동안, 하드웨어는 사용자 프로그램이 생성한 가상 주소에 베이스 값을 더해 각 주소를 변환한다.또한 주소가 유효한지 확인할 수 있어야 하며, 이는 CPU 내의 Bound Register와 circuitry에 의해 수행된다.<br>

하드웨어는 또한 OS가 프로세스를 실행할 때 베이스와 바운드 레지스터를 변경할 수 있도록 특별한 명령어를 제공한다. 이러한 명령어는 커널 모드에서만 실행될 수 있으며, 이는 OS가 메모리를 관리할 수 있도록 한다.<br>

CPU는 사용자 프로그램이 메모리에 접근을 시도할 때 ( 범위를 벗어난 주소로 ) 불법적으로 메모리에 접근하려는 겅우 예외를 발생시켜야 한다.<br>

### 15.5 Operating System Issues
하드웨어의 지원과 OS 관리의 조합으로 간단한 가상 메모리의 구현이 이루어진다.<br>
이때 Base and Bound register 버전 가상 메모리를 구현하기 위해 OS가 개임해야하는 몇 가지 중요한 사항이 있다.<br>

1. 프로세스가 생성될 때 OS는 해당 프로세스의 주소 공간을 메모리에서 찾아야한다.

2. 프로세스가 종료될 때 OS는 해당 프로세스의 모든 메모리를 다른 프로세스나 OS에서 사용하기 위해 회수해야한다.

3. Context Switching이 발생할 때 OS는 몇 가지 추가 작업을 수행해야한다.

4. OS는 앞에서 설명한 대로 예외 처리기 또는 호출될 함수를 제공해야한다.

>Context Switching : 스케줄러가 실행 중인 프로세스나 스레드의 실행을 일시 중단하고 다른 프로세스나 스레드로 전환하기 위해 현재 상태를 저장하고 복구하는 과정이다.
