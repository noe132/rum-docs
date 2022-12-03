# 怎样创建 Port 论坛

### ****

### ****

### **第一步**

要创建属于自己的 Port 论坛，我们需要创建一个论坛型的种子网络。

在自己作为 Owner 的全节点上创建一个论坛型的种子网络，就拥有该种子网络的管理员权限。管理员可以在 Rum App 中管理种子网络的基本信息和用户权限。

因此我们需要先运行一个 quorum 全节点。运行全节点的方式请参考：[怎样运行一个全节点](zen-yang-yun-hang-yi-ge-quan-jie-dian.md)

然后通过 Rum App 远程连接全节点创建自己的论坛型的种子网络。参考：[通过 Rum App 远程连接全节点](zen-yang-yun-hang-yi-ge-quan-jie-dian.md#step-3-yong-rum-app-yuan-cheng-lian-jie-quorum-fu-wu)

****

### **第二步**

创建了论坛型的种子网络后，有以下两种方式将其导入到 Port 服务中：

#### 第一种是直接在 [Port 官方建立的 Port 服务](https://port.base.one) 中输入种子网络地址进行访问。

种子网络的地址可以通过在 Rum App 中分享该种子网络获取。你将获取到"Rum://"开头的一串文本。复制并粘贴在如下图所示的文本框并回车：

<figure><img src=".gitbook/assets/Screenshot 2022-12-03 at 23.54.34.png" alt=""><figcaption></figcaption></figure>

如果想将自己创建的种子网络加入 [Port 登录页](https://port.base.one) 的论坛列表中，可以发送邮件给 Rum 团队进行申请：

team@rumsystem.net

这种方式建立的论坛需要

#### 第二种方式是自己部署一个 Port 服务，并在该服务中导入自己创建的论坛型的种子网络。

{% hint style="info" %}
第二种方式还在努力开发中，我们将提供快速搭建 Port 服务的功能，敬请期待！
{% endhint %}



下面是两种方式的对比：

<table><thead><tr><th>自建种子网络</th><th>自建 Port 服务并导入自建种子网络</th><th data-type="rating" data-max="1">相同</th></tr></thead><tbody><tr><td>需要运行一个 Quorum 服务</td><td>需要运行一个 Quorum 服务</td><td>1</td></tr><tr><td>需要创建一个论坛型种子网络</td><td>需要创建一个论坛型种子网络</td><td>1</td></tr><tr><td>不需要运行一个 Port 服务</td><td>需要运行一个 Port 服务</td><td>null</td></tr><tr><td>与 Port 官方共享 NFT 权限控制</td><td>自行发布 NFT 并定义权限</td><td>null</td></tr></tbody></table>
