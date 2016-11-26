---
title: Linux 信号及其处理
date: 2016-06-06 15:58:12
categories: Linux
tags: [signal]
toc: true
---

## Linux 信号
### 基本概念
软中断信号（`signal`，简称为信号）用来通知进程发生了异步事件。进程之间可以互相通过系统调用kill发送软中断信号。内核也可以因为内部事件而给进程发送信号，通知进程发生了某个事件。
__注意：信号只是用来通知某进程发生了某事件，并不给进程传递任何数据。__

收到信号的进程对各种信号有不同处理方法：
* 第一种方法，类似中断的处理程序，对于需要处理的信号，进程可以指定处理函数，由该函数处理；
* 第二种方法，忽略某个信号，对该信号不做任何处理，就像未发生过一样；
* 第三种方法，对该信号的处理保留系统默认值，对大部分信号的缺省操作是使进程终止。
<!-- More -->

void (*signal(int sig, void(*func)(int)))(int);
Set function to handle signal
Specifies a way  to handle the signals with the signal number specified by sig.
Parameter func specifies one of the three ways in which a signal can be handled by a program：
* Default handling (SIG_DEL):The signal is handled by the default action for that particular signal.
* Ignore signal (SIG_IGN):The signal is ignored and the code execution will continue even if not meaningful.
* Function handler: A specific function is defined to handle the signal.
Either SIG_DEL or SIG_IGN is set as the default signal handling behavior at program start up for each of the supported signals.

在进程表的表项中有一个软中断信号域，该域中每一位对应一个信号，进程并不知道在处理之前来过多好个信号。

### 信号类型
发出信号的原因很多，按信号发出原因分类如下：
1. 与进程终止相关的信号。当进程退出，或者子进程终止时，发出这类信号。
2. 与进程例外事件相关的信号。如进程越界，企图写入只读内存区域，执行特权指令，各种硬件错误。
3. 与系统调用期间遇到不可恢复条件相关的信号。如执行系统调用 exec 时，原有资源已释放，而目前系统资源又已经耗尽。
4. 与执行系统调用时遇到非预测错误条件相关的信号。如执行一个并不存在的系统调用。
5. 在用户态下的进程发出的信号。如进程调用 kill 向其他进程发送信号。
6. 与终端交互相关的信号。如用户关闭一个终端，或者按下 break 键。
7. 跟踪进程执行的信号。

### 信号描述
编号 01~31：传统`UNIX`支持的信号，是不可靠信号（非实时的）。
编号 32~64：扩充的信号，称作可靠信号（实时信号）。
不可靠信号与可靠信号的区别：前者不支持排队，可能造成信号丢失，而后者不会。

