
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
    - 同時管很多台實體電腦，標出很多的船 Docker

---

- 如何在舊版本執行新版虛擬機

    - 行規: 向下相容

![](https://i.imgur.com/InI50SI.png)

- virtualHW.version = "18"

![](https://i.imgur.com/Xv9Qjni.png)

---

![](https://i.imgur.com/DuCLlGx.png)

- 雲端裡面再建雲: 巢狀雲 Nested Virtualization
- Linux 純種虛擬技術: Linux KVM
- K8S會咬IP，如果虛擬機在同一台電腦，移動到另一台電腦，IP會換







