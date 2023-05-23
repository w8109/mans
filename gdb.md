#### todo
```
break 25 thread 4
b func if arg1->foo().bar().c_str() == "xxxx"
interrupt
thread apply 4 bt
thread apply all continue
gdb --command=gdbcmd1 routine
```

#### CLI
CLI commands：Commnad Line Interface

#### MI
MI commands：Machine Interface。

#### async
```
set target-async on
```

#### permission
gdb need root


#### attach and detach
```
attach 
detach
```

#### enable non-Stop mode

in `~/.gdbinit` 
```
set target-async on
set pagination off
set non-stop on
```
但是一定要注意，如果是attch一个已经启动的程序，那么attach之后会把所有的线程stop。
因为是non-stop模式，所以continue只会恢复当前线程，此时需要continue -a来恢复所有线程。
然后ctrl -c 就可以stop当前线程。

#### remote debug
```
gdbserver --attach :4444 <pid>
gdb ceph-mds
target remote 192.168.0.11:4444
```    

#### break point
```
save breakpoints <filename>
break file.c:100 thread all
``` 

#### 符号
```
maint print symbol all.sym 
info functions C_ObjectOperation_decodevals    
set multiple-symbols
```

#### 当前指令位置
```
p $rip
```

#### 变量类型
```   
ptype
whatis
```

#### 查看类
```    
p *this
p *(C_MaybeExpiredSegment *)0x7f17d08d8560
```

#### 查看函数
```
p C_MaybeExpiredSegment::complete
```

#### 查看虚函数
```
p C_MaybeExpiredSegment::finish时报错
Cannot reference virtual member function "finish"
```

#### 查看虚函数表
```
info vtbl this
```

#### 反汇编函数
```    
disass /m Server::handle_client_readdir
```   
#### 显示汇编代码
```
set disassemble-next-line on
```

#### 设置汇编样式
```
set disassembly-flavor intel
```
    
#### 查看内存内容
```
x $rbp-0x18
```   

#### 单行汇编执行
```
si
```  

#### 网络相关
```
set tcp auto-retry on
```

#### 调度器
```
set scheduler-locking off|on|step
```    

#### 信号捕捉
```
info signals // 显示所有信号以及默认的gdb处理方式
info handle  // =  info signals
handle signal keywords
``` 

#### 指定程序启动参数
```
(gdb) file /opt/bin/ceph-mds
(gdb) set args -f --cluster ceph --id mds0 --setuser ceph --setgroup ceph
(gdb) run
```

#### 指定命令启动
```
gdb -p 1234 -ex 'break MOSDOpReply::decode_payload'
```

### python debug
```
tar xf centos_gdb_python_debug_mini.tgz -C debug
debug/install.sh
(gdb) thread apply all py-list
```

#### 为什么bt显示的某些栈帧看不到参数

一个软件的正式版本一般都是经过优化的，如果一个接口的参数，在调用这个接口之后不再使用，那么这个接口的参数一般会通过寄存器传入。
这个参数也就不会在栈帧中看到，除非是对内层的调用。所以如果碰到某个接口的参数无法显示，不用觉得奇怪。

#### pagination off 

如果命令到gdb输出的内容很多，默认情况下会分页，现在off就是关闭分页，等价于 set height unlimited 。

#### 多线程调试
```
(1) attach 之后 all-t 状态
(2) continue 之后 all-S 状态

设置 pagination off 之后
(1) attach 之后 all-t 状态
(2) continue 之后 all-S 状态

设置 target-async on 之后
(1) attach 之后 all-t 状态
(2) continue 之后 all-S 状态

设置 non-stop on 之后
(1) attach 之后 all-t 状态
(2) continue 之后 one-S 状态

设置 target-async on, non-stop on 之后
(1) attach 之后 all-t 状态
(2) continue 之后 one-S 状态
```

#### 只暂停多线程进程中的一个线程
```
attach <pid_of_thread>
```