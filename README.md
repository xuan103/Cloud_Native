
# Cloud Native 雲原生架構師

---

- [Cloud Native Computing Foundation (cncf)](https://www.cncf.io/)

> open source 大家都可以用，讓資訊都是公平的

![](https://i.imgur.com/ykWdbDu.png)

---

# 雲端（三大雲）

> 最強

- AWS Amazon

> 第二：微軟 與 google 競爭

- Azure 微軟
- GCP google

---

- Graduated projects 正式版

```
- containerd
- CoreDns
- etcd
- Harbor
- Helm
- Kubernetes
```

![](https://i.imgur.com/1j6zmHD.png)


- Incubating projects 孵蛋計畫

![](https://i.imgur.com/exJxGgT.png)


---

![](https://i.imgur.com/MboioTh.png)

- docker: Container 貨櫃


![](https://i.imgur.com/JytZ83k.png)

- Kubernetes 船舵

    - 控制與管理 Docker 貨櫃
    - 同時管很多台實體電腦，標出很多的 Docker

---

## 如何在舊版本執行新版虛擬機

- 版本不相容
    - 無法開啟（行規: 版本可向下相容, 但不支援向上兼容 [可以就奇怪了..] ）

![try1](https://i.imgur.com/fp8utTY.jpg)

![try2](https://i.imgur.com/tj5Rnt7.jpg)

### 解決辦法：

- 到 vmware 資料夾, 找到 .vmx 檔案（.vmx是虛擬機的設定檔 ）

![](https://i.imgur.com/CCdonwt.png)


- 利用 notepad++ (linux 的版本叫 notepadqq) 更改 `virtualHW.version = "18"` (改後面版本 "18" 建議為 "14") 

![](https://i.imgur.com/Xv9Qjni.png)

---

![](https://i.imgur.com/DuCLlGx.png)

- 雲端裡面再建雲: 巢狀雲 Nested Virtualization
- Linux 純種虛擬技術: Linux KVM(Kernel Virtual Machine)
- K8S會咬IP，如果虛擬機在同一台電腦，移動到另一台電腦，IP會換，K8S會死掉，所以要在雲端建置



- 點檔案 > wk 目錄 > windows 視窗擺右邊, VM 擺左邊, cnt20 拖進來 > 變成小手手再放開

    - cnt20必須在Linux虛擬機器解壓縮
    - 要等右上角圈圈跑完: 第二次拷貝

> unzip cnt20.zip; cd cnt
```
alpine
ub1804
ub2004
```
- 總共操控的Docker機器

> dkh start

- cnt: Cloud Native Trainer

> dkh start ddg52

> dkh ps 

- `-m`: Memory

mac: manifactory 網路卡出廠代號

```
-m 2560 -> 2.5G
macadd=52:54:72:16:30:52 -> ADDG 52 網卡的MAC最後一碼 電腦名稱最後一個數字
前兩碼52:54 代表著 Linux KVM
```

> ifconfig

```
ens32
ether `00:0c` VMWare WorkStation在用
前兩碼為系統出場
```

- Windows

> ipconfig

```
乙太網路 實體網路卡: C8-D9-D2-04-57-12
Intel
```

- mac 才能真的鎖住機器，IP 可以造假 (mac 編號全球唯一, mac可以管很多IP)

> ssh x.x.x.52 

- 52 為 MAC IP最後一碼
   - root一定要關掉 

> sudo apt update; sudo apt upgeade -y 

> poweroff

> ls -al
```
ddg52.qcow2 虛擬硬碟檔
```

- windows 看本機規格

> 搜尋列打 dxdiag

```
處理器: i7 12核
記憶體: 16G 可用 4G
```

- 看完本機規格，調整虛擬機器

> Virtual Machine Settings

```
記憶體 12G*1024=12288MB
Processors 8核心
```
![](https://i.imgur.com/7DMaLF8.png)


- 記憶體

> free -mh


- 查看CPU

> cat /proc/cpuinfo | grep 'model name'

> lscpu | grep 'CPU(s):'

---

# 執行程序

[圖片原檔](https://docs.google.com/presentation/d/1_8Yqq2AAKlOHm66FIEa9ux2IMHBpr5YaO95S1ms6CeY/edit?usp=sharing)

![](https://i.imgur.com/FtIdcPT.png)


- 當執行一個檔案(File)的時候，必須要有寫入和執行的權限，執行的時候由作業系統執行。

- 要告訴作業系統路徑：

1. “ ./ “ （在當前的目錄下直接執行）
 
2. 把檔案放到$PATH裡面 （就像平常輸入的指令一樣）
 
當作業系統開始執行程式的時候檔案 (File) 就會變成 process，CPU 就會開始去讀取 process 然後依照內容抓取需要的 system call 還有 library，在抓取需要的 system call 還有 library 的時候權限的判定是依照執行者的權限。

- 開始執行的時候作業系統會把檔案 load 到記憶體裡



- rwx 權限可用指令:

```
r -> ls,cat,head,tail
w -> nano(必須要有R才能W),touch, > , >> (append), rm , cp(對原檔案有讀的權限,對目的地檔案有寫的權限) , mv(原檔案有rm功能)
x(要有讀的功能) -> source, ./File
```

- Run Program 的人是 Operating System(OS)
1. ./ 是告訴OS檔案在這裡
2. $PATH

檔案 `File` 被執行的時候就變  `process`

- process
   -  system call(kernel) Ex: ls(), cat(),upgrade -> 預設的
   -  library(ML, GPU, printer Driver) -> 像是Java

---

- 查看 ping source 檔案
> which ping
```
/user/bin/ping
```
- ybean@cvn71:~$ 
- 看程式有哪些Library
> sudo ldd /usr/bin/cat
```
	linux-vdso.so.1 (0x00007ffccf7d0000)
	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007fa236316000)
	/lib64/ld-linux-x86-64.so.2 (0x00007fa236528000)
```
- .so 副檔名為 Library，其他為System Call


>  sudo ldd /usr/bin/grep
```
	linux-vdso.so.1 (0x00007fffe6474000)
	libpcre.so.3 => /lib/x86_64-linux-gnu/libpcre.so.3 (0x00007f55bd271000)
	libdl.so.2 => /lib/x86_64-linux-gnu/libdl.so.2 (0x00007f55bd26b000)
	libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f55bd079000)
	libpthread.so.0 => /lib/x86_64-linux-gnu/libpthread.so.0 (0x00007f55bd056000)
	/lib64/ld-linux-x86-64.so.2 (0x00007f55bd32c000)
```

> ps aux

- cp 跟 mv 有 W的目錄才能寫進去
> mkdir /data
```
拒絕不合權限的操作
```

> ls -al /
```
Owener Group
root root .
```
- sudo :授權 至 root 權限，只是授權而已,而非就是 root ！！

> sudo mkdir /data
```
Owener Group
root   root  data
```
- -R recursive 強制data下面 改成bigred擁有者
> sudo chown bigred:root -R /data

- 能寫能改,不能刪
> chmod 1777 /data
> ls -al /
```
drwxrwxrwt   2 root root       4096 11月 27 15:11 data
```
> sudo useradd -m -s /bin/bash ybean
> echo "ybean:ybean" | sudo chpasswd
> sudo login ybean

> ybean@cnv71:~$ ls -al /data
> ybean@cnv71:~$ ls -al /data/jbean
> rm -r /data/jbean
```
無法移除
```
> mkdir /data/jbean/ybean
```
無法建立，代表不能改別人內容
```


---

### try SBIT: `chmod 1777 /data` 

#### can `cp`, can not `mv` 

- ybean@cvn71:~$

> mkdir /data/ybean
> 
> touch /data/ybean/test.txt
> 
> ls -al /data/
```
total 16
drwxrwxrwt  4 bigred root  4096 11æœˆ 26 14:02 .
drwxr-xr-x 21 root   root  4096 11æœˆ 26 13:29 ..
drwxrwxr-x  2 jbean  jbean 4096 11æœˆ 26 14:02 jbean
-rw-rw-r--  1 jbean  jbean    0 11æœˆ 26 14:02 test.txt
drwxrwxr-x  2 ybean  ybean 4096 11æœˆ 26 14:01 ybean
```

> mv /data/test.txt  /data/jbean/

``` 
mv: '/data/test/try.txt' and '/data/test/try.txt' are the same file
```

- jbean@cvn71:~$

> ls -al /data/

```
total 12
drwxrwxrwt  3 bigred root  4096 11æœˆ 26 14:00 .
drwxr-xr-x 21 root   root  4096 11æœˆ 26 13:29 ..
drwxrwxr-x  2 ybean  ybean 4096 11æœˆ 26 14:01 ybean
```

> cp /data/ybean/test.txt /data/
> 
> ls -al /data/
```
total 12
drwxrwxrwt  3 bigred root  4096 11æœˆ 26 14:02 .
drwxr-xr-x 21 root   root  4096 11æœˆ 26 13:29 ..
-rw-rw-r--  1 jbean  jbean    0 11æœˆ 26 14:02 test.txt 
drwxrwxr-x  2 ybean  ybean 4096 11æœˆ 26 14:01 ybean
```
> mkdir /data/jbean

---

> sudo find / -name *.so
..
/usr/lib/crda/libreg.so
/usr/lib/tc/m_ipt.so
/usr/lib/tc/q_atm.so
/usr/lib/tc/m_xt.so
/usr/lib/sudo/libsudo_util.so
/usr/lib/sudo/group_file.so
/usr/lib/sudo/sudoers.so
/usr/lib/sudo/sudo_noexec.so
/usr/lib/sudo/system_group.so
...

- ybean@cvn71:~$ 

> pstree -ph
```
systemd(1)─┬─ModemManager(726)─┬─{ModemManager}(747)
           │                   └─{ModemManager}(750)
           ├─NetworkManager(614)─┬─{NetworkManager}(694)
           │                     └─{NetworkManager}(702)
           ├─accounts-daemon(600)─┬─{accounts-daemon}(603)
           │                      └─{accounts-daemon}(698)
..
           ├─sshd(725
..
           ├─systemd-journal(258)
           ├─systemd-logind(647)
           ├─systemd-resolve(558)
           ├─systemd-timesyn(559)───{systemd-timesyn}(597)
           ├─systemd-udevd(280)
           ├─thermald(649)───{thermald}(718)
           ├─udisksd(650)─┬─{udisksd}(667)
           │              ├─{udisksd}(701)
           │              ├─{udisksd}(737)
           │              └─{udisksd}(774)
           ├─unattended-upgr(779)───{unattended-upgr}(800)
           ├─upowerd(963)─┬─{upowerd}(967)
           │              └─{upowerd}(968)
           ├─vmnet-bridge(1334)
           ├─vmnet-dhcpd(1348)
           ├─vmnet-dhcpd(1359)
           ├─vmnet-natd(1351)
           ├─vmnet-netifup(1342)
           ├─vmnet-netifup(1353)
           ├─vmware-usbarbit(685)
           ├─whoopsie(1262)─┬─{whoopsie}(1285)
           │                └─{whoopsie}(1297)
           └─wpa_supplicant(652)



```


- 新版Linux第一個為 ststemd(1)
- 舊版Linux第一個為 int(1)
- sshd(936) 為 openssh-server
- vmtollsd(795) 為 VMware tools
- 背景程式 Ubuntu稱為demon Windows稱為Server

---

![](https://i.imgur.com/kqxnc8Y.png)

- top 可知道process用了多少CPU Memory,案q離開
- top -bin 2 -d 1
    - 每一秒抓一次,顯示兩次

> top -bin 2  -d 1 > pid.txt
> cat pid.txt 
```
top - 15:42:16 up  2:40,  3 users,  load average: 0.56, 0.47, 0.46
Tasks: 326 total,   1 running, 324 sleeping,   1 stopped,   0 zombie
%Cpu(s):  6.3 us,  5.1 sy,  0.0 ni, 88.6 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :   7819.9 total,    439.4 free,   2637.8 used,   4742.7 buff/cache
MiB Swap:   2048.0 total,   1706.2 free,    341.8 used.   4172.4 avail Mem 

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
   8344 bigred    20   0   12328   4040   3320 R  17.6   0.1   0:00.04 top
   1495 xuan      20   0  861260  44768  22760 S   5.9   0.6   2:58.70 Xorg

top - 15:42:17 up  2:40,  3 users,  load average: 0.56, 0.47, 0.46
Tasks: 326 total,   1 running, 324 sleeping,   1 stopped,   0 zombie
%Cpu(s):  5.9 us,  1.7 sy,  0.0 ni, 92.4 id,  0.0 wa,  0.0 hi,  0.0 si,  0.0 st
MiB Mem :   7819.9 total,    439.4 free,   2637.8 used,   4742.7 buff/cache
MiB Swap:   2048.0 total,   1706.2 free,    341.8 used.   4172.4 avail Mem 

    PID USER      PR  NI    VIRT    RES    SHR S  %CPU  %MEM     TIME+ COMMAND
   2914 xuan      20   0 4806868 261484  93716 S  11.7   3.3  15:27.51 chrome
   1658 xuan      20   0 4571252 202236  64020 S   5.8   2.5   4:05.84 gnome-shell
   2679 xuan      20   0  643300 115004  53352 S   5.8   1.4   5:25.84 chrome
   1495 xuan      20   0  861260  44768  22760 S   2.9   0.6   2:58.73 Xorg
   8344 bigred    20   0   12328   4040   3320 R   2.9   0.1   0:00.07 top
   2684 xuan      20   0  386460  69980  33156 S   1.0   0.9   1:04.80 chrome
   2795 xuan      20   0  930352  48056  33500 S   1.0   0.6   0:32.93 gnome-terminal-
   3062 xuan      20   0 3051656 118700  70468 S   1.0   1.5   0:33.86 notepadqq-bin
   6683 root      20   0       0      0      0 I   1.0   0.0   0:02.91 kworker/u8:1-events_unbound
```


> nano nostop.sh
- trap 鎖定使用者如果按了ctrl-c會發生的事件
```
#!/bin/bash
# trap ctrl-c and call ctrl_c()
trap ctrl_c INT

function ctrl_c() {
   echo -e "\n** Trapped CTRL-C"
   # 下式顯示游標
   tput cnorm
   exit 0 
}

clear
# 下式隱藏游標
tput civis

while [ 1 ]
do
  echo -ne "\033[10;10f Hi Chen : "   
  date
done
```

> ./nostop.sh

![](https://i.imgur.com/41cEKN3.png)

- ctrl-z 在背景後執行,fg即可回到前面執行


> nano nostop01.sh
```
#!/bin/bash
c=0; echo "" > /tmp/counter
while  [  true  ]
do 
   echo  "$c"  >>  /tmp/counter
   let  "c=$c+1"
done
```
- 在背景執行
> ./nostop01.sh &

- 看是否有在跑
> tail -n 2 /tmp/counter
```
255086
255087
```
- 看 ID
> ps aux | grep 'nostop01.sh'
```
bigred      6521  100  0.0   9832  3316 pts/0    R    17:02   1:09 /bin/bash ./nostop01.sh
```
- 停止背景執行
> kill -9 6521

> fg
```
bash: fg: 工作已經終止
[1]+  已砍掉               ./nostop01.sh
```


---
# Process Security

linux kvm（英語：Kernel-based Virtual Machine，縮寫為KVM）
kvm 優於 vmware

ddg52: 172.29.0.52


- go Language
   - C的速度 
   - python的簡潔 
   - java的物件


> cat myfile.go
```
package main
import (
    "fmt"
    "io/ioutil"
)
func main() {
    err := ioutil.WriteFile("/mulan.txt", []byte("Hello"), 0755)
    if err != nil {
        fmt.Printf("Unable to write file: %v\n", err)
    } else {
        fmt.Printf("/mulan.txt created\n")
    }
}
```

> CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -o myfile
> dir myfile
```
-rwxrwxr-x 1 bigred bigred 2.0M NOV 27 10:30 myfile
```
> ./myfile
```
Unable to write file: open /mulan.txt:permision denied
```
> sudo ./myfile
```
/mulan.txt created
```
> sudo rm /mulan.txt

> dir myfile
```
-rwxrwxr-x bigred bigred 2.0M NOV 27 10:30 myfile
```

> sudo chown root myfile
> dir myfile
```
-rwxrwxr-x 1 root bigred 2.0M Nov 27 10:30 myfile
```

## SUID:chmod 4XXX
- SUID 只會用在執行檔, Stick 用在目錄
> sudo chmod 4755 myfile
> ./myfile
```
/mulan.txt created
```
> dir /myfile.txt
```
-rwxrwxr-x 1 root bigred 5 Nov 27 10:58 /myfile.txt
```
- 檢視 系統中有多少具有 setuid 功能的 命令
> sudo find / -user root -perm -4000 2>/dev/null | grep -E '^/bin|^/usr/bin'
```
/bin/umount
/bin/su
/bin/fusermount
/bin/mount
/usr/bin/chsh
/usr/bin/pkexec
/usr/bin/sudo
/usr/bin/passwd
/usr/bin/chfn
/usr/bin/newgrp
/usr/bin/gpasswd
```

> ls -al /usr/bin/passwd
```
-rwsr-xr-x root root 68208 May 28 2020 /usr/bin/passwd
```
- 創造使用者
> sudo useradd -m -s /bin/bash zbean
> echo "zbean:zbean" | sudo chpasswd
> exit
```
logout
```
- 因SUID,所以zbean可以用root執行改密碼這個動作
> ssh zbean@X.X.X.52
> passwd
```
Current password:
New password:
passwd: password updated successfully
```

> ls -al /usr/bin/passwd
```
-rwsr-xr-x 1 root root 68208 May 28 2020 /usr/bin/passwd
```

- 修補漏洞,更改 others 權限
> exit
> ssh bigred@X.X.X.52
> sudo chmod 750 /usr/bin/passwd
> dir /usr/bin/passwd
```
-rwxr-x--- root root 68208 May 28 2020 /usr/bin/passwd
```

> exit
> ssh zbean@X.X.X.52
> passwd
```
-bash: /usr/bin/passwd: Permission denied
```

- ping 要能送封包 必須要 root，否則很容易被入侵
> k8s start cg60
which ping
```
/usr/bin/ping
```
> ls -al /usr/bin/ping
```
-rwxr-xr-x 1 root root
```
> k8s start cg60
ssh X.X.X.60
which ping
```
/bin/ping
```
> ls -al /bin/ping
```
-rwsr-xr-x 1 root root 64424 Jun 28 2019 /bin/ping
```
- Ubuntu 18.04(cg60) ping 是Set UID 20.04已拿掉

- 關掉 cg60
> k8s stop cg60
> ssh bigred@X.X.X.52

## capbility:小角色變成King
bigred@ddg52:~$
> sudo cp /usr/bin/pyton3 /home/zbean


- python3 在跑程式的時候有setUID的能力
> sudo setcap cap_setuid+ep /home/zbean/python3
> exit
```
logout
```
> ssh zbean@X.X.X.52


- Linux 有多少命令設定 capbility
    - 20.04能ping, 是因為 raw(同樣有getid功能)
    - python3 埋程式進去(getid)
> getcap -r / 2>/dev/null
```
/home/zbean/python3 = cap_setuid+ep
/bin/ping = cap_net_raw+ep
```

> ls -al python3
```
-rwxr-xr-x 1 root root 5453504 Jul 28 04:56 python3
```
>  ./python3 -c 'import os;os.setuid(0);os.system("/bin/bash")'
>  root@ddg52:~$ whoami
```
root
```
## Linux Name Space
- Linux Name Space:把一個程式變成一台電腦(隔離 Process ID,可以變成 有檔案系統,有獨立IP) -> 比傳統電腦快上十倍
   - unshre

- 把sh做隔離
> sudo unshare --uts sh

bigred@ddg52:~$ 

> sudo unshare --uts sh
> `#` hostname
```
ddg52
```
> `#` hostname ssn763
> `#` hostname
```
ssn763
```

- 因隔離再次登入ddg，hostname 未被更改
> ssh X.X.X.52
> hostname
```
ddg52
```
- ps aux 讀一個目錄 /proc , 記憶體型的虛擬目錄RAM Disk
> ps aux
- --fork process的系統裡面可以生下子子孫孫
> sudo unshare --pid --fork sh
> `#` ps aux
```
還是看到原來的process資訊, 因沒有對應到 /proc,還在讀舊的檔案
```
bigred@ddg52:~$
- 少下參數 Mount:override 蓋上去,原來的/proc 還在
> sudo unshare --pid --fork --mount-proc sh
>  `#` ps aux
```
USER PID COMMAND
root 1     sh
root 2     ps aux
```
> `#`sleep 300 &
> `#`ps aux
```
USER PID COMMAND
root  1     sh
root  3    sleep 300
root  4    ps aux
```
- PID 3 因有下 --fork 有Child
-  /proc 目錄被掛載二次
> mount | grep -e "^proc"
```
proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
```

> curl -o alpine.tar.gz `http://dl-cdn.alpinelinux.org/alpine/v3.12/releases/x86_64/alpine-minirootfs-3.12.0-x86_64.tar.gz`
- tar.gz 是 Linux 的主力壓縮檔
> dir
```
-rw-rw-r-- 1 bigred bigred 2.6M Nov 27 15:20 alpine.tar.gz
```
> tar xvf alpine.tar.gz ;rm alpine.tar.gz; cd .. 

> dir rootfs/bin
```
cat -> /bin/busybox
```
- 多個指令都是由 /bin/busybox去run
> which busybox
```
/bin/busybox
```
> ls -alh /bin/busybox
```
-rwxr-xr-x root root 2.1M Nov 11 20:15 /bin/busybox
```

> dir rootfs/proc
```
內容是空的
```
> sudo unshare --pid --fork --mount-proc -R rootfs bash
> which bash
```
/bin/bash
```
- 是從rootfs裡面找的,非ddg52
-  -R rootfs run rootfs 檔案系統
> sudo unshare --pid --fork --mount-proc -R rootfs sh
> mount
```
proc on /proc type proc (rw,nosuid,nodev,noexec,relatime)
```
> ps aux
```
PID USER TIME
```
> mkdir /hi
> exit
> dir rootfs/hi

- bridge control

> brctl show
```
bridge name
kvmbr29
```

> cat bin/kvmnet.sh
```
#!/bin/bash

ifconfig kvmbr29 &>/dev/null
if [ $? != 0 ]; then
     sudo brctl addbr kvmbr29
     sudo ifconfig kvmbr29 hw ether 02:00:00:00:00:01
     sudo ifconfig kvmbr29 X.X.X.254 netmask 255.255.255.0 up
     sudo iptabkes -t nat -A POSTROUTING -s X.X.X.0/24 -j MASQUERADE
fi
```

![](https://i.imgur.com/gNM2AlN.png)


- 橋接器取名
  - sudo brctl addbr kvmbr29
- MAC address : bridge 主力網卡就要用最小!!!
  - 02:00:00:00:00:01
- 設定IP
  - 行規 gateway:254
- 偽裝IPtables 
  - CVN71 為 bridge, kvmbr29 就等同於新增一片網卡,把Linux KVM 的虛擬機連到kvmbr29偽裝,連到CVN71出去Internet

- 在哪執行,判讀是否有bridge
> cat bin/dkh
```
start)
    /home/bigred/wk/cnt/bin/kvmnet.sh
```

![](https://i.imgur.com/vYVsxzE.jpg)

- Switch 跟 Bridge 的差別是 有防呆功能
    - Switch若沒整線,就會有迴圈,Bridge會判斷有無迴圈而自動斷電
