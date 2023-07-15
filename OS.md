# OS

>OS 란?<br>
>컴퓨터 시스템의 자원들을 효율적으로 관리하며, 사용자가 켬퓨터를 편리하고, 효과적으로 사용할 수 있도록 환경을 제공하는 여러 프로그램의 모임.<br>
>컴퓨터 사용자와 컴퓨터 하드웨어 간의 인터페이스로서 동작하는 시스템 소프트웨어의 일종으로, 다른 응용프로그램이 유용한 작업을 할 수 있도록 환경을 제공해준다.

>사용자 <-> 응용프로그램 <-> OS <-> 하드웨어




------
## 4 The Abstraction: The Process
<br>

### 4.1 Introduction<br>
프로세스는 실행중인 프로그램을 말한다. 프로그램은 보조기억장치(SSD, HD)에 저장되어 있고, 이를 주기억장치(RAM)으로 LOAD하여 CPU에서 실행된다.<br>
이 프로세스간의 상호작용을 운영체제가 담당한다.<br>

>#### 프로세스란?
>프로세스란 OS가 실행중인 프로그램을 의미한다.<br>
>HardDisk에 있는 프로그램을 실행하면, 실행을 위해 메모리 할당이 이루어지고, 할당된 메모리 공간으로 Binary Code가 올라가게 된다.<br>

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

3. Persistent storage devices (영구 저장 장치, 보조기억장치를 설명하는 듯)<br>
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
>SSD or Hard Disk -> RAM(주 기억장치) -> CPU<br>



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

>heap은 동적으로 할당되는 메모리 영역을 가리킴.Stack과 달리 프로그램에서 필요에 따라 동적으로 메모리를 할당하고 해제할 수 있다.<br>

### 4.4 Process States<br>

#### Process States<br>

1. Ready : 프로세스가 실행될 준비가 되어있는 상태
2. Running : 프로세스가 CPU를 사용하고 있는 상태
3. Blocked : 다른 이벤트가 발생할 때까지 실행 준비가 되지 않은 상태.



#### Process가 I/O작업을 시작하면 그 Process가 Block된다.
I/O작업은 일반적으로 시간이 소요되는 작업이며, 외부 장치나 파일 시스템과의 상호 작용이 필요합니다.<br>
그래서 I/O 작업이 끝날때까지 프로세스는 실행될 준비가 되지 않은 상태가 된다.<br>
#### Process가 I/O작업을 마치면 그 Process가 Ready된다.

### 4.5 Data Structures<br>
>운영체제도 프로그램이며, 다른 프로그램들과 마찬가지로 다양한 관련 정보를 추적하는 주요 데이터 구조를 가지고 있다.<br>
>예를 들어 각각 프로세스의 상태를 추적하기 위해 운영체제는 Process List를 가지고 있다.<br>
>이 프로세스 List는 Ready 상태의 모든 프로세스를 포함하며, 어떤 프로세스가 실행되고 있는지를 추적하기 위한 정보들을 포함한다.<br>
>또란 Blocked 상태의 프로세스 또한 추적해야한다. 에를 들어 I/O event가 완료되었을 때 운영체제는 올바른 프로세스를 깨워 다시 실행하기 위해 ready 상태로 전환해야 한다.


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

-----
## 6. Mechanism: Limited Direct Execution



여러 개의 프로그램을 동시에 작동하기 위해 CPU의 가상화가 필요하다.<br>

메모리 가상화는 각 프로세스에게 독립적인 가상 주소 공간을 제공하고, 운영체제가 가상 주소를 실제 물리 주소로 매핑하여 메모리를 효율적으로 관리한다.<br>
이를 통해 여러 프로세스가 동시에 실행될 수 있고, 각 프로세스는 자신만의 가상 주소 공간을 갖게 되어 메모리 충돌을 피하고 격리된 환경을 유지할 수 있다.<br>







### 6.1 Basic Technique: Limited Direct Execution (LDE)
- Direct Execution Protocol : <br>
프로세스가 실행되면 종료될 때까지 멈추지 않는다. <br>
이 방법은 프로세스를 제어할 수 없으며, 프로세스가 종료되기 전까지 다른 프로세스를 실행할 수 없다.<br>

프로세스를 실행 중에 I/O요청이나 CPU,Memory에서 더 많은 리소스를 요청하는 경우를 해결할 수 없다.<br>
이를 해결하기 위해 LDE를 사용한다.<br> 

###  6.2 Problem #1: Restricted Operations
- 프로그램이 CPU에서 실행중에 프로그램이 디스크에 I/O요청을 보내거나 CPU나 메모리와 같은 시스템 지원에 접근하는 등의 제한된 작업을 수행하려는 경우<br>

#### Restricted operation에 대해서는 프로세스가 원하는 것을 모두 허용하는 것.<br>
이렇게 한다면 프로세스가 디스크를 읽거나 쓰는 , 모든 보호기능이 상실될 수 있으므로 위험하다.<br>

#### 따라서 User Mode , Kernel Mode 로 상태를 구분한다.<br>
- User Mode : 프로세스가 할 수 있는 작업이 제한됨<br>
예를 들어 User mode에서 실행 중인 프로세스느 I/O 요청을 보낼 수 없다.<br>

- Kernel Mode : 프로세스가 모든 작업을 수행할 수 있도록 하는 모드
실행되는 코드는 I/O요청을 포함한 권한이 있는 작업을 수행할 수 있다.

#### System Call :
User  mode의 프로세스가ㅏ Kernel mode에서 수행 가능한 작업을 원할 때는 system call을 사용한다.<br>
System Call이 실행되면 Trap 명령으로 kernel mode로 권한을 바꿔 작업을 수행하고 수행 후에 OS가 return-from-trap 명령으로 user mode 로 돌아온다.<br>

> trap instruction(명령어) : trap 명령어는 프로세스가 현재 실행 중인 명령 흐름을 일시적으로 중단하고, 운영 체제 또는 커널 모드로 전환하여 privileged operation을 수행할 수 있게 해주는 명령어이다.

#### Trap table :
OS는 Trap table을 통해 Trap 명령어를 사용한다.<br>
Trap Table은 주로 kernel mode에서 사용된다.<br>
1. 부팅 시 커널은 Trap Table을 초기화하며 각 Trap Number에 해당하는 Trap Handler를 설정한다.<br>
2. 프로세스 실행 시 Trap 명령어가 실행되면 Trap Number를 참조하여 Trap Handler를 실행한다.<br>


### 6.3 Problem #2: Switching Between Processes

