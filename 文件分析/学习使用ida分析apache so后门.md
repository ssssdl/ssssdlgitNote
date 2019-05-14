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
