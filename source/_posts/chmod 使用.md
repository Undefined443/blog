|  数字   | 权限 |
| :-----: | :--: |
| 4 (100) |  读  |
| 2 (010) |  写  |
| 1 (001) | 执行 |

![image](https://www.runoob.com/wp-content/uploads/2014/08/rwx-standard-unix-permission-bits.png)

`u` 表示该文件的拥有者，`g` 表示该文件的拥有者所属的组，`o` 表示其他人，`a` 表示所有人。

e.g.

```bash
# 数字表示法
chmod 777 file # 为所有用户开放 file 的全部权限
chmod 744 file # 只有拥有者有全部权限，其他人只读。

# 字母表示法
chmod o+w file # 为其他人增加写权限
chmod a+x file # 为所有人增加执行权限
chmod a-x file # 为所有人移除执行权限
chmod u=rwx,g=rx,o=r file
chmod u=rwx,og=rx file
```

### 查看文件权限

```sh
$ ls -l
total 16
drwxr-xr-x  13 p6  staff  416  4  8 11:00 Courses
drwxr-xr-x  10 p6  staff  320  4  8 15:37 Notes
-rw-rw-rw-@  2 p6  staff   28  4  8 15:41 file.txt
-rw-rw-rw-@  2 p6  staff   28  4  8 15:41 fileLink.txt
lrwxr-xr-x   1 p6  staff    8  4  8 15:42 fileSoftLink -> file.txt
```

- 访问目录必须拥有执行权限。
- 每行的第一个字母表示文件类型，`d` 表示目录，`-` 表示文件，`l` 表示链接文件，`b` 表示块设备文件，`c` 表示字符型设备文件，`s` 表示套接字文件，`p` 表示管道文件。
- 块设备文件的特点是程序员可以随机访问设备上的数据，比如磁盘。而字符型文件则只能顺序访问数据，比如键盘。

See also:

- [ls -l 输出内容详解 | 博客园](https://www.cnblogs.com/justmine/p/9053419.html)
- [Linux 文件权限查看及修改 | 博客园](https://www.cnblogs.com/cb0327/p/6189586.html)
- [「复制、拷贝、替身、软连接、硬连接」区别详解 | CSDN](https://blog.csdn.net/woodpeck/article/details/78761219)
