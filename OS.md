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

--------
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

