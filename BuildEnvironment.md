## 第一章实验：基于VirtualBox的网络攻防基础环境搭建

### 实验目的

- 掌握VirtualBox虚拟机的安装与使用；
- 掌握VirtualBox的虚拟网络类型和按需配置；
- 掌握VirtualBox的虚拟硬盘多重加载。

### 实验环境

- VirtualBox虚拟机
- 攻击者主机（Attacker）：Kali Rolling 2109.2
- 网关（Gateway）：Debian Buster
- 靶机（Victim）：From Sqli to shell / xp-sp3 / Kali

### 实验步骤详述

第一步：安装VirtualBox虚拟机

- 进入VirtualBox虚拟机官网，网址：https://www.virtualbox.org/
- 点击下载VirtualBox 6.0版，这时会跳转网页；
- 因为我使用的操作系统是Windows，所以选择Windows hosts，保存文件VirtualBox-6.0.12-133076-Win.exe到合适的位置，我存放的文件位置是D盘；
- 下载好后，启动该可执行文件，按照步骤安装即可。

第二步：下载Kali Rolling并检验kali的校验和

- 进入Kali官网，网址：https://www.kali.org/
- 点击downloads，选择kali linux 64-bit的torrent文件（最好不要直接下载kali linux 64-bit.iso，因为校验和在torrent文件里，校验和可以用来验证镜像文件的完整性），然后下载该种子文件。
- 下载好种子文件后，打开种子文件的文件夹，里面有两个文件，一个是光盘映像文件，另一个是SHA256SUM文件，用后者来做校验和。
- 在Windows系统下，自带certutil -hashfile  <文件名>  <hash类型>命令行进行校验。

第三步：创建kali系统的虚拟机

- 打开VirtualBox，新建虚拟电脑，类型选择linux，版本64位即可（因为我的电脑是64位）。
- 内存设置为2048MB，然后一直选默认配置，直到选择文件位置和大小为止。
- 文件位置随意，大小需要大一些，我设为50GB。
- 然后启动kali虚拟机，

###### （未完待续......）
