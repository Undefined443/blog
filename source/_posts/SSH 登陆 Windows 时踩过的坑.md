有一次处于某些原因我在 Mac 上使用 SSH 远程登陆了 Windows，然后在 Windows 上使用 SSH 登陆 localhost，惊讶地发现登不进去！SSH 提示公钥验证失败。可是我的 Windows 使用的私钥和 Mac 是一样的，并且以前在 Windows 上也一直可以登陆 localhost，为什么今天突然不行了呢？

```sh
$ ssh localhost
myuser@localhost: Permission denied (publickey).
```

> 我关闭了密码登录，因此没有提示我输密码就直接拒绝访问

抱着百思不得姐的心情我开始了 debug。

我首先使用 `ssh -v` 命令查看连接过程中的日志：

```sh
$ ssh localhost -v
```

```log
OpenSSH_for_Windows_8.6p1, LibreSSL 3.4.3
debug1: Reading configuration data C:\\Users\\MyUser/.ssh/config
debug1: Authenticator provider $SSH_SK_PROVIDER did not resolve; disabling
debug1: Connecting to localhost [::1] port 22.
debug1: Connection established.
debug1: identity file C:\\Users\\MyUser/.ssh/id_rsa type 0
debug1: identity file C:\\Users\\MyUser/.ssh/id_rsa-cert type -1
debug1: identity file C:\\Users\\MyUser/.ssh/id_dsa type -1
debug1: identity file C:\\Users\\MyUser/.ssh/id_dsa-cert type -1
debug1: identity file C:\\Users\\MyUser/.ssh/id_ecdsa type -1
debug1: identity file C:\\Users\\MyUser/.ssh/id_ecdsa-cert type -1
debug1: identity file C:\\Users\\MyUser/.ssh/id_ecdsa_sk type -1
debug1: identity file C:\\Users\\MyUser/.ssh/id_ecdsa_sk-cert type -1
debug1: identity file C:\\Users\\MyUser/.ssh/id_ed25519 type -1
debug1: identity file C:\\Users\\MyUser/.ssh/id_ed25519-cert type -1
debug1: identity file C:\\Users\\MyUser/.ssh/id_ed25519_sk type -1
debug1: identity file C:\\Users\\MyUser/.ssh/id_ed25519_sk-cert type -1
debug1: identity file C:\\Users\\MyUser/.ssh/id_xmss type -1
debug1: identity file C:\\Users\\MyUser/.ssh/id_xmss-cert type -1
debug1: Local version string SSH-2.0-OpenSSH_for_Windows_8.6
debug1: Remote protocol version 2.0, remote software version OpenSSH_8.9p1 Ubuntu-3ubuntu0.7
debug1: compat_banner: match: OpenSSH_8.9p1 Ubuntu-3ubuntu0.7 pat OpenSSH* compat 0x04000000
debug1: Authenticating to localhost:22 as 'windows11'
debug1: load_hostkeys: fopen C:\\Users\\MyUser/.ssh/known_hosts2: No such file or directory
debug1: load_hostkeys: fopen __PROGRAMDATA__\\ssh/ssh_known_hosts: No such file or directory
debug1: load_hostkeys: fopen __PROGRAMDATA__\\ssh/ssh_known_hosts2: No such file or directory
debug1: SSH2_MSG_KEXINIT sent
debug1: SSH2_MSG_KEXINIT received
debug1: kex: algorithm: curve25519-sha256
debug1: kex: host key algorithm: ssh-ed25519
debug1: kex: server->client cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none
debug1: kex: client->server cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none
debug1: expecting SSH2_MSG_KEX_ECDH_REPLY
debug1: SSH2_MSG_KEX_ECDH_REPLY received
debug1: Server host key: ssh-ed25519 SHA256:SWsZo54/ljQNSNSieY+AyQ27sbR31qWnyCbv7hzkarg
debug1: load_hostkeys: fopen C:\\Users\\MyUser/.ssh/known_hosts2: No such file or directory
debug1: load_hostkeys: fopen __PROGRAMDATA__\\ssh/ssh_known_hosts: No such file or directory
debug1: load_hostkeys: fopen __PROGRAMDATA__\\ssh/ssh_known_hosts2: No such file or directory
debug1: Host 'localhost' is known and matches the ED25519 host key.
debug1: Found key in C:\\Users\\MyUser/.ssh/known_hosts:1
debug1: rekey out after 134217728 blocks
debug1: SSH2_MSG_NEWKEYS sent
debug1: expecting SSH2_MSG_NEWKEYS
debug1: SSH2_MSG_NEWKEYS received
debug1: rekey in after 134217728 blocks
debug1: pubkey_prepare: ssh_get_authentication_socket: No such file or directory
debug1: Will attempt key: C:\\Users\\MyUser/.ssh/id_rsa RSA SHA256:Mqzkt00T7hxTv3hB2xbvKm7fron2ScNtlaNRbtuntvk
debug1: Will attempt key: C:\\Users\\MyUser/.ssh/id_dsa
debug1: Will attempt key: C:\\Users\\MyUser/.ssh/id_ecdsa
debug1: Will attempt key: C:\\Users\\MyUser/.ssh/id_ecdsa_sk
debug1: Will attempt key: C:\\Users\\MyUser/.ssh/id_ed25519
debug1: Will attempt key: C:\\Users\\MyUser/.ssh/id_ed25519_sk
debug1: Will attempt key: C:\\Users\\MyUser/.ssh/id_xmss
debug1: SSH2_MSG_EXT_INFO received
debug1: kex_input_ext_info: server-sig-algs=<ssh-ed25519,sk-ssh-ed25519@openssh.com,ssh-rsa,rsa-sha2-256,rsa-sha2-512,ssh-dss,ecdsa-sha2-nistp256,ecdsa-sha2-nistp384,ecdsa-sha2-nistp521,sk-ecdsa-sha2-nistp256@openssh.com,webauthn-sk-ecdsa-sha2-nistp256@openssh.com>   
debug1: kex_input_ext_info: publickey-hostbound@openssh.com (unrecognised)
debug1: SSH2_MSG_SERVICE_ACCEPT received
debug1: Authentications that can continue: publickey
debug1: Next authentication method: publickey
debug1: Offering public key: C:\\Users\\MyUser/.ssh/id_rsa RSA SHA256:Mqzkt00T7hxTv3hB2xbvKm7fron2ScNtlaNRbtuntvk
debug1: Authentications that can continue: publickey
debug1: Trying private key: C:\\Users\\MyUser/.ssh/id_dsa
debug1: Trying private key: C:\\Users\\MyUser/.ssh/id_ecdsa
debug1: Trying private key: C:\\Users\\MyUser/.ssh/id_ecdsa_sk
debug1: Trying private key: C:\\Users\\MyUser/.ssh/id_ed25519
debug1: Trying private key: C:\\Users\\MyUser/.ssh/id_ed25519_sk
debug1: Trying private key: C:\\Users\\MyUser/.ssh/id_xmss
debug1: No more authentication methods to try.
MyUser@localhost: Permission denied (publickey).
```