####  process가 CPU에서 실행중이라면 OS는 실행중이 아니다.
그렇다면 어떻게 CPU 를 Regain할 수 있을까?<br>
> 프로세스를 수행하는 주체는 CPU이지 OS가 아니다.<br>
> 그렇기에 Switch Between Process를 수행하기 위해 CPU에 대한 제어권, 즉 Regain control을 얻어야한다.

Time sharing 기법을 사용하여 여러 프로세스를 진행할 때, OS는 CPU를 얻기 위해 두 가지 방법을 사용한다.<br>

1. Cooperative Approach : Wait For system calls  서로 협력하는 방법으로 system call 사용

너무 오래 실행된 프로세스는 주기적으로 CPU를 양보하여 다른 작업을 수행할 수 있도록 한다.<br>
즉, Process가 자발적으로 양도!<br>
system call이나 illegal instruction을 실행하면 Trap을 통해 OS가 CPU를 얻을 수 있다.<br>

하지만 full of bugs or malicious process가 inifinite loop에 빠질 수 있다는 문제점이 발생한다.

이러한 문제점이 발생했을 때 유일한 방법은 기계재부팅이다.

2. Non-Cooperative Approach : The OS takes control : Time interrupt 를 사용

일정한 타임 간격으로 OS가 CPU를 얻을 수 있도록 한다.<br>
-> Timer 장치는 milliseconds 단위로 interrupt를 발생시킨다.<br>
Timer interrupt가 발생하면 OS는 CPU를 얻을 수 있다.<br>
OS는 timer interrupt가 발생할때까지 어떤 코드를 실행할지 하드웨어에 알려줘야한다.<br>
즉, OS가 강제로 CPU를 얻는다.

OS Regain control - > 실행중인 프로세스를 계속 실행할지 or 다른 프로세스로 변환할지 정해야한다.

- Context Switching : OS가 실행중인 프로세스를 중단하고 다른 프로세스를 실행할 때 수행하는 작업<br>
프로세스 A -> B -> A 순으로 실행할 때 다시 실행한 A를 처음부터 실행하지 않고 이전에 실행중이던 프로세스를 이어서 실행한다.<br>
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
동시성에 대한 문제가 발생할 수 있다.<br>
1. system call중에 timer interrupt가 발생하면 어떻게 될까?<br>
2. interrupt를 처리하는 중에 다른 interrupt가 발생하면 어떻게 될까?<br>

다음과 같은 방법으로 해결할 수 있다.<br>
1. Disable Interrupt : OS가 interrupt를 처리하는 동안 다른 interrupt가 발생하지 않도록 한다.<br>
2. Priority : interrupt에 우선순위를 줘서 순차적으로 처리한다.<br>
3. Locking Mechanism : Lock 기법을 사용하여 처리한다.<br>

Concurrency 단원에서 자세히 다루자.


-----------
## 7 Scheduling: Introduction
저번 단원에서는 Low-level 메커니즘을 배웠다.<br>
이번 단원에서는 OS 스케줄러가 사용하는  High-level 메커니즘을 배운다.<br>


- 스케줄링 정책에 대한 기본적인 프레임워크를 구출할 때 고려해야할 사항

1. 프로세스들이 서로 독립적인가? 의존성이 있는가? 우선순위가 있는가?<br>
2. 어떤 기준이 중요한가? round-robin, CPU 사용량 , 응답 시간, 처리량, 우선순위<br>
3. 초기 컴퓨터의 기본 접근 방식 : 기존의 기본적인 스케줄링 정책 사용

### 7.1 Workload Assumptions
가정하는 각각의 내용을 Workload라고 한다. 이 Workload들을 자세히 알고 있으면 스케줄링 정책을 잘 설계할 수 있다.<br>
1. 각 작업은 동일한 시간 동안 실행됩니다.
2. 모든 작업들은 동시에 도착합니다.
3. 시작되면 각 작업은 완료될 때까지 계속 실행됩니다.
4. 모든 작업들은 CPU만 사용합니다. (입출력을 수행하지 않습니다.)
5. 각 작업의 실행 시간은 알려져 있습니다.

이 가정들은 비현실적이므로 각 가정들을 하나씩 없애가며 스케줄링 정책을 설계해보자.

### 7.2 Scheduling Metrics
>matric : 측정항목<br>
스케줄링 정책을 설계할 때 고려해야할 메트릭이 있다.<br>
#### 반환시간 T_turnaround = T_completion - T_arrival<br>
#### 응답시간 T_response = T_first - T_arrival<br>
#### 형평성  Fairness = T_completion / N<br>
#### 처리량  Throughput = N / T_completion<br>
#### 마감시간 Deadline    - Turnaround Time < Deadline Time<br>



일단 간단하게 , Turnaound time(반환 시간) 만 사용하도록 하자.<br>
#### T_turnaround = T_completion - T_arrival<br>
작업이 완료된 시간에서 작업이 시스템에 도착한 시간을 뺀 값으로 정의.

모든 작업이 동시에 도착한다고 가정했기 때문에, 현재 Trarrival = 0이고 따라서 T_turnaround = T_completion 이다.


### 7.3 First In, First Out (FIFO) Scheduling (Alos known as First come, First Served)
먼저 도착한 작업을 먼저 실행하는 방식<br>

하지만 이는 첫번째 가정을 고려하지 않으므로 비현실적이다.<br>
1. 각 작업은 동일한 시간 동안 실행됩니다.
하지만 A B C 순으로 작업이 들어왔다고 가정할 때<br>
A가 매우 긴시간동안 작동한다면 B C는 짧은 작업임에도 불구하고 오랫동안 기다려야한다. = Convey effect<br>
즉 세 작업의 Turnaround Time은 길어진다.<br>


### 7.4 Shortest Job First (SJF) Scheduling
가장 짧은  작업을 먼저 실행하는 방식<br>
SJF는 평균 회전 시간 측면에서 성능이 좋음.<br>

2. 모든 작업들은 동시에 도착합니다. 라는 가정하에 성립한다.<br>
실행시간이 긴 A가 시작되기전에, A보다 짧은 작업이 계속해서 도착한다면, A는 계속해서 밀릴것이기 때문에 비효율적이다.<br>

### 7.5 Shortest Time-to-Completion First (STCF) Scheduling
3. 시작되면 각 작업은 완료될 때까지 계속 실행됩니다. 라는 가정하에 성립한다.<br>
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


그렇다면 마지막 가정인 각 작업의 길이를 스케줄러가 알고 있다는 가정을 제거해보자.<br>
어떤 프로세스가 만들어졌을 때 OS에게 이 프로세스의 수행시간을 알려줘야 하는데 이는 어떻게 알려줄 수 있을까?<br>
사용자가 알려주는 것이 가장 좋겠지만 항상 그럴 수는 없기 때문에 예측을 통해 알려주게 된다.<br>
예측은 다음과 같은 방법으로 이루어진다.<br>
Exponential Mobing average(지수 이동 평균) :
T_n = a * T_n-1 + (1-a) * T_n<br>


