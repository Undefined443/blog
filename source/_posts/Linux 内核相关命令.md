Shell 命令：

```sh
ipcs  # 查看共享内存

dmesg          # 显示内核消息
sudo dmesg -c  # 清空内核消息

sudo mknod /dev/rwbuf c 60 0

sudo insmod rwbuf.ko  # 加载内核模块
sudo rmmod rwbuf.ko   # 卸载内核模块
lsmod                 # 显示已加载的内核模块
modinfo module_name   # 显示模块信息
```

内核编程：

```c
perror("In test.c")  // 根据错误码显示错误信息。如：In test.c: No such file or directory
printk("My name is %s", name);  // 内核中的 printf，打印的信息可以用 dmesg 查看
```