在日志中我发现了这样一行记录：

```log
debug1: Remote protocol version 2.0, remote software version OpenSSH_8.9p1 Ubuntu-3ubuntu0.7
```

奇怪，我在 Windows 上连接 localhost，怎么连到一个 Ubuntu 版本的 OpenSSH 上了？

我立马想到了 WSL。

于是我使用 `wsl --shutdown` 命令关闭了所有 WSL 实例，再次尝试连接。

SSH 再次提示我远程主机的指纹变了，删除 `known_hosts` 后再次尝试，连接成功：

```sh
$ wsl --shutdown
$ ssh localhost
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@    WARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!     @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
IT IS POSSIBLE THAT SOMEONE IS DOING SOMETHING NASTY!
Someone could be eavesdropping on you right now (man-in-the-middle attack)!
It is also possible that a host key has just been changed.
The fingerprint for the ED25519 key sent by the remote host is
SHA256:DHrLaqaBwTvslmpHDDUMma3P62c9xQrqDNj+v/DFAww.
Please contact your system administrator.
Add correct host key in C:\\Users\\MyUser/.ssh/known_hosts to get rid of this message.
Offending ED25519 key in C:\\Users\\MyUser/.ssh/known_hosts:1
Host key for localhost has changed and you have requested strict checking.
Host key verification failed.
$ rm .\known_hosts
$ ssh localhost
The authenticity of host 'localhost (::1)' can't be established.
ED25519 key fingerprint is SHA256:DHrLaqaBwTvslmpHDDUMma3P62c9xQrqDNj+v/DFAww.
This key is not known by any other names
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'localhost' (ED25519) to the list of known hosts.
>
```

确实是 WSL 在捣鬼。不过刚刚关闭 WSL 的时候 Docker 也自动退出了。之前我在 WSL 上并没有设置过有关网络镜像的配置，为什么会把 Windows 宿主机上的 localhost 映射到 WSL Ubuntu 虚拟机上呢？不过我的 WSL Ubuntu 共用宿主机的 Docker Desktop，会不会和 Docker Desktop 有关呢？

于是我打开了 WSL，但是关闭了 Docker Desktop，再次尝试连接 localhost，成功连接。

此时我再打开 Docker Desktop，然后尝试连接 localhost，于是发生了和开头相同的一幕：SSH 提示我远程主机指纹发生变化，删除 `known_hosts` 文件后再次尝试连接，提示公钥验证失败。

这时我想起 WSL Ubuntu 上设置的用户名和 Windows 不同，于是我在 ssh 命令中加上了 Ubuntu 用户名，再次尝试连接，成功登陆 WSL Ubuntu。

目前尚不清楚 Docker 为什么会影响 WSL 的网络，我已经把 Docker 运行的所有容器都删了，依然会影响 WSL 的网络。