------------------
## 13. The Abstraction: Address Spaces
- Address Virtualization 이란?<br>
메모리 가상화를 하지 않는 경우, 여러 프로세스가 실핼될 때 메모리 관리가 매우 복잡해진다. 각 프로세스는 독립적으로 메모리를 요구하며, 메모리 공간의 충돌을 피하기 위해 운영체제가 메모리를  직접 관리해야 한다.<br>
이러한 경우 문제점으로
1. 메모리의 한계로 인해 여러 프로세스가 실행될 수 없는 경우가 발생할 수 있고 충돌 가능성이 생긴다.<br>
2. 프로세스 간의 보안이 보장되지 않는다.<br>
프로세스는 독립적으로 실행되어야 하며, 서로의 메모리 영역에 접근해서는 안된다. 그러나 가상화를 하지 않은 경우, 각 프로세스가 실제 물리메모리 주소를 공유하게 되어 다른 프로세스의 데이터에 접근할 수 있다.<br>


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
LDE는 대부분의 경우는 프로그램을 하드웨어 위에서 직접 실행하되, 특정한 중요한 시점에 운영 체제가 개입하여 올바른 동작이 이루어지도록 한다.<br>
따라서, OS는 약간의 하드웨어 지원을 통해 실행 중인 프로그램을 가장 효율적으로 가상화하기 위해 최선을 다하지만, 중요한 시점에서 개입하여 하드웨어 제어권을 유지한다.<br>

메모리 가상화도 마찬가지로 하드웨어의 지원을 받아 Virtualization을 구현한다.<br>

효율성과 제어권은 OS의 주요 목표이다.<br>

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

-------------
## 16. Segmentation
지금까지는 각 프로세스의 전체 주소 공산을 메모리에 넣었다.<br>
기준(base)과 한계(bounds) 레지스터를 사용하여 OS는 프로세스를 쉽게 다른 물리 메모리 영역으로 재배치할 수 있다.<br>
그러면 이제 Stack과 Heap 사이의 빈 공간을 낭비하지 않기 위한 방법을 알아보자.<br>

>페이징은 프로세스를 물리적으로 일정한 크기로 나눠서 메모리에 할당하는 것이다.<br>
>세그멘테이션은 프로세스를 논리적 내용을 기반으로 나눠서 메모리에 배치하는 것이다.<br>
>예를 들면, 돼지고기를 1cm 간격으로 자르는 것이 Paging, 목살,삼겹살,갈비로 나누는 것이 Segmentaion이다.<br>


세그멘테이션은 프로세스를 세그먼트의 집합으로 생각한다.



### 16.1 Segmentation: Generalized Base/Bounds
MMU에서 하나의 base와 bounds 쌍만 있는 것이 아니라, 주소 공간의 각 논리적 Segment마다 기준과 한계 쌍을 가지도록 한다.<br>
Segment는 특정 길이의 연속적인 주소 공간 부분을 의미하며, 기준 주소 공간에서는 Code, Stack, Heap 이라는 세 가지 논리적으로 다른 Segment가 있다.<br>

Segmentaion은 OS가 각 Segment를 물리 메모리의 다른 위치에 배치함으로써 미사옹 가상주소공간으로 인해 물리메모리를 채우지 않도록 한다.<br>

### 16.2 Which Segment Are We Referring To?
하드웨어는 Segment Regiser를 사용하여 주소 변환을 수행한다.<br>
어떻게 Segment의 Offset을 알고 어떤 Segment에 해당하는지 알 수 있을까?<br>

- Explicit 접근 방식 : 가상 주소의 상이 몇 비트를 기준으로 주소 공간을 세그먼트로 분할하는 것이다.<br>

에를 들어 세 개의 세그먼트가 있다면 이 작업을 수행하기 위해 두 개의 비트가 필요하다. 14비트 가상 주소의 상위 두 비트를 세그먼트 선택에 사용하는 경우
상위 두 비트가 00 : Code Segment<br>
상위 두 비트가 01 : Heap Segment<br>
상위 두 비트가 10 : Stack Segment<br>
로 나타낼 수 있다.<br>

상위 두 비트가 00이라면 하드웨어는 가상 주소가 Code Segment에 속한다는 것을 알고, Base Bounds  쌍을 사용하여 주소를 올바른 물리 주소로 변환한다.<br>
상위 두 비트가 01 이라면 하드웨어는 가상 주소가 Heap Segment에 속한다는 것을 알고, Heap의 base와 bounds를 사용하여 주소를 올바른 물리 주소로 변환한다.<br>




```C
// 14비트 가상 주소에서 상위 2비트 가져오기
Segment = (VirtualAddress & SEG_MASK) >> SEG_SHIFT
// 오프셋 얻기
Offset = VirtualAddress & OFFSET_MASK
if (Offset >= Bounds[Segment])
    RaiseException(PROTECTION_FAULT)
else
    PhysAddr = Base[Segment] + Offset
    Register = AccessMemory(PhysAddr)

```


- Implicit 접근 방식 : 하드웨어가 주소가 어떻게 생성되었는지에 따라 세그먼트를 결정한다.<br>
예를 들어, 주소가 PC에서 생성되었다면(즉 명령어 가져오기의 경우)
주소는 코드 세그먼트에 속한다.<br>
주소가 Stack이나 base pointer에 기반한 것이라면 Stack Segment에 속하게 된다.<br>
다른 주소는 Heap에 속한다고 가정한다.<br>

### 16.4 What about the Stack?
Stack은 역방향으로 성장하기 때문에 Translation 과정이 다르게 진행된다.<br>

이를 위해 하드웨어의 지원이 필요하다.<br>
하드웨어는 Segment가 어느 방향으로 성장하는지 알고 있어야 한다.<br>
이는 양수 방향으로 성장할 때 1로 설정되는 비트, 음수 방향으로 성장할 때 0으로 설정되는 비트를 사용하여 수행할 수 있다.<br>

가상주소 15KB를 physical 주소 27KB에 Mapping한다고 해보자.<br>
가상주소는 다음과 같다.  11 1100 0000 0000 (16진수 0x3C00)<br>
하드웨어는 상위 두 비트 11를 세그먼트로 지정하지만 여기서는 3KB이 Offset이 남는다.<br>
올바른 음수 offset은 3KB에서 Segment의 최대 크기인 4KB를 빼면 -1KB가 된다.<br>
실제 메모리의 주소는 28KB- 1KB가 된 27KB가 된다.<br>

>Offset이란?
>가상 주소의 남은 부분을 의미한다.<br>

