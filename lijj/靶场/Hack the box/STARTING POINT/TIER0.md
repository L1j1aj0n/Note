# FAWN

## task 1

> What does the 3-letter acronym FTP stand for? 
>
> 3 个字母的首字母缩写词 FTP 代表什么？
>
> File Transfer Protocol 



## task2

>Which port does the FTP service listen on usually? 
>
>FTP监听哪个端口？
>
>21

## task 3

> What acronym is used for the secure version of FTP? 
>
> FTP的安全版本使用什么首字母缩略词？
>
> sFTP
>
> s为secure 中文译为;安全文件传输协议

## task 4

> From your scans, what version is FTP running on the target? 
>
> 我们可以使用什么命令来发送 ICMP 回显请求以测试我们与目标的连接？
>
> ping

## task 5

> From your scans, what version is FTP running on the target? 
>
> **根据您的扫描，目标上运行的 FTP 版本是什么？**
>
> 使用nmap -v扫描可看到FTP的版本
>
> vsftpd 3.0.3

## task 6

> From your scans, what OS type is running on the target?
>
> **通过你的扫描，靶机上的操作系统是什么**
>
> 同样是使用namp-v可以看见目标的操作系统是UNIX

## task 7

> What is the command we need to run in order to display the ‘ftp’ client help menu?
>
> 为了显示“ftp”客户端帮助菜单，我们需要运行什么命令？
>
> ftp-h

## task 8

> What is username that is used over FTP when you want to log in without having an account?
>
> 当您想在没有帐户的情况下登录时，FTP上使用的用户名是什么？
>
> anonymous

## task 9

> What is the response code we get for the FTP message ‘Login successful’?
>
> FTP消息“登录成功”的响应代码是什么？
>
> 使用ftp 10.129.33.194登录anonymous用户返回的响应代码是230

## task10

> There are a couple of commands we can use to list the files and directories available on the FTP server. One is dir. What is the other that is a common way to list files on a Linux system.
> 有几个命令可以用来列出FTP服务器上可用的文件和目录。一个是dir。另一个是什么，这是在Linux系统上列出文件的常用方法。
>
> ls

## task 11

> What is the command used to download the file we found on the FTP server?
>
> 用于下载我们在FTP服务器上找到的文件的命令是什么？
>
> `get`

## task 12

> Submit root flag
>
> 登录上去后使用ls发现存在flag.txt再使用get 获取到本地

# DANCING

## task1

What does the 3-letter acronym SMB stand for? 

3 个字母的首字母缩写词 SMB 代表什么？

> SMB即Server Message Block（服务器消息块），是一种文件共享协议。当文件原件在你的A电脑上，而你想在局域网下用你的手机、iPad或是另一台电脑来访问A电脑上的该文件时，你可能需要用到SMB共享。

## task2

What port does SMB use to operate at? 

SMB 使用什么端口进行操作？

```┌──(root㉿kali)-[~]
──(root㉿kali)-[~]
└─# nmap -sV 10.129.53.157
Starting Nmap 7.92 ( https://nmap.org ) at 2022-11-26 02:49 EST
Nmap scan report for 10.129.53.157
Host is up (0.52s latency).
Not shown: 997 closed tcp ports (reset)
PORT    STATE SERVICE       VERSION
135/tcp open  msrpc         Microsoft Windows RPC
139/tcp open  netbios-ssn   Microsoft Windows netbios-ssn
445/tcp open  microsoft-ds?
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 47.05 seconds
```

> SMB（服务器消息块）协议的一项核心任务是文件共享。在 Windows NT 中，它运行在 NBT（TCP/IP 上的 NetBIOS）之上，它使用著名的端口UDP 137 和 138以及TCP 139。在Windows 2000中，Microsoft添加了直接通过TCP/IP运行SMB的选项（TCP端口445），而没有额外的NBT层。
> **简言之：**有两种SMB，TCP端口139是NETBIOS（网络基本输入/输出系统协议）上的SMB。而TCP 445是基于TCP/IP的SMB，是较新的版本的SMB。

答案:445

## task 3

What is the service name for port 445 that came up in our Nmap scan? 

