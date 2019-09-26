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

**第一步：安装VirtualBox虚拟机**

- 进入VirtualBox虚拟机官网，网址：https://www.virtualbox.org/
- 点击下载VirtualBox 6.0版，这时会跳转网页；
- 因为我使用的操作系统是Windows，所以选择Windows hosts，保存文件VirtualBox-6.0.12-133076-Win.exe到合适的位置，我存放的文件位置是D盘；
- 下载好后，启动该可执行文件，按照步骤安装即可。

**第二步：下载Kali Rolling并检验kali的校验和**

- 进入Kali官网，网址：https://www.kali.org/
- 点击downloads，选择kali linux 64-bit的torrent文件（最好不要直接下载kali linux 64-bit.iso，因为校验和在torrent文件里，校验和可以用来验证镜像文件的完整性），然后下载该种子文件，下载过程需要一些时间。
- 下载好种子文件后，打开种子文件的文件夹，里面有两个文件，一个是光盘映像文件，另一个是SHA256SUM文件，用后者来做校验和。
- 在Windows系统下，自带`certutil -hashfile  <文件名>  <hash类型>`命令行进行校验。
- 若校验失败，则删除文件，再次下载，一直到校验成功为止。

**第三步：创建kali系统的虚拟机kali-attacker**

- 打开VirtualBox，新建虚拟电脑，类型选择linux，版本64位即可（因为我的电脑是64位）。
- 内存设置为2048MB，然后一直选默认配置，直到选择文件位置和大小为止。
- 文件位置随意，大小需要大一些，我设为50GB。
- 然后启动kali虚拟机，要求选择启动盘，使用刚下载的kali镜像即可。
- 选择install，选择合适的语言和时区，等待配置完成。
- 之后出现任何选项都选择合适的配置（要写入磁盘分区，不需要网络镜像），不详细描述。

**第四步：安装kali-attacker的增强功能**

- 进入kali系统，点击工具栏的“设备”-“安装增强功能”。
- 按照提示操作即可。

**第五步：创建kali系统的虚拟机kali-victim，并配置多重加载**

- 点击新建，按照之前安装kali-attacker的步骤即可，在创建虚拟硬盘时要记得勾选“使用已有的虚拟硬盘文件”，然后就没有然后了。

- 打开kali-victim，查看到底有没有成功安装。

- 现在来设置多重加载，VirtualBox工具栏点击“管理”-“虚拟介质管理”，在“虚拟硬盘”选中kali-attacker.vdi，右键释放它，然后在下方“属性”栏修改“类型”为多重加载，点击“应用”。

- 修改kali-victim的设置，这时存储栏里的控制器应该没有任何vdi，添加kali-attacker.vdi到控制器里。

- 此时多重加载已经配置完成。

  <img src="D:\2019Term2\移动互联网安全and网络安全资料\第一章实验_多重加载.png" alt="第一章实验_多重加载" style="zoom:50%;" />

- 再次启动kali-victim，以查看是否能正常启动。

**第六步：创建两个windows xp系统的虚拟机xp-victim，并配置多重加载**

- 下载老师给的windows xp的vdi文件。

- 新建虚拟机，按照正常的安装系统步骤进行操作。

- 按照和kali多重加载一样的步骤配置即可。


**第七步：创建debian系统的虚拟机debian-victim的debian-gateway，并配置多重加载**

- 下载老师给的debian的vdi文件。
- 新建虚拟机并挂载上这个光盘映像文件。
- 按照给kali配置多重加载的方法配置debian的多重加载。

**第八步：搭建所有网络**

- 先全局设定一个NAT，然后将attacker和gateway都配置为NAT网络。

- 将kali-victim和winxp-victim-1配置为内部网络intnet-1。

- 将debian-victim和winxp-victim-2配置为内部网络intnet-2。

- 最后给gateway配置上intnet-1和intnet-2，如图所示：

  <img src="D:\2019Term2\移动互联网安全and网络安全资料\第一章实验_3个网卡.png" alt="第一章实验_3个网卡" style="zoom:40%;" />

**第九步：测试网络**

- [x] 靶机可以直接访问攻击者主机

  攻击者主机ip如图：

  <img src="D:\2019Term2\移动互联网安全and网络安全资料\第一章实验_攻击者主机ip.png" alt="第一章实验_攻击者主机ip" style="zoom:40%;" />

  用kali-victim来ping攻击者主机ip，发现**网络连接激活失败**。

  

- [ ] 攻击者主机无法直接访问靶机

- [x] 网关可以直接访问攻击者主机和靶机

  网关访问攻击者主机如图：

  <img src="D:\2019Term2\移动互联网安全and网络安全资料\第一章实验_网关访问攻击者.png" alt="第一章实验_网关访问攻击者" style="zoom:50%;" />

  

- [ ] 靶机的所有对外上下行流量必须经过网关

- [x] 所有节点均可以访问互联网

  攻击者主机访问互联网如下图所示：

  <img src="D:\2019Term2\移动互联网安全and网络安全资料\第一章实验_攻击者访问互联网.png" alt="第一章实验_攻击者访问互联网" style="zoom:33%;" />

  网关访问互联网：

  <img src="D:\2019Term2\移动互联网安全and网络安全资料\第一章实验_网关访问互联网.png" alt="第一章实验_网关访问互联网" style="zoom:33%;" />





### 参考资料

> kali安装指南：https://blog.csdn.net/JaydenWang5310/article/details/78104472
>
> 