### 16.5 Fine-grained vs. Coarse-grained Segmentation
- size가 큰 segment :
segment의 수가 줄어들게 되어 관리가 편해지지만 단순하게 구성할 수 밖에 없다
- size가 작은 segment :
유연하게 메모리를 구성할 수 있지만 segment가 많기 때문에 관리가 힘들어질 수 있다.

### 16.6 OS Support
Segmentation을 할 때 OS가 해야 하는 일은 다음과 같다.<br>

1. Context-Switch문제 


## 18 Paging: Introduction
OS는 space-management problem에 대해 두 가지 방법을 제공한다.<br>
첫 번째는 세그먼트화를 통해 공간을 가변 크기 조각으로 분할하는 것이다.<br>
세그먼트화는 공간을 다양한 크기의 청크로 분할하면 공간 자체가 단편화되어 할당이 시간이 지남에 따라 더 어려질 수 있다.<br>

두 번째 방법으로 공간을 고정된 크기의 조각으로 분할하는 페이징이 있다.<br>
프로세스의 주소 공간을 일부 가변 크기의 논리적 세그먼트(예: 코드, 힙, 스택)로 분할하는 대신에, 우리는 고정 크기의 단위인 페이지로 나눈다.<br>

페이징은 세그먼트화에 비해 많은 이점이 있다.<br>
1. 유연성 : 체계적인 페이징 접근 방식을 사용하면 프로세스가 주소 공간을 어떻게 사용하든지 효과적으로 주소 공간을 추상화할 수 있다. 
예를 들어, 힙과 스택이 어떻게 성장하고 사용되는지에 대한 가정을 하지 않는다.<br>
2. 페이징이 제공하는 자유 공간 관리의 단순성:
운영 체제가 우리의 작은 64바이트 주소 공간을 8개의 페이지 물리 메모리에 배치하고 싶다면, 그냥 네 개의 빈 페이지를 찾는다.

페이지 테이블 : 가상 주소 공간의 각 페이지에 대한 물리적 주소를 저장하는 테이블<br>
주소 공간의 각 가상 페이지가 물리 메모리에 어디에 위치하는지 기록하기 위해 운영 체제는 일반적으로 페이지 테이블이라고 하는 프로세스별 데이터 구조를 유지한다.<br>

#### Address Translation
먼저 가상 페이지 번호 (VPN)와 페이지 내 오프셋으로 나눈다.<br>
+ VPN : 페이지 테이블에서 가상 페이지가 물리 메모리의 어느 위치에 있는지를 찾기 위해 사용.
+ 오프셋 : 페이지 내에서 접근하려는 바이트의 위치를 나타냄.

>VPN(Virtual Page Number) : 가상 페이지 번호<br
>PFN(Physical Frame Number) : 물리적 페이지 번호<br>


* 프로그램의 논리 주소 = VPN(페이지 번호) + 오프셋
* 프로그램의 물리 주소 = PFN(프레임 번호) + 오프셋




프로세스의 가상 주소 공간이 64바이트이므로 가상 주소에 대해 총 6개의 비트가 필요하다고 가정해보자.

|Va5|Va4|Va3|Va2|Va1|Va0|

여기서 Va5는 최상위 비트이고, Va0은 최하위 비트이다.

64바이트 주소 공간에서 페이지 크기는 16바이트이므로 4개의 페이지를 선택할 수 있다.<br>
가상 페이지 번호 (VPN)는 주소의 상위 2비트이고, 나머지 비트는 페이지 내에서 어떤 바이트를 참조하는지를 알려준다.<br>
이 예제에서는 4비트이며, 이를 오프셋이라고 한다.<br>

|VPN|VPN|Offset|Offset|Offset|Offset|Offset|Offset|
|Va5|Va4|Va3|Va2|Va1|Va0|

#### Example

``` C
movl 21, %eax
```
는 가상 주소 21로 로드하는 것이다.

"21"을 이진 형태로 변환하면 "010101"이 되며, 따라서 이 가상 주소를 살펴보고 가상 페이지 번호 (VPN)와 오프셋으로 분해해보자.<br>

가상 주소 "21"은 가상 페이지 "01" (또는 1)의 5번째 ("0101"번째) 바이트에 해당한다.
가상 페이지 번호를 사용하여 페이지 테이블을 인덱싱하고, 가상 페이지 1이 어느 물리 프레임에 속하는지 찾을 수 있다. 
페이지 테이블에서 물리프레임번호(PFN)dms 7(111)이다. 이 가상 주소를 VPN을 PFN으로 바꾸고, 그런 다음 물리 메모리로 로드를 수행할 수 있다.

즉, 가상 주소 "21"은 가상 페이지 "01" (또는 1)의 5번째 ("0101" 번째) 바이트에 있다.<br>
페이지 테이블을 이용해 VPN을 PFN으로 바꾸고, OFFSET은 그대로 사용한다.<br>
실제 메모리는 1110101이 된다.<br>

### 18.2 Where Are Page Tables Stored?
그럼 이제 page table이 어디에 저장되는지 살펴보겠습니다. 우선 page table은 가변 크기 할당에서 사용하던 segment table이나 base, limit 레지스터를 사용하는 것 보다 필요한 크기가 훨씬 커질 수 있습니다. 예를 들어 4KB page를 갖는 32 비트의 가상 주소 공간을 생각해보겠습니다. 이 가상 주소는 20비트의 VPN, 12비트의 offset으로 분할됩니다. VPN이 20개의 비트로 표현된다라는 말은 OS가 각 프로세스에 대해 관리해야 하는 page수가 100만 개 정도 된다는 것입니다. 또한 하나의 page의 정보를 저장하기 위한 page table의 크기가 4바이트라고 한다면 4바이트 * 100만 = 4MB입니다. 이러한 프로세스가 100개라면... 400MB의 메모리를 필요로 하게 됩니다. 이를 CPU의 레지스터에 저장한다는 것은 불가능하므로 page table은 메모리에 저장하게 된다고 일단 생각하겠습니다. 실제로는 메모리에 저장되는 것이 아닙니다.  나중에 OS가 사용하는 메모리가 가상화될 수 있다는 것을 알게 되는데요, page table은 OS 가상 메모리에 저장됩니다. 또한 디스크로 swap 될 수도 있습니다. 


페이지 테이블은 가변크기할당을 사용하던 segment table보다 훨씬 커질 수 있다.<br>
예를 들어, 4KB 페이지를 갖는 32비트 가상 주소 공간을 생각해보자.  이 가상 주소는 20비트의 VPN, 12비트의 오프셋으로 분할된다.<br>
VPN이 20비트라는 것은 OS가 각 프로세스에 대해 관리해야 할 page의 수가 100만개 가량 된다는 것이다. 또한 하나의 page의 정보를 저장하기 위한 page table의 크기가 4바이트라고 한다면 총 4MB이다.<br>
이러한 프로세스를 100개 가지고 있다면 400MB의 메모리가 필요하다.<br>
이를 CPU의 레지스터에 저장한다는 것은 불가능하므로 page table은 메모리에 저장하게 된다고 생각하자.<br>
실제로는 메모리에 저장되는 것은 아니다. Page Table은 OS 가상 메모리에 저장된다.<br> 또한 디스크로 Swap 될 수도 있다.


### 18.3  What’s Actually In The Page Table?

>Page Table Entry (PTE) : Page Table에서 각 Page의 정보

Page table의 역할은 가상 주소를 실제 메모리 주소로 변환해줄 때 사용하는 자료구조이다.
+ 1차원 구조 :  VPN으로 해당 프로세스의 page table 배열을 인덱싱하고 변환을 위해 해당 인덱스의 page table entry(PTE)를 조회한다. <br>

- PTE의 구조
    + P : 현재 실제 메모리나 디스크에 존재하는 page인지 확인하는 비트
    + R/W : page가 쓰기가 허용되는지 여부를 결정하는 비트
    + U/S : User Mode 프로세스가 page에 접근할 수 있는지에 대한 비트
    + PWT, PCD, PAT, G : page에 대해 하드웨어 캐싱이 작동하는 방식을 결정하는 비트
    + A : 최근 참조된 적이 있는지에 대한 정보 (페이지 교체 정책에서 사용됨)
    + D : page가 수정된 적이 있는지에 대한 정보

나머지는 실제 메모리 주소를 저장하는 PFN의 정보가 들어간다.

### 18.4 Paging is too Slow
좀 전에 살펴봤던  
```C
movl 21, %eax
```
즉 가상 주소 21을 실제 주소 117로 변환하는 과정을 다시 살펴보자.

1. 프로세스의 page table을 찾는다.
2. Page table의 정보를 통해 주소 변환을 수행한다.
3. 실제 메모리에서 데이터를 가져온다.


위와 같은 과정을 위해서 하드웨어는 현재 실행중인 프로세스에 대한 page table위치를 알아야 한다.<br>
먼저 프로세스의 가상 주소로 VPN(virtual page number)과 offset을 알아낸 뒤 해당 프로세스의 VPN값으로 page table에서 프로세스의 PTE정보를 가지고 온다.<br> 가지고 온 PTE(page table entry) 정보에서 PFN(page frame number)을 추출하고 offset, PFN을 사용하여 실제 메모리 주소를 얻는다.

```c
// 가상 주소에서 vpn만 추출한다.
VPN = (VirtualAddress & VPN_MASK) >> SHIFT

// Page table 주소에 vpn 값으로 현재 접근하는 프로세스의 PTE 찾는다.
//페이지 테이블의 주소를 찾는 것!
//PTEAddr은 페이지 테이블에서 가상 페이지에 대한 정보를 가리키는 포인터로 사용!!
PTEAddr = PageTableBaseRegister + (VPN * sizeof(PTE))
            // PageTableBaseRegister : page table의 시작 주소
            // sizeof(PTE) : page table entry의 크기


// 가상 주소에서 offset만 추출한다.
offset = VirtualAddress & OFFSET_MASK

// offset과 PFN 값으로 실제 메모리 주소를 얻는다.
PhysAddr = (PFN << SHIFT) | offset
```


PTEAddr은 페이지 테이블에서 VItual Page에 대한 정보를 찾기 위한 중간 단계로 사용된다.<br>
PTEAddr을 통해 페이지 테이블 항목(PTE)을 찾으면 해당 항목에 저장된 물리 페이지 번호(PFN)를 사용하여 실제 메모리 주소를 계산할 수 있다.


이를 수행하면 메모리에서 데이터를 가지고 오는데 메모리를 한 번 참조하는 것이 아닌 두 번 참조하게 되고 성능도 그만큼 저하되게 된다.<br> 
Paging 방법을 사용하는 것은 메모리 가상화를 위한 좋은 방법처럼 보이지만 성능도 안 좋고 메모리도 많이 사용하는 방법인 것입니다. <br>
즉 이를 극복해야 실제로 사용할 수 있습니다.

### 18.5 A Memory Trace
Paging이 메모리에 접근하는 과정을 다시 한번 보자.
```C
int array[1000];

for(int i = 0; i < 1000; i++) {
    array[i] = 0;
}
```
정수형 배열을 초기화 해보자. 이 과정중 한번의 반복을 어셈블리어로 표현해보면
```C
가상주소 
1024   movl  $0x0, (%edi, %eax, 4) // 0x0위치로 이동 , %edi는 배열의 시작 주소 , %eax는 인덱스 값 , 4는 정수형 배열이므로 인덱스당 4바이트만큼 사용
1028   incl  %eax              // %eax의 값을 증가시킴, i++을 의미
1032   cmpl  $0x03e8, %eax  // i < 1000 부분을 처리하는 곳
1036   jne   0x1024         // true가 발생하면  1024로 이동하여 반복을 수행
```
이렇게 된다. 이를 다시 한번 살펴보자.

#### 정리 :
프로세스는 가상메모리에서 동작한다. 여기서 VPN을 통해 page table에서 PTE를 찾고 PTE에서 PFN을 찾아 실제 메모리 주소를 찾는다. <br>
VPN -> Page Table(PFN)  -> Physical Memory
장점 : 외부단편화 해결! , 유연한 메모리 관리 가능
단점 : 메모리 접근 속도 저하 , 메모리 사용량 증가


#### 그렇다면 VPN이 직접 PFN을 찾는 것이 아닌 page table을 거쳐서 PFN을 찾는 이유는 무엇일까?
VPN이 직접 페이지의 실제 위치를 나타내는 방법을 사용하지 않는 이유는 가상 주소 공간과 실제 메모리 주소 공간이 다른 크기를 가질 수 있기 때문이다.<br>
가상 주소 공간은 프로세스마다 독립적으로 할당되며, 그 크기는 프로세스의 요구에 따라 다를 수 있다.<br> 
반면에 실제 메모리 주소 공간은 하드웨어의 제한에 따라 정해진 크기를 가지고 있기 때문이다.<br>


## 19 Paging: Faster Translations (TLBs)
Paging의 문제점은 잦은 메모리 접근과 page table을 위한 메모리 사용으로 인한 메모리 낭비이다.<br>

이 두개의 문제 중에서 잦은 메모리 접근을 보완하기 위한 Translation Lookaside Buffer(TLB)를 살펴보자.<br>