在Nmap扫描中出现的端口445的服务名称是什么?

上一题可以看到服务名称是 microsoft-ds

## task 4

What is the 'flag' or 'switch' we can use with the SMB tool to 'list' the contents of the share?

SMB工具可用于“列出”共享内容的“标志”或“开关”是什么?

在 linux 中，可以使用命令行工具smbclient对目标机器进行SMB服务的连接，smbclient将尝试连接到远程主机，并检查是否需要任何身份验证，使用命令：

```powershell
┌──(root㉿kali)-[~]
└─# smbclient -h   

Invalid option -h: unknown option

Usage: smbclient [-?EgqBNPkV] [-?|--help] [--usage] [-M|--message=HOST] [-I|--ip-address=IP]
        [-E|--stderr] [-L|--list=HOST] [-T|--tar=<c|x>IXFvgbNan] [-D|--directory=DIR]
        [-c|--command=STRING] [-b|--send-buffer=BYTES] [-t|--timeout=SECONDS] [-p|--port=PORT]
        [-g|--grepable] [-q|--quiet] [-B|--browse] [-d|--debuglevel=DEBUGLEVEL]
        [--debug-stdout] [-s|--configfile=CONFIGFILE] [--option=name=value]
        [-l|--log-basename=LOGFILEBASE] [--leak-report] [--leak-report-full]
        [-R|--name-resolve=NAME-RESOLVE-ORDER] [-O|--socket-options=SOCKETOPTIONS]
        [-m|--max-protocol=MAXPROTOCOL] [-n|--netbiosname=NETBIOSNAME] [--netbios-scope=SCOPE]
        [-W|--workgroup=WORKGROUP] [--realm=REALM] [-U|--user=[DOMAIN/]USERNAME[%PASSWORD]]
        [-N|--no-pass] [--password=STRING] [--pw-nt-hash] [-A|--authentication-file=FILE]
        [-P|--machine-pass] [--simple-bind-dn=DN] [--use-kerberos=desired|required|off]
        [--use-krb5-ccache=CCACHE] [--use-winbind-ccache] [--client-protection=sign|encrypt|off]
        [-k|--kerberos] [-V|--version] [OPTIONS] service <password>

```

使用-L显示服务器端分享的

## task 5

How many shares are there on Dancing? 

几个？

```powershell
┌──(root㉿kali)-[~]
└─# smbclient -L 10.129.53.157
Password for [WORKGROUP\root]:

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        WorkShares      Disk      
Reconnecting with SMB1 for workgroup listing.
do_connect: Connection to 10.129.53.157 failed (Error NT_STATUS_RESOURCE_NAME_NOT_FOUND)
Unable to connect with SMB1 -- no workgroup available
```

## task 6

What is the name of the share we are able to access in the end with a blank password? 

最后我们可以使用空白密码访问的共享名称是什么？

在列出的用户里，后面有美元符号$的都是管理员权限的，所以我们只能连接WorkShares这个用户（除了WorkShares之外，其他三个都是默认共享的，可以用自己物理机输入命令net share就知道了）。
此题答案为：WorkShares

## task7

What is the command we can use within the SMB shell to download the files we find? 

我们可以在 SMB shell 中使用什么命令来下载我们找到的文件？

get

## task8

这里有个语法，使用`smbclient \\\\IP\\用户`进行连接，密码为空直接回车。连接进去之后，有两个用户，都看了看，James.P里有flag.txt，使用get命令下载到本地，再使用cat命令查看即可。
具体使用如下：

```powershell
┌──(root㉿kali)-[~]
└─# smbclient \\\\10.129.53.157\\workshares
Password for [WORKGROUP\root]:
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Mon Mar 29 04:22:01 2021
  ..                                  D        0  Mon Mar 29 04:22:01 2021
  Amy.J                               D        0  Mon Mar 29 05:08:24 2021
  James.P                             D        0  Thu Jun  3 04:38:03 2021

                5114111 blocks of size 4096. 1752070 blocks available
smb: \> cd James.P
smb: \James.P\> ls
  .                                   D        0  Thu Jun  3 04:38:03 2021
  ..                                  D        0  Thu Jun  3 04:38:03 2021
  flag.txt                            A       32  Mon Mar 29 05:26:57 2021

                5114111 blocks of size 4096. 1752054 blocks available
smb: \James.P\> get flag.txt
getting file \James.P\flag.txt of size 32 as flag.txt (0.0 KiloBytes/sec) (average 0.0 KiloBytes/sec)
smb: \James.P\> exit
                                                                                  
┌──(root㉿kali)-[~]
└─# cat flag.txt
5f61c10dffbc77a704d76016a22f1664      
```

