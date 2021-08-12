---
title: "1.1 检查你的硬件、BIOS和系统"
---

# 1. 打开[英特尔官网](https://www.intel.cn/content/www/cn/zh/homepage.html)，搜索自己的CPU型号
![image.png](https://cdn.nlark.com/yuque/0/2020/png/579469/1605266706489-fba32db1-a247-408d-926c-9169043a5183.png#height=735&id=nbarH&margin=%5Bobject%20Object%5D&name=image.png&originHeight=735&originWidth=705&originalType=binary&ratio=1&size=261049&status=done&style=none&width=705)<br />![image.png](https://cdn.nlark.com/yuque/0/2020/png/579469/1605266756952-39d8b0f6-9bd2-426d-a852-7bd5c2c7a679.png#height=500&id=SCdkj&margin=%5Bobject%20Object%5D&name=image.png&originHeight=500&originWidth=702&originalType=binary&ratio=1&size=94779&status=done&style=none&width=702)
### 如图所示，这样就是支持的芯片。
# 2、Phala 硬件需求门槛
![image.png](https://cdn.nlark.com/yuque/0/2021/png/1743015/1627285625807-4d4d7326-f5e5-4c5f-8c3b-9f20709e78bd.png#clientId=u544c2d17-4ba4-4&from=paste&height=171&id=u4d81b48b&margin=%5Bobject%20Object%5D&name=image.png&originHeight=362&originWidth=1552&originalType=binary&ratio=1&size=345627&status=done&style=none&taskId=ufa74962c-3a66-4331-b40e-f843405126a&width=731.3333740234375)<br />如何检查自己的设备支持 BIOS：<br />[前往 Intel 官网查询自己的芯片是否支持 SGX](https://www.notion.so/Intel-SGX-26209def476b40b0bb578c89d4d2e7da)<br />

# 3、确认 BIOS 设置

1. 首先打开百度，查询进入你的电脑的 BIOS 键是什么。每个品牌不一样。重启电脑，快速按下刚刚查到的键，进入 BIOS 界面。
   - 找到 Security（安全选项） ，找到 Secure Boot（安全启动） ，选择 Disabled（关闭）
   - 找到 Boot（启动选项） ，在 Boot Mode (启动模式) 里 启动 UEFI
   - 找到 SGX 选项，优先选 Enabled，如果没有则选 Software Controlled。选择 Software Controlled 的，进入系统以后输入下面的指令启动驱动：
```shell
wget https://github.com/Phala-Network/sgx-tools/releases/download/0.1/sgx_enable 
sudo chmod +x sgx_enable
sudo ./sgx_enable
```
Tip<br />**如何打开 Ubuntu 终端：在桌面点击右键 →终端（Open in Terminal）**
# 4、Ubuntu 18.04 / 20.04

- 目前暂不支持这两个版本以外的版本
- [怎么安装 Ubuntu 18.04](https://ywnz.com/linuxaz/2588.html#:~:text=1.%E8%BF%9B%E5%85%A5win%20PE%EF%BC%88%E8%BF%99,Ubuntu%2018.04%E7%B3%BB%E7%BB%9F%E5%AE%89%E8%A3%85%E4%BA%86%E3%80%82)


<br />**​**

[**矿力觉醒↗️**](https://www.yuque.com/fagephalanetwork/phalatothemoon/kp0rv0)<br />矿力觉醒为社区成员提供的通过SGX测试的硬件设备，方便新用户快速上手。<br />此表中所有设备均非官方推荐，官方仅对系统中所查询数据负责，机器仅供参考。