Paging을 사용하여 메모리 가상화를 지원한다면 오버 헤드가 발생한다.<br>
메모리에 자주 접근하게 되고 Page Table을 위한 메모리도 존재해야 한다.<br>
메모리의 잦은 접근을 줄이기 위해 CPU의 MMU에 위치한 TLB를 사용한다.<br>
TLB에는 VPN(Virtual Page Number)과 PFN(Page Frame Number)정보가 쌍으로 저장되어 있다.<br>
이를 사용해 주소변환을 하도록 도와주는 하드웨어 Cache이다.<br>




### 19.1 TLB Basic Algorithm


Page Table과 TLB를 이용한 주소 변환 과정을 알아보자.
```C
VPN = (VirtualAddress & VPN_MASK) >> SHIFT
(Success, TlbEntry) = TLB_Lookup(VPN)
// CPU가 주소변환을 할 때 TLB가 존재한다면 주소 변환을 위해 TLB부터 조회한다.
if (Success == True)   // TLB Hit
    if (CanAccess(TlbEntry.ProtectBits) == True)
        Offset   = VirtualAddress & OFFSET_MASK
        PhysAddr = (TlbEntry.PFN << SHIFT) | Offset
        Register = AccessMemory(PhysAddr)
    else
        RaiseException(PROTECTION_FAULT)
// TLB에 존재하지 않는다면 Page Table에 접근하여 주소 변환 정보를  TLB에 저장한다.
else                  // TLB Miss
    PTEAddr = PTBR + (VPN * sizeof(PTE))
    PTE = AccessMemory(PTEAddr)
    if (PTE.Valid == False)
        RaiseException(SEGMENTATION_FAULT)
    else if (CanAccess(PTE.ProtectBits) == False)
        RaiseException(PROTECTION_FAULT)
    else
        TLB_Insert(VPN, PTE.PFN, PTE.ProtectBits)
        RetryInstruction()
```

CPU가 주소변환을 할 때 TLB가 존재한다면 주소 변환을 위해 TLB부터 조회한다.<br>
원하는 변환 정보가 있다면 바로 변환을 하고 그렇지 않다면 메모리에 존재하는 Page Table에 접근하여 변환 정보를 TLB에 저장한다.<br>
그리고 다시 주소 변환을 시도한다.<br>

TLB를 사용하더라도 특정 Page에 처음 접근할 때는 메모리 접근이 필요하다.<br>
이때 TLB Miss가 발생한다.<br>


>spatial locality(공간 지역성)란?
>어떤 요소에 접근 한 상황이라면 그 주변의 요소들에 접근할 가능성이 높다.

>temporal locality(시간 지역성)란?
>최근에 어떤 요소에 접근 한 상황이라면 그 요소에 다시 접근할 가능성이 높다.

### 19.3 Who Handles The TLB Miss?

TLB MISS가 발생하면 메모리에 존재하는 page table에 접근해야한다.<br>

이 때 메모리에 접근하는 방법은 두가지가 있다.<br>
1. HW가 직접 접근하여 page table을 가져온다.
2. OS가 접근하여 page table을 가져온다.


HW가 직접 접근하는 방법은 HW가 page table이 메모리 어디에 존재하는지와 Page Table에 존재하는 정보를 정확하게 알아야 한다.<br>

소프트웨어 즉 OS가 TLB MISS를 관리하는 방법은
1. TLB MISS가 발생하면 하드웨어는 현재 명령을 중지하고 권한을 Kernel Mode로 올린 뒤 Trap 처리기로 예외를 발생시킨다.(TLB를 업데이트 한 뒤 다시 실행한다.)
2. Page Table을 조회하여 TLB를 업데이트하고 다시 주소 변환을 시도한다.

```C
VPN = (VirtualAddress & VPN_MASK) >> SHIFT
(Success, TLBEntry) = TLB_Lookup(VPN)

// TLB에 변환 정보가 존재한다면
if (Success == True)
    // 접근 가능한지 확인후 TLB의 정보로 바로 주소변환
    if (CanAccess(TLBEntry.ProtectBits) == True)
        Offset = VirtualAddress & OFFSET_MASK
        PhysAddr = (TLBEntry.PFN << SHIFT) | Offset
        Register = AccessMemory(PhysAddr)
    else
        RaiseException(PROTECTION_FAULT)

// TLB에 변환 정보가 없다면
else
    // TLB Miss 핸들러 호출
    RaiseException(TLB_MISS)
```

OS로 TLB를 관리할 떄의 장점은 유연성이다.<br>
OS는 하드웨어의 변경 없이 Page Table을 구현하는 모든 자료 구조를 사용할 수 있다.<br>

### 19.4 TLB Contents: What’s In There?
일반적으로 TLB에는 32, 64, 128 개의 Entry가 존재할 수 있으며, 이를 Fully Associative라고 한다.<br>
> Fully Associative란?
> TLB에 저장되는 VPN과 PFN의 쌍은 어느 위치에 저장되어도 상관없다는 의미이다.

주소변환 정보가 TLB의 어디에나 저장될 수 있고 하드웨어는 TLB를 병렬로 검색하여 주소 변환 정보를 찾을 수 있다.<br>

TLB 구조
|VPN|PFN|other bits|

### 19.5 TLB Issue: Context Switches

TLB를 사용할 때 Context Swtitch가 발생한다면 어떻게 될까?<br>

> Context Switch : OS에서 실행 중인 프로세스를 중지하고 다른 프로세스를 실행하기 위해 현재 프로세스의 상태를 저장하고 다른 프로세스의 상태를 복원하는 과정을 말한다.<br>

TLB에는 현재 실행중인 프로세스에 대한 Virtual Address와 Physical Address의 쌍이 저장되어 있다.<br>
이러한 정보는 다른 프로세스에 대해서는 의미가 없다.<br>
따라서 Context Switch가 발생하면 하드웨어와 OS는 현재 실행 중인 프로세스의 변환 정보가 이전에 실행한 다른 프로세스의 변환 정보로 잘못 사용되지 않도록 주의해야한다.<br>


이를 해결하기 위한 방법은 두가지가 있다.<br>

1. Context Switch가 발생하면 TLB를 비운다.(Flush)
TLB의 주소 변환 정보중 Valid Bit 값을 0으로 초기화한다.<br>
이 방법은 TLB를 비우는데 시간이 걸리기 때문에 비효율적이다.<br>

2. TLB구조에 ASID(Address Space Identifier)를 추가한다.
HW 의 도움을 받는 것인데, ASID는 프로세스의 고유한 식별자이다.<br>
프로세스마다 다른 ASID 정보를 저장하여, 주소변환 시 ASID를 비교하여 프로세스를 구별한다.<br>

그렇다면 실제 페이지의 주소 PFN이 같은 상황, 즉 프로세스가 페이지를 공유하고 있는 상황은 어떻게 될까?<br>

PFN이 동일하게 되면 즉, Page를 공유하게 된다면 메모리의 사용을 줄일 수 있다는 이득이 있다.
### 19.5 Issue: Replacement Policy
TLB에 저장 가능한 공간이 가득 찬 경우에 새로운 프로세스가 실행된다면 일반적으로 가장 오랜 시간동안 사용하지 않은 TLB Entry를 제거한다.<br>
이를 LRU(Least Recently Used)라고 한다.<br>

-----
## 21 Beyond Physical Memory: Mechanisms
지금까지는 가상 주소 공간이 작아서 물리 메모리에 들어갈 수 있다고 가정하였다.<br>

이제 이러한 가정을 완화ㅏ고, 많은 동시 실행중인 큰 주소 공간을 지원하고자 한다.<br>

이를 위해 memory hierarchy에 새로운 level을 추가한다.<br>
메모리 계층을 다음과 같이 표현할 수 있다.<br>

레지스터
L1 캐시(SRAM)
L2 캐시(SRAM)
메인 메모리(DRAM)
local storage(SSD, HDD)
remote storate(SERVER)
아래로 갈수록 싸고 느리고 크다.<br>


지금까지는 모든 페이지가 물리 메모리에 있는 것으로 가정했다.<br>
그러나 큰 주소 공간을 지원하기 위해서는 현재 크게 요구되지 않는 주소 공간의 일부를 저장할 공간이 필요합니다.<br>
일반적으로 이러한 위치의 특성은 메모리보다 더 많은 용량을 가져야 하며, 결과적으로 느리다.<br>
현대에는 이러한 역할을 하는 장치로 SSD, HDD가 있다.<br>
따라서 우리의 메모리 계층에서는 크고 느린 하드드라이브가 아래에 위치하고 그 위에 메모리가 있다.<br>


그렇다면 우리는 page를 main memory에서 local storage로 옮겨서 프로세스를 실행하는 방법에 대해 알아볼 예정이다.

그렇다면 왜 이와 같이 하나의 큰 주소 공간을 지원하려고 할까?<br>
이는 편의성 때문이다.<br>
큰 주소 공간을 사용하면 프로그램의 데이터 구조에 메모리 공간이 충분한지 걱정할 필요가 없다.<br>
대신 필요한대로 메모리를 할당하여ㅠ 프로그램을 자연스럽게 작성할 수 있다.<br>

### 21.1 Swap Space
메모리와 디스크 사이의 데이터 전송을 위해 디스크에 일부 공간을 예약한다. 이를 Swap Space라고 한다.<br>
프로세스는 메모리와 디스크에 모두 존재할 수 있으며, 프로세스의 일부는 메모리에 있고 일부는 디스크에 있을 수 있다.<br>
또한 디스크에만 저장돼있는 프로세스도 있을 수 있다.<br>

### 21.2 The Present Bit
그럼 이제 메모리에 있는지 디스크에 있는지 어떻게 알 수 있을까?<br>

이를 위해 우리는 Page Table Entry에 Present Bit를 추가한다.<br>
메모리에 존재하지 않는다면 page fault가 발생하고 page fault handler가 디스크의 swap space에서 페이지를 가져와 메모리에 올린다.<br>

### 21.3 Page Fault
우선 page fault는 정상적인 접근이고, segmentation fault는 비정상적인 접근이므로 프로세스가 중단된다.<br>

>Page Fault란?
>프로세스가 메모리에 존재하지 않는 page에 접근하려고 할 때 발생한다.<br>

OS는 Disk 주소에 대한 정보를 page의 PTR(Page Table Entry)에서 알아낸다. 이렇게 알아낸 주소로 page를 가져오면 메모리에 할당하고 page table의 present bit를 업데이트하여 page가 메모리에 존재한다는 것을 기억한다.<br> 그런 뒤 TLB에도 해당 정보를 업데이트하여 다음 접근부터는 TLB로 주소변환을 수행한다.<br>

### 21.4 What If Memory Is Full?
만약 메모리 공간이 부족한 상황이라면 어떻게 될까?<br>
OS는 현재 메모리에 존재하는 page들 중 일부를 디스크의 swap space로 옮긴다.<br>
하지만 이 때 옮긴 page가 매우 자주 사용되는 중요한 page라면, 이는 성능 저하를 야기할 수 있다.<br>

### 21.5 Page Fault Control Flow
Page Fault 발생
```C
// 추출한 VPN에 대한 주소변환 정보가 TLB에 있는지 확인
(Success, TLBEntry) = TLB_Lookup(VPN)
// TLB에 있으면
if (Success == True)
    if (CanAccess(TLBEntry.ProtectBits) == True)
        Offset = VirtualAddress & OFFSET_MASK
        PhyAddr = (TLBEntry.PFN << SHIFT) | OFFSET
        Register = AccessMemory(PhysAddr)
    else
        RaiseException(PROTECTION_FAULT)
// TLB에 없으면
else
    // Page Table에 접근하여 page 위치 알아온다
    PTEAddr = PTBR + (VPN * sizeof(PTE))
    PTE = AccessMemory(PTEAddr)
    if (PTE.Valid == False)
        RaiseException(SEGMENTATION_FAULT)
    else
        if (CanAccess(PTE.ProtectBits) == False)
            RaiseException(PROTECTION_FAULT)
        // Present Bit가 True라면 (메모리에 page table 존재)
        else if (PTE.Present == True)
            TLB_Insert(VPN, PTE.PFN, PTE.ProtectBits)
            RetryInstruction()
        // Present Bit가 False라면 (Swap 공간에 page table 존재)
        else if (PTE.Present == False)
            // Page Fault 발생
            RaiseException(PAGE_FAULT)
```

OS에서 처리
```C
// Page를 가지고 와서 할당할 메모리 공간을 찾는다
PFN = FindFreePhysicalPage()
if (PFN == -1)
    PFN = EvictPage()
// Disk에서 Page Table 데이터를 가지고 온다
DiskRead(PTE.DiskAddr, PFN)
// Present Bit를 True로 수정
PTE.present = True
PTE.PFN = PFN
RetryInstruction()
```

## 22 Beyond Physical Memory: Policies
이제 어떤 Page를 swap 공간으로 교체할 것인가에 대해 알아보자.<br>

### 22.1 Cache Management
Policy를 알아보기 전에 어떠한 문제를 해결할 것이지 자세히 알아보자.<br>
주 메모리는 시스템의 모든 프로세스의 일부를 보유하고 있다.<br>
(모든 프로세스가 주 메모리를 공유한다는 것을 의미하는 것 같다.)<br>
그렇기 때문에 가상메모리의 페이지의 캐시로 볼 수 있다. 따라서 이 캐시의 교체 정책을 선택하는 목표는 캐시 미스의 수를 최소화하는 것이다.<br>