# Redeemer

## task1 

Which TCP port is open on the machine? 

TCP的端口号

```powershell
┌──(root㉿kali)-[~]
└─# nmap 10.129.79.203 -sS -T 4 -p 6000-7000
Starting Nmap 7.92 ( https://nmap.org ) at 2022-11-26 03:39 EST
Nmap scan report for 10.129.79.203
Host is up (3.6s latency).
Not shown: 941 closed tcp ports (reset), 59 filtered tcp ports (no-response)
PORT     STATE SERVICE
6379/tcp open  redis

Nmap done: 1 IP address (1 host up) scanned in 7.53 seconds

```

使用端口扫描 6000到7000端口

或者使用`nmap -p- -sV`扫描全端口，非常慢，没搞

## task2

Which service is running on the port that is open on the machine?

哪个服务正在计算机上打开的端口上运行？

redis

## task3

What type of database is Redis? Choose from the following options: (i) In-memory Database, (ii) Traditional Database？

Redis是什么类型的数据库？从以下选项中选择：（i）内存数据库，（ii）传统数据库

In-memory 

## task4

Which command-line utility is used to interact with the Redis server? Enter the program name you would enter into the terminal without any arguments. 

哪个命令行实用程序用于与Redis服务器交互？输入要在终端中输入的程序名，不带任何参数。

redis-cli

## task5

Which flag is used with the Redis command-line utility to specify the hostname? 

Redis命令行实用程序使用哪个标志来指定主机名？

输入redis-cli --help

```powershell
┌──(root㉿kali)-[~]
└─# redis-cli --help
redis-cli 7.0.5

Usage: redis-cli [OPTIONS] [cmd [arg [arg ...]]]
  -h <hostname>      Server hostname (default: 127.0.0.1).
  -p <port>          Server port (default: 6379).
  -s <socket>        Server socket (overrides hostname and port).
  -a <password>      Password to use when connecting to the server.
                     You can also use the REDISCLI_AUTH environment
                     variable to pass this password more safely
                     (if both are used, this argument takes precedence).
  --user <username>  Used to send ACL style 'AUTH username pass'. Needs -a.
  --pass <password>  Alias of -a for consistency with the new --user option.
  --askpass          Force user to input password with mask from STDIN.
                     If this argument is used, '-a' and REDISCLI_AUTH
                     environment variable will be ignored.

```

可以看到 是 -h

## task 6

Once connected to a Redis server, which command is used to obtain the information and statistics about the Redis server? 

连接到Redis服务器后，使用哪个命令获取有关Redis服务器的信息和统计信息？

```powershell
┌──(root㉿kali)-[~]
└─# redis-cli -h 10.129.79.203
10.129.79.203:6379> info
# Server
redis_version:5.0.7
redis_git_sha1:00000000
redis_git_dirty:0
redis_build_id:66bd629f924ac924
redis_mode:standalone
os:Linux 5.4.0-77-generic x86_64
arch_bits:64
multiplexing_api:epoll
atomicvar_api:atomic-builtin
gcc_version:9.3.0
process_id:751
run_id:690f1c1a1efa255b820525b02114f27a980b1aa2
tcp_port:6379
uptime_in_seconds:1722

```

答案：info

## task 7

What is the version of the Redis server being used on the target machine?

版本号是多少

info的第一排 5.0.7

## task 8

Which command is used to select the desired database in Redis? 

哪个命令用于选择想要的数据库

select

## task9

How many keys are present inside the database with index 0? 

0库里有多少个关键字

使用`keys *`可查看

4个

## task 10

直接get flag 