| 编号 | 信号 | 默认动作 | 描述 |
|:-----:|:-----:|:-----:|:------:|
| 1 | SIGHUP | 进程终止 | 用户终端连接结束。 |
| 2 | SIGINT | 进程终止 | INTR字符，`Ctrl+c`发出，通知前台进程终止进程。 |
| 3 | SIGQUIT | 进程意外结束（Dump） | QUIT字符，`Ctrl+\`发出，进程会产生 core 文件，类似于程序错误信号。 |
| 4 | SIGILL | 进程意外结束（Dump） | 执行了非法指令：可执行文件本身错误、试图执行数据段、堆栈溢出。 |
| 5 | SIGTRAP | 进程意外结束（Dump） | 有断点指令或其它 trap 指令产生。 |
| 6 | SIGABRT | 进程意外结束（Dump） | 程序异常终止，如调用`abort()`。 |
| 7 | SIGBUS | 进程意外结束（Dump） | 非法地址，包括内存地址对齐出错。 |
| 8 | SIGFPE | 进程意外结束（Dump） | 发生致命的算术运算错误：浮点运算错误、溢出、除零。 |
| 9 | SIGKILL | 进程终止 | 立即结束程序运行，不能被阻塞、处理、忽略。 |
| 10 | SIGUSR1 | 进程终止 | 留给用户使用。 |
| 11 | SIGSEGV | 进程意外结束（Dump） | 试图访问未分配给自己的内存，试图向没有写权限的内存地址写数据。 |
| 12 | SIGUSR2 | 进程终止 | 留给用户使用。 |
| 13 | SIGPIPE | 进程终止 | 管道破裂。<br>读管道没有打开或者意外终止就往管道写，写进程会收到 SIGPIPE 信号。<br>写进程在写 Socket 的时候，读经常已经终止。 |
| 14 | SIGALRM | 进程终止 | 时钟定时信号，计算的是实际时间或时钟时间。`alarm`函数使用该信号。 |
| 15 | SIGTERM | 进程终止 | 程序结束信号，可以被阻塞和处理。<br>用来要求程序正常退出，shell kill 默认使用该命令。 |
| 16 | ... | 进程终止 | 堆栈错误。 |
| 17 | SIGCHLD | 忽略 | 子进程结束时，父进程会收到该信号。<br>如果父进程没有处理这个信号，也没有等待子进程，子进程虽然终止，但是还会在内存进程表中占有表项，这时子进程称为僵尸进程。 |
| 18 | SIGCONT | 忽略 | 让一个停止的进程继续执行，不能被阻塞，可以被处理。 |
| 19 | SIGSTOP | 进程暂停 | 停止进程的执行。程序还未结束，只是暂停执行，不能被阻塞、处理、忽略。 |
| 20 | SIGTSTP | 进程暂停 | 停止进程的运行，可以被处理和忽略。SUSP字符，`Ctrl+z`发出。 |
| 21 | SIGTTIN | 进程暂停 | 后台作业要从用户终端读取数据时，作业中所有进程会收到该信号。<br>缺省时这些进程会停止执行。 |
| 22 | SIGTTOU | 进程暂停 | 类似 SIGTTIN ，但在写终端或修改终端模式时收到。 |
| 23 | SIGURG | 忽略 | 有紧急数据或 out-of-band 数据到达 Socket 时产生。 |
| 24 | SIGXCPU | 进程意外结束（Dump） | 超过 CPU 时间资源限制。<br>这个限制可以由 getrlimit/setrlimit 来读取/改变。 |
| 25 | SIGXFSZ | 进程意外结束（Dump） | 进程企图扩大文件以至于超过文件大小资源限制。 |
| 26 | SIGVTALRM | 进程终止 | 虚拟时钟信号，类似于 SIGALRM ，但是计算的是该进程占用的 CPU 时间。 |
| 27 | SIGPROF | 进程终止 | 类似于 SIGALRM / SIGVTALRM ，但是包括该进程的 CPU 时间以及系统调用时间。 |
| 28 | SIGWINCH | 忽略 | 窗口大小改变是发出。 |
| 29 | SIGIO | 进程暂停 | 文件描述符准备就绪，可以进行 输入 / 输出操作。 |
| 30 | SIGPWR | 进程暂停 | Power failure |
| 31 | SIGSYS | 进程暂停 | 非法的系统调用。 |
| 32~64 | ... | 用户自定义 |

### 信号归类
* 程序不可捕获、阻塞、忽略：
`SIGKILL`，`SIGSTOP`。
* 不能恢复至默认动作：
`SIGILL`，`SIGTRAP`。
* 默认会导致进程流产：
`SIGABRT`，`SIGBUS`，`SIGFPE`，`SIGILL`，`SIGIOT`，`SIGQUIT`，`SIGSEGV`，`SIGTRAP`，`SIGXCPU`，`SIGXFSZ`。
* 默认会导致进程退出：
`SIGALRM`，`SIGHUP`，`SIGINT`，`SIGKILL`，`SIGPIPE`，`SIGPOLL`，`SIGPROF`，`SIGSYS`，`SIGTERM`，`SIGUSR1`，`SIGUSR2`，`SIGVIALRM`。
* 默认会导致进程停止：
`SIGSTOP``SIGTSTP``SIGTTIN``SIGTTOU`。
* 默认被进程忽略：
`SIGCHLD`，`SIGPWR`，`SIGURG`，`SIGWINCH`。

### 信号机制
内核给一个进程发送软中断信号的方法是在进程所在的进程表项的信号域设置对应于该信号的位。如果进程睡眠在可被终端的优先级上，则唤醒进程，否则置位，不唤醒。进程检查是否收到信号的时机是：一个进程在即将从内核态返回到用户态时，或者进程要进入或离开一个适当的低调度优先级睡眠状态。
内核处理一个进程收到的信号的时机是在一个进程从内核态返回用户态时。当一个进程在内核状态下运行时，软中断信号并不立即起作用，要等到将返回用户态时才处理。进程只有处理完信号才会返回用户态，进程在用户态下不会有未处理完的信号。
内核处理一个进程收到的信号的时机是在该进程的上下文中，因此，进程必须处于运行状态。（三种信号：退出、忽略、执行。）当进程收到一个它忽略的信号时，进程丢弃该信号，就像没收到信号似的继续运行。如果进程收到一个要捕获的信号，那么进程在从内核态返回到用户态时执行用户定义的函数。执行用户定义的函数时，内核在用户栈上创建一个新的层，该层中将返回地址的值设置成用户定义的函数的地址，这样进程从内核返回弹出栈顶是就返回到用户定义的函数处，从函数返回再弹出栈顶时，才返回原先进入内核的地方。这样做的原因是用户定义的函数不能且不允许在内核态下执行，否则用户可以获得任何权限。

### 相关系统调用
__signal 系统调用__
系统调用`signal`来设定某个信号的处理方法。声明如下：
void (*signal(int signum, void (*handler)(int)))(int);
signum: 指出要设置方法的信号。
handler: 指定一个处理函数或是`SIG_IGN`、`SIG_DFL`。
* SIG_IGN:忽略参数 signum 所指的信号。
* SIG_DFL:恢复参数 signum 所指的信号的处理方法为默认值。

传递给信号处理函数的参数是信号值，这样使得一个信号处理函数可以处理多个信号。返回值是信号 signum 上一个处理函数或者错误代码 SIG_ERR （出错时）。
__kill 系统调用__
系统调用`kill`用来向进程发送一个信号。声明如下：
int kill(pid_t pid, int sig);
如果参数 pid 大于 0 ，该调用将信号 sig 发送的进程号为 pid 的进程。
如果参数 pid 等于 0 ，那么信号 sig 将被发送给当前进程所属的进程组里的所有进程。
如果参数 pid 等于 -1 ，那么信号 sig 将被发送给除了进程 1 和自身以外的所有进程。
如果参数 pid 小于 -1 ，那么信号 sig 将被发送给属于进程组 -pid 的所有进程。
如果参数 sig 等于 0 ，将不发送信号。
该调用执行成功时，返回值为 0；出错时，返回值为 -1，并设置相应的错误 代码 errno：
* EINVAL: 指定的信号 sig 无效。
* ESRCH: 参数 pid 指定的进程或进程组不存在。（进程表项中可能存在僵尸进程）
* EPERM: 进程没有权限将信号发送到指定的进程。

__pause 系统调用__
系统调用`pause`的作用是等待一个信号。
int pause(void);
该调用使得发出调用的进程进入睡眠，直到收到一个信号为止。该调用总是返回 -1，并设置错误代码为 EINTR（接收到一个信号）。

__alarm 和 setitimer 系统调用__
系统调用`alarm`的功能是设置一个定时器，当定时器到达时，将发出一个信号给进程。声明如下：
unsigned int alarm(unsigned int seconds);
系统调用`alarm`安排内核为调用进程在指定的 seconds 秒后发出一个`SIGALRM`的信号。如果指定的参数 seconds 为 0，则不再发生`SIGALRM`的信号。后一次的设定将取消前一次的设定。返回值为上次定时调用到发送之间剩余的时间，或者因为没有前一次调用而返回 0。
_注意：在使用时，alarm 只设定为发送一次信号，如果要多次发送，就要多次调用 alarm。_
`setitimer`用来设置定时器，`getitimer`用来得到定时器的状态，生命如下：
int getitimer(int which, struct itimerval \*value);
int setitimer(int which, const struct itimerval \*new_value, struct itimerval \*old_value);
该系统调用给进程提供三个定时器，它们各自有其独有的计时域，当其中任何一个到达，就发送一个相应的信号给进程，并使得计时器重新开始。三个计时器由 which 指定，如下所示：
ITIMER_REAL: 按实际时间计时，计时到达将给进程发送 SIGALRM 信号。
ITIMER_VIRTUAL: 仅当进程执行时才进行计时。计时到达将发送 SIGVTALRM 信号给进程。
ITIMER+PROF: 当进程执行时和系统为该进程执行动作时都计时。计时到达将发送 SIGPROF 信号给进程。
定时器参数 value 用来指明定时器的时间，结构如下：
struct itimerval
{
    struct timeval it_interval;// 下一次的取值
    struct timeval it_value;// 本次的设定值
}
该结构中 timeval 结构定义如下：
struct timeval
{
    long tv_sec;// 秒
    long tv_usec;// 微妙，1 秒 = 1000000 微秒
}
在`setitimer`调用中，参数 old_value 如果不为空，则其中保留的是上次调用设定的值。定时器将 it_value 递减到 0 时，产生一个信号，并将 it_value 的值设定为 it_interval 的值，然后重新计时，如此往复。当 it_value 设定为 0 时，计时器停止，或者当它计时到期，而 it_interval 为 0 时停止。
调用成功时，返回 0，错误是返回 -1，并设置相应的错误代码 errno：
EFAULT: 参数 value 或 ovalue 是无效的指针。
EINVAL: 参数 which 不是`ITIMER_REAL`、`ITIMER_VIRT`或`ITIMER_PROF`中的一个。

``` cpp
#include <signal.h>
#include <unistd.h>
#include <stdio.h>
#include <sys/time.h>

void sigRoutine(int sigNum)
{
    switch(sigNum)
    {
        case SIGALRM:
            printf("Catch a signal -- SIGALRM:%d\n",sigNum);
            break;
        case SIGVTALRM:
            printf("Catch a signal -- SIGVTALRM:%d\n",sigNum);
            break;
        default:
            printf("Receive signal:%d\n", sigNum);
    }
}

int main()
{
    struct itimerval value,ovalue,value2;

    printf("process id is %d\n", getpid());

    signal(SIGALRM,sigRoutine);
    signal(SIGVTALRM,sigRoutine);

    value.it_value.tv_sec = 1;
    value.it_value.tv_usec = 0;
    value.it_interval.tv_sec = 1;
    value.it_interval.tv_usec = 0;
    setitimer(ITIMER_REAL, &value, &ovalue);

    value2.it_value.tv_sec = 0;
    value2.it_value.tv_usec = 500000;
    value2.it_interval.tv_sec = 0;
    value2.it_interval.tv_usec = 500000;
    setitimer(ITIMER_VIRTUAL, &value2, &ovalue);

    for(;;)
    {
    }

    return 0;
}

```
