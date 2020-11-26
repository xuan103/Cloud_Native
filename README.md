
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
- Linux 純種虛擬技術: Linux KVM
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

### try `chmod 1777 /data` 

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

> top -bin 2  -d 1 >pid.txt
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

https://drive.google.com/drive/folders/1A4r2hGF1FueXqmsqO7QsDjT3R_WZB6UQ?usp=sharing





jbean@Lenovo:~$ ls -al /data
總計 16
drwxrwxrwt  4 bigred root   4096 11月 26 22:37 .
drwxr-xr-x 21 root   root   4096 11月 26 22:34 ..
drwxrwxr-x  2 bigred bigred 4096 11月 26 14:10 test
-rw-rw-r--  1 ybean  ybean     0 11月 26 22:36 try.txt
drwxrwxr-x  2 ybean  ybean  4096 11月 26 22:37 ybean
jbean@Lenovo:~$ cp /data/try.txt /data/ybean/
cp: 無法建立普通檔案 '/data/ybean/try.txt': 拒絕不符權限的操作
jbean@Lenovo:~$ cp /data/try.txt /data/ybean/try.txt
cp: 無法建立普通檔案 '/data/ybean/try.txt': 拒絕不符權限的操作

jbean@Lenovo:/data/ybean$ mkdir testtt.txt
mkdir: 無法建立目錄‘testtt.txt’: 拒絕不符權限的操作

