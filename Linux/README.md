# Linux
<h4>Estimated time: 6 Weeks
<br></br>

Author: Tomer Krausz Mallik
<br></br>

הערה: כדי לתרגל את הפרק הזה בצורה הטובה ביותר מומלץ(=חובה) לבצע את כל מה שלמדת על שרת חי ונושם.

להזמין שרת לינוקס זמני ולא להתאמן על שרת/מחשב אמיתי - מניסיון (רן ב.)
<br></br>

** Reading materials:** 
* [](https://linuxjourney.com/ Linux journey (recommended)  
* [](https://www.tecmint.com/free-online-linux-learning-guide-for-beginners/ Linux courses (Sections 1, 2, 3, 5, 6, 7, 8, 9, 10, 12, 17)  
<br></br>

## Operating System:
### in depth:
*  Kernel
*  Kernel space
*  User space
*  Virtual memory 
*  Pagefile
*  Swap
*  Init
*  System calls
*  interrupts
*  File descriptor
*  File descriptor table
*  stdin
*  stdout
*  stderr
*  pipeline
*  i/o redirection
*  Environment variables
*  boot the system
  
### surface level:
*  Cpu dispacher
*  Non preemptive scheduling
*  preemptive scheduling
*  Schedualing algorithms (FCFS, SJF)
*  Context switching
*  /etc/sudoers - hands on
*  /etc/passwd - hands on
*  /etc/shadow - hands on

<br></br>
** Reading materials:** 
* [](https://www.ionos.com/digitalguide/server/know-how/what-are-system-calls/ System calls  
* [](https://linux-kernel-labs.github.io/refs/heads/master/lectures/syscalls.html What happens during system calls  
* [](http://www.linfo.org/context_switch.html Context switch  
* [](https://elinux.org/images/4/4c/Ott.pdf Virtual memory  
* [](https://www.bottomupcs.com/file_descriptors.xhtml File descriptors  
* [](https://www.guru99.com/cpu-scheduling-algorithms.html CPU scheduling algorithms 
<br></br>
  
## Commands:
** Deliveries:** 
*  sed
*  awk
*  grep
*  cut
*  cat
*  find
*  locate
*  vim
*  top
*  ps
<br></br>
** Reading materials:** 
* [](http://www.vimgenius.com/start Vim  
<br></br>

## Procees Management:
### in depth
*  Process
*  Process states
*  Zombie process
*  signals
*  threads
*  Fork & exec
*  Copy on write
*  Shared memory
*  Inter process communications
*  systemd 
*  Service 
*  Systemctl
  
### surface level  
*  Process permissions
*  Process priorities
*  OOM Killer
*  Service creation - hands on
*  daemon
<br></br>
** Reading materials:** 
* [](https://www.tutorialspoint.com/operating_system/os_processes.htm Process  
* [](https://linuxjourney.com/lesson/process-permissions Process Permissions  
* [](https://unix.stackexchange.com/questions/31118/why-is-the-default-process-creation-mechanism-fork fork and exec  
* [](https://www.youtube.com/watch?v=BU9m45WWqjM Inter process communications  
* [](https://www.shubhamdipt.com/blog/how-to-create-a-systemd-service-in-linux/ how to create a service  
<br></br>

## File System and Storage Management System:
### in depth 
*  /proc - hands on
*  binary permissions
*  inode
*  inode table
*  inode structure
*  metadata
*  soft link
*  hard link
*  LVM - hands on
*  Raid

### surface level
*  chown - hands on
*  file owner group
*  tmpfs
*  NAS
*  NFS - hands on
*  SAN
*  journaling
*  MBR
*  file systems - allocation schemes (XFS, NTFS, EXT, FAT)
*  VFS
*  Partition - hands on
*  mount - hands on
*  /etc/fstab - hands on
<br></br>
** Reading materials:** 
* [](https://www.thegeekdiary.com/understanding-the-proc-file-system/ /proc  
* [](http://haifux.org/lectures/84-sil/users-processes-files-and-permissions/users-perms-lec.html permissions  
* [](https://docs.nersc.gov/filesystems/unix-file-permissions/ file permissions  
* [](https://www.linux.com/training-tutorials/linux-filesystem-explained/ linux filesystem 
* [](https://www.howtogeek.com/465350/everything-you-ever-wanted-to-know-about-inodes-on-linux/ inodes  
* [](https://docs.oracle.com/cd/E18752_01/html/817-5093/fscreate-99040.html creating tmpfs  
* [](https://docs.microsoft.com/en-us/troubleshoot/windows-client/backup-and-storage/fat-hpfs-and-ntfs-file-systems FAT & NTFS  
* [](https://opensource.com/article/17/5/introduction-ext4-filesystem EXT4  
* [](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/storage_administration_guide/ch-xfs XFS 
* [](https://www.thegeekdiary.com/how-to-create-an-xfs-filesystem/ creating a filesystem  
* [](https://emmanuelbashorun.medium.com/linux-file-system-virtual-file-system-vfs-layer-part-3-79235c40a499 VFS  
* [](https://medium.com/@meghamohan/hard-link-and-symbolic-link-3cad74e5b5dc#:~:text=Both%20the%20inodes%20and%20data,a%20disk%20partition%20is%20organized.&text=Symbolic%20link%20(Symlinks%2FSoft%20links,directory%20it%20is%20pointing%20to. Hard & Soft links  
* [](https://opensource.com/business/16/9/linux-users-guide-lvm LVM  
* [](https://phoenixnap.com/kb/linux-create-partition create partitions  
<br></br>

## Networking in Linux:
** Deliveries:** 
### in depth
*  NetworkManager
*  NetworkManager CLI -hands on
*  /etc/sysconfig/network-scripts/
*  tcpdump -hands on
*  netstat -hands on
*  /etc/resolv.conf
*  /etc/hosts

### surface level
*  /etc/sysconfig/network
*  <strong> iptables </strong>
<ul>
  *  chains 
  *  hooks
  *  netfilter
  *  tables
  *  targets
</ul>
*  IPVS
*  ss - hands on
*  Route tables - hands on
*  sockets
*  unix sockets
*  /etc/nsswitch.conf
*  NTP
*  Chronyd
<br></br>
** Reading materials:** 
* [](https://www.thegeekdiary.com/linux-os-service-networkmanager/ NetworkManger  
* [](https://www.golinuxcloud.com/nmcli-command-examples-cheatsheet-centos-rhel/ NMCLI  
* [](https://docs.oracle.com/cd/E37670_01/E41138/html/ch10s02s04.html /etc/sysconfig/network 
* [](https://docs.netapp.com/sgws-110/index.jsp?topic=%2Fcom.netapp.doc.sg-install-rhel%2FGUID-04DC1897-D5B4-4A43-BF6B-E8CB719BBA0D.html /etc/sysconfig/network-scripts/  
* [](https://www.digitalocean.com/community/tutorials/a-deep-dive-into-iptables-and-netfilter-architecture iptables  
* [](https://www.booleanworld.com/depth-guide-iptables-linux-firewall/ more iptables  
* [](https://kubernetes.io/blog/2018/07/09/ipvs-based-in-cluster-load-balancing-deep-dive/ IPVS  
* [](https://cdn.comparitech.com/wp-content/uploads/2019/06/tcpdump-cheat-sheet.jpg tcpdump cheat sheet 
* [](https://opensource.com/business/16/8/introduction-linux-network-routing routing  
* [](https://www.cloudsavvyit.com/1263/what-are-unix-sockets-and-how-do-they-work/ unix sockets  
* [](https://www.thegeekdiary.com/understanding-etc-hosts-file-in-linux/ /etc/hosts  
* [](http://linuxservertutorials.blogspot.com/2008/11/ubuntu-nsswitchconf-guide.html /etc/nsswitch.conf 
<br></br>

## Permissions Management:
** Deliveries:** 
### in depth
*  Linux Permissions
*  SELINUX
*  MAC
*  DAC
*  RBAC
*  Type Enforcement
<br></br>
** Reading materials:** 
* [](https://people.redhat.com/duffy/selinux/selinux-coloring-book_A4-Stapled.pdf SELINUX Coloring Book  
* [](https://sites.google.com/site/cacsolin/selinux SELINUX  
* [](https://wiki.centos.org/HowTos/SELinux SELINUX  
* [](http://www.differencebetween.net/technology/software-technology/difference-between-mac-and-dac/#:~:text=DAC%20provides%20users%20who%20have,including%20them%20in%20the%20list.&text=A%20good%20example%20of%20a,systems%20is%20a%20good%20example. MAC VS DAC  
* [](https://auth0.com/docs/manage-users/access-control/rbac RBAC  
<br></br>

## Secured Networking:
** Deliveries:** 
### in depth
*  SSH Keys
*  Certificates
*  OPENSSL - hands on
*  CSR
*  CA
<br></br>
** Reading materials:** 
* [](https://jumpcloud.com/blog/what-are-ssh-keys#:~:text=Essentially%2C%20SSH%20keys%20are%20an,to%20manage%20the%20remote%20system. SSH Keys  
<br></br>

## Permissions Management:
** Deliveries:** 
### in depth
*  RPM
*  YUM
*  /etc/yum.repos.d/
*  Pxe boot

### surface level
*  Cron
*  Ansible - hands on
*  RPC
*  udev
<br></br>
** Reading materials:** 
* [](https://www.simplilearn.com/tutorials/ansible-tutorial/what-is-ansible?source=sl_frs_nav_playlist_video_clicked Ansible  
* [](https://www.learnlinux.tv/getting-started-with-ansible/ Getting started with Ansible
* [](https://www.seobility.net/en/wiki/CronJob Cron  
<br></br>

### Drills:
*  Lab
*  Estimated time: Day
<br></br>
*  Test
*  Estimated time: Day
