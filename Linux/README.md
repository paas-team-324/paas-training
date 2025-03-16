# Linux
#### Estimated time: 6 Weeks
<br></br>

Author: Tomer Krausz Mallik
<br></br>

הערה: כדי לתרגל את הפרק הזה בצורה הטובה ביותר מומלץ(=חובה) לבצע את כל מה שלמדת על שרת חי ונושם.

להזמין שרת לינוקס זמני ולא להתאמן על שרת/מחשב אמיתי - מניסיון (רן ב.)
<br></br>

**Reading materials:** 
* [Linux journey (recommended)](https://linuxjourney.com/)
* [Linux courses (Sections 1, 2, 3, 5, 6, 7, 8, 9, 10, 12, 17)](https://www.tecmint.com/free-online-linux-learning-guide-for-beginners/)
<br></br>

## System Basics:
* Boot Process
* Init
* Kernel
* Kernel Space
* User Space
* System calls
* Interrupts
* Virtual Memory
* Pagefile
* Swap
* CPU Scheduler
* CPU Dispacher
* CPU Epoch Managment
* Context Switching
* File Descriptor
* File Descriptor Table
* stdin
* stdout
* stderr
* pipeline
* I/O Redirection
* Environment Variables
* /etc/sudoers - hands on
* /etc/passwd - hands on
* /etc/shadow - hands on

<br></br>
**Reading materials:** 
* [System calls](https://www.ionos.com/digitalguide/server/know-how/what-are-system-calls/)
* [What happens during system calls](https://linux-kernel-labs.github.io/refs/heads/master/lectures/syscalls.html)
* [Context switch](http://www.linfo.org/context_switch.html)
* [Virtual memory](https://elinux.org/images/4/4c/Ott.pdf)
* [File descriptors](https://www.bottomupcs.com/file_descriptors.xhtml)
* [CPU scheduling algorithms](https://www.guru99.com/cpu-scheduling-algorithms.html)
<br></br>
  
## Commands:
** Deliveries:** 
* sed
* awk
* grep
* cut
* cat
* find
* locate
* vim
* top
* ps
* cp
* mv
<br></br>
**Reading materials:** 
* [Vim](http://www.vimgenius.com/start)
<br></br>

## Procees Management:
* Process
* Process states
* Zombie process
* Process permissions
* Process priorities
* OOM Killer
* signals
* threads
* Fork & exec
* Copy on write
* Shared memory
* Inter process communications
* systemd 
* Service 
* Systemctl
* Service creation - hands on
* daemon
<br></br>
**Reading materials:** 
* [Process](https://www.tutorialspoint.com/operating_system/os_processes.htm)
* [Process Permissions](https://linuxjourney.com/lesson/process-permissions)
* [fork and exec](https://unix.stackexchange.com/questions/31118/why-is-the-default-process-creation-mechanism-fork)
* [Inter process communications](https://www.youtube.com/watch?v=BU9m45WWqjM)
* [how to create a service](https://www.shubhamdipt.com/blog/how-to-create-a-systemd-service-in-linux/)
<br></br>

## File System and Storage Management System:
* binary permissions
* file owner group
* chown - hands on
* chmod - hands on
* inode
* inode table
* inode structure
* metadata
* soft link
* hard link
* journaling
* Forms of data storage (File, Block, Object)
* NAS
* NFS - hands on
* SAN
* file systems - allocation schemes (XFS, EXT)
* S3
* LVM - hands on
* Raid
* tmpfs
* VFS
* /proc - hands on
* Partition - hands on
* mount - hands on
* /etc/fstab - hands on
* df - hands on
* du - hands on
* fdisk - hands on
* iostat - hands on
<br></br>
**Reading materials:** 
* [/proc](https://www.thegeekdiary.com/understanding-the-proc-file-system/)
* [permissions](http://haifux.org/lectures/84-sil/users-processes-files-and-permissions/users-perms-lec.html)
* [file permissions](https://docs.nersc.gov/filesystems/unix-file-permissions/)
* [linux filesystem](https://www.linux.com/training-tutorials/linux-filesystem-explained/)
* [inodes](https://www.howtogeek.com/465350/everything-you-ever-wanted-to-know-about-inodes-on-linux/)
* [creating tmpfs](https://docs.oracle.com/cd/E18752_01/html/817-5093/fscreate-99040.html)
* [FAT & NTFS](https://docs.microsoft.com/en-us/troubleshoot/windows-client/backup-and-storage/fat-hpfs-and-ntfs-file-systems)
* [EXT4 ](https://opensource.com/article/17/5/introduction-ext4-filesystem)
* [XFS](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/storage_administration_guide/ch-xfs)
* [creating a filesystem](https://www.thegeekdiary.com/how-to-create-an-xfs-filesystem/)
* [VFS](https://emmanuelbashorun.medium.com/linux-file-system-virtual-file-system-vfs-layer-part-3-79235c40a499)
* [Hard & Soft links ](https://medium.com/@meghamohan/hard-link-and-symbolic-link-3cad74e5b5dc#:~:text=Both%20the%20inodes%20and%20data,a%20disk%20partition%20is%20organized.&text=Symbolic%20link%20(Symlinks%2FSoft%20links,directory%20it%20is%20pointing%20to))
* [LVM](https://opensource.com/business/16/9/linux-users-guide-lvm)
* [Create partitions](https://phoenixnap.com/kb/linux-create-partition)
<br></br>

## Networking in Linux:
**Deliveries:** 
* NetworkManager
* nmcli (NetworkManager CLI) - hands on
* ethtool - hands on
* dmesg - hands on
* /etc/sysconfig/network
* /etc/sysconfig/network-scripts/
* /etc/sysconfig/network-scripts/ifcfg content
* Route tables - hands on
* tcpdump - hands on
* netstat - hands on
* ss - hands on
* /etc/resolv.conf
* /etc/hosts
* /etc/nsswitch.conf
* iptables
> * chains 
> * hooks
> * netfilter
> * tables
> * targets
* sockets
* unix sockets
* NTP
* Chronyd
<br></br>
**Reading materials:** 
* [NetworkManger](https://www.thegeekdiary.com/linux-os-service-networkmanager/)
* [NMCLI](https://www.golinuxcloud.com/nmcli-command-examples-cheatsheet-centos-rhel/)
* [/etc/sysconfig/network](https://docs.oracle.com/cd/E37670_01/E41138/html/ch10s02s04.html)
* [/etc/sysconfig/network-scripts/](https://docs.netapp.com/sgws-110/index.jsp?topic=%2Fcom.netapp.doc.sg-install-rhel%2FGUID-04DC1897-D5B4-4A43-BF6B-E8CB719BBA0D.html)
* [iptables](https://www.digitalocean.com/community/tutorials/a-deep-dive-into-iptables-and-netfilter-architecture)
* [more iptables](https://www.booleanworld.com/depth-guide-iptables-linux-firewall/)
* [IPVS](https://kubernetes.io/blog/2018/07/09/ipvs-based-in-cluster-load-balancing-deep-dive/)
* [tcpdump cheat sheet](https://cdn.comparitech.com/wp-content/uploads/2019/06/tcpdump-cheat-sheet.jpg)
* [routing](https://opensource.com/business/16/8/introduction-linux-network-routing)
* [unix sockets](https://www.cloudsavvyit.com/1263/what-are-unix-sockets-and-how-do-they-work/)
* [/etc/hosts](https://www.thegeekdiary.com/understanding-etc-hosts-file-in-linux/)
* [/etc/nsswitch.conf](http://linuxservertutorials.blogspot.com/2008/11/ubuntu-nsswitchconf-guide.html)
<br></br>

## Permissions Management:
**Deliveries:** 
* Linux Permissions
* SELINUX
* MAC
* DAC
* RBAC
* Type Enforcement
<br></br>
**Reading materials:** 
* [SELINUX Coloring Book](https://people.redhat.com/duffy/selinux/selinux-coloring-book_A4-Stapled.pdf)
* [SELINUX](https://sites.google.com/site/cacsolin/selinux)
* [SELINUX](https://wiki.centos.org/HowTos/SELinux)
* [MAC VS DAC](http://www.differencebetween.net/technology/software-technology/difference-between-mac-and-dac/#:~:text=DAC%20provides%20users%20who%20have,including%20them%20in%20the%20list.&text=A%20good%20example%20of%20a,systems%20is%20a%20good%20example.)
* [RBAC](https://auth0.com/docs/manage-users/access-control/rbac)
<br></br>

## Secured Networking:
**Deliveries:** 
*  SSH Keys
*  Certificates
*  OPENSSL - hands on
*  CSR
*  CA
<br></br>
** Reading materials:** 
* [SSH Keys](https://jumpcloud.com/blog/what-are-ssh-keys#:~:text=Essentially%2C%20SSH%20keys%20are%20an,to%20manage%20the%20remote%20system.)
<br></br>

## Others:
**Deliveries:** 
* RPM
* YUM
* /etc/yum.repos.d/
* Pxe boot
* Cron
* Ansible - hands on
* udev
<br></br>
**Reading materials:** 
* [Ansible](https://www.simplilearn.com/tutorials/ansible-tutorial/what-is-ansible?source=sl_frs_nav_playlist_video_clicked)
* [Getting started with Ansible](https://www.learnlinux.tv/getting-started-with-ansible/)
* [Cron](https://www.seobility.net/en/wiki/CronJob)
<br></br>

### Drills:
*  Lab
*  Estimated time: Day
<br></br>
*  Test
*  Estimated time: Day
