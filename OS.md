# OS

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
- >fork() : 새로운 프로세스를 위한 메모리 할당 X , 프로세스가 하나 더 생김!
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