다시 말해, 디스크에서 페이지를 가져와야 하는 횟수를 최소화하는 것이다.<br>또는 캐시 히트의 수, 즉 메모리에 접근한 페이지가 메모리에 존재하는 횟수를 최대화하는 것으로도 볼 수 있다.<br>

cache hits 와 misses 를 알면 프로그램의 평균 메모리 액세스 시간(AMAT)를 계산할 수 있다.

AMAT = TM +(PMiss ·TD) <br>

TM은 메모리 액세스 비용, TD는 디스크 액세스 비용, PMiss는 캐시에서 데이터를 찾지 못하는 확률이다.<br>

### 22.2 The Optimal Replacement Policy
가장 이상적인 Replacement Policy는 다음과 같다.<br>
Belady가 개발한 최적교체정책이 있다. 이는 앞으로 가장 오랫동안 사용하지 않을 페이지를 교체하는 것이다.<br>


처음 page를 요청할 때는 아직 캐시에 아무런 page가 없으므로 무조건 miss가 발생한다. 이를 Compulsory Miss라고 한다.<br>


### 22.3 A Simple Policy: FIFO
FIFO 교체 정책은 구현하기 매우 간단하다는 장점이 있다.<br>
하지만 최적 값과 비교했을 때 FIFO의 성능은 현저히 떨어집니다.<br>

### 23.4 Another Simple Policy: Random
임의의 페이지를 선책하여 교체하는 방식이다.<br>
구현하기 간단하지만 어떤 블록을 제거할지에 대해 아무런 정보를 제공하지 않는다.<br>

성능이 운에 달려있기 때문에 다른 정책보다 더 나은 성능을 보장하지 못한다.<br>

### 23.5 Using History: LRU
FIFO나 Random 과 같이 간단한 Policy는 중요한 Page를 제거할 수도 있다는 단점이 있다.<br>

그렇다면 Page의 중요도를 나타낸 지표는 두 가지가 있다.<br>
1. localty(지역성) : 최근에 사용한 Page는 다시 사용할 확률이 높다.
2. 빈도 : 자주 사용하는 Page는 다시 사용할 확률이 높다.
 

- Least-Frequently-Used : 
LFU(최소빈도사용정책)은 가장 적게 사용된 페이지를 교체한다.<br>
-  Least-Recently- Used : 
LRU(최근) 정책은 가장 오랫동안 사용하지 않은 페이지를 교체한다.<br>

Most-Frequently-Used (MFU)와 Most-Recently-Used (MRU)의 정반대 정책도 존재하지만, 이는 실제로는 잘 사용되지 않는다.<br>

### 22.6 Workload Examples
이제 각 정책의 성능을 비교해보자.<br>

첫 번째로 작업의 지역성이 없는 경우는 Random, FIFO, LRU 모두 동일한 성능을 보인다.<br>

두 번째로 작업 부하가 전체 작업 부하를 수용할 수 있는 크기의 캐시인 경우 어떤 정책을 사용하더라도 상관이 없다.<br>


#### 지역성이 있는 80-20 Workload 가정 :<br>
참조의 80%가 페이지의 20%인 핫페이지에서 이루어지며 , 나머지 20%의 참조는 나머지 80%의 페이지인 콜드페이지에서 이루어진다.<br>
즉 사용 빈도가 높은 페이지가 존재한다는 것이다.<br>
이 가정에서는 LRU 정책이 가장 좋은 성능을 보인다.<br>

#### 0~49 page까지 순서대로 50개의 page를 반복적으로 참조하는 Workload 가정 :<br>
이 경우 캐시의 크기가 50을 넘어가면 모든 정책에서 hit가 100%로 나타난다.<br>
하지만 캐시 크기가 50미만일 때는 LRU, FIFO가 모두 최악의 경우인 hit rate가 0%를 보인다.<br>
random의 방법은 어느 정도 성능이 나오는데, 이는 random의 장점인 예외가 없다는 것이다.

### 22.7 Implementing Historical Algorithms
LRU 정책은 어떤 page가 가장 사용된지 오래 됐는지, 어떤 page가 가장 최근에 사용됐는지를 알아야 한다.<br>
이러한 비용이 커진다면 LRU를 좋은 정책이라고 볼 수 없다.<br>
이를 효율적으로 처리하기 위해 하드웨어의 지원을 받는다. 

Machine은 각 페이지 액세스마다 메모리의 시간 필드를 업데이트할 수 있다. 따라서 페이지가 액세스되면 하드웨어의 시간 필드가 현재 시간으로 설정된다. 그런 다음 페이지를 교체할 때, OS는 시스템의 모든 시간 필드를 스캔하여 가장 오래된 페이지를 찾을 수 있다.<br>

그러나 시스템 내의 page 수가 증가함에 따라 거대한 시간 배열을 스캔하여 가장 오래된 페이지를 찾는 것은 매우 비효율적이다.<br>

그렇다면 절대적으로 가장 오래된 페이지를 찾는 것이 아니라 근사치로 대체할 수 있을까?<br>

### 22.8 Approximating LRU
LRU를 근사하는 것은 계산 오버헤드 측면에서 더 실현 가능하며, 실제로 많은 현대 시스템에서 사용되고 있다.<br>
이는 하드웨어의 지원이 필요하며 , use bit를 사용한다.<br>
시스템의 각 페이지마다 하나의 use bit가 있으며,페이지가 참조될 때마다 1로 업데이트 된다.

OS는 usebit를 LRU를 근사하는 방법은 다음과 같다.<br>

- Clock Algorithm : 시스템의 모든 페이지가 원형 목록으로 배열되어 있다고 상상해보자.<br>
시계 바늘이 특정 페이지를 가리키고 시작한다. 페이지 교체가 발생해야 할 때, OS는 현재 가리키고 있는 페이지 P의 use bit가 1인지 0인지 확인한다.<br>
1이라면 페이지 P가 최근에 사용되었음을 의미하므로 교체되지 않는다. 이후  페이지 P의 사용 비트는 0(지워짐)으로 설정되고, 시계 바늘은 다음 페이지(P + 1)로 증가합니다. 이 알고리즘은 모든 페이지의 사용 비트가 0으로 설정될 때까지 진행됩

### 22.9 Considering Dirty Pages
메모리가 page에 있는 동안 page가 수정되었는지 여부를 알아야 한다.<br>
수정된 page를 교체하려고 한다면 Disk에 다시 기록해야 해서 비용이 많이 들기 때문이다.<br> 수정되지 않은 page를 교체할 때는 I/O 없이 사용할 수 있지마ㅣㄴ
