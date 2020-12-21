# linux 命令

## top

```bash
top - 09:44:56 up 16 days, 21:23,  1 user,  load average: 9.59, 4.75, 1.92
# 当前时间，运行时间，登录用户数，系统负载
Tasks: 145 total,   2 running, 143 sleeping,   0 stopped,   0 zombie
# 任务总数，运行中的任务，休眠任务，...
Cpu(s): 99.8%us,  0.1%sy,  0.0%ni,  0.2%id,  0.0%wa,  0.0%hi,  0.0%si,  0.0%st
# 用户空间占cpu百分比，内核空间占用cpu百分比，用户进程空间内改变过优先级的进程占用CPU百分比，id 空闲cpu百分比
Mem:   4147888k total,  2493092k used,  1654796k free,   158188k buffers
# 物理内存总量，使用的内存量，空闲内存量，用作内核缓存的内存量
Swap:  5144568k total,       56k used,  5144512k free,  2013180k cached
```

- c：切换显示命令名称和完整命令行；
- M：根据驻留内存大小进行排序；
- P：根据 CPU 使用百分比大小进行排序；
- T：根据时间/累计时间进行排序；

## netstat

查看网络状态的命令

```bash
# 找出运行在指定端口的进程：
netstat -an | grep ':80'
# 找出程序运行的端口
netstat -ap | grep ssh
```

## free

`free -m`,以 m 为单位查看内存使用情况

```bash
              total        used        free      shared  buff/cache   available
Mem:        3881692     3029704      166472         472      685516      594824
Swap:             0           0           0

# total = used + free + buff/cache
```

## df

查看磁盘分区上可使用的磁盘空间，默认显示 KB

如：`df -h`使用-h 选项以 KB 以上的单位来显示，可读性高

## vmstat

查看系统整体运行状态

```bash
procs -----------memory---------- ---swap-- -----io---- -system-- ------cpu-----
 r  b   swpd   free   buff  cache   si   so    bi    bo   in   cs us sy id wa st
 4  0      0 119388  30284 701764    0    0     1     6    0    0  1  0 99  0  0
```

## op


