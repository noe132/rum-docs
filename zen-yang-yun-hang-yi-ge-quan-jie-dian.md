# 怎样运行一个全节点

{% hint style="info" %}
本文旨在指引在 Linux 服务器上使用 Quorum 运行全节点的用户。&#x20;
{% endhint %}

{% hint style="info" %}
如果您的宽带运营商正常提供上传带宽，或者您能自己配置 NAT，则可以直接在本地运行全节点客户端 Rum APP，和本文档要达到的目的一致。
{% endhint %}



> 远程环境：Unbuntu 20.04 + Quorum
>
> 本地环境：Windows 10 + Rum App V3.3.18

#### 0. 什么是全节点

全节点是区别于轻节点的概念。

为了降低用户门槛，Rum 团队开发了轻节点，用户通过轻节点可以加入种子网络，也可以浏览和发布内容，但是不能向 Rum Network 提供网络资源，比如出块。

全节点用户除了可以在应用层面使用 Rum 的内容管理和发布功能，也可以向 Rum Network 提供网络资源。

#### 1. 为什么要运行全节点

Rum Network 的内容由群组（Groups，应用层面称为种子网络，SeedNet）聚合并呈现在不同的 DApp 中。而群组则由不同的节点组成，通过一个叫 Gossip Network 的通信层联系在一起。由此可见，节点是 Rum System 的基础单位，由节点们共同提供网络资源，服务于应用层面。如下图所示：

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption><p>Rum System 网络结构示意图</p></figcaption></figure>

全节点的建立者有以下的补助与权限：

1. 提供了网络资源的节点会得到经济系统的补助（经济系统建设中）。
2. 由节点创建的种子网络，该节点拥有该种子网络的管理员权限。

#### 3. 怎样运行全节点

#### 环境配置：

1. 将 Quorum 克隆到服务器

```
git clone git@github.com:rumsystem/quorum.git
```



[https://github.com/rumsystem/quorum](https://github.com/rumsystem/quorum)

1.  安装 Go 语言运行环境

    按照 Go 语言官方文档操作即可：[https://go.dev/doc/install](https://go.dev/doc/install)

