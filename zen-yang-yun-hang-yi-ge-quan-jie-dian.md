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

#### 前提条件

* 将 Quorum 克隆到服务器

```
git clone git@github.com:rumsystem/quorum.git
```

{% hint style="info" %}
仓库地址：[https://github.com/rumsystem/quorum](https://github.com/rumsystem/quorum)
{% endhint %}

* 安装 Go 语言运行环境

按照 Go 语言官方文档操作即可：[https://go.dev/doc/install](https://go.dev/doc/install)

* Build Quorum 二进制文件

执行命令：

`make linux` 或 `make buildall`

成功 Build 的二进制文件位于`dist`文件夹中。

也可以 Build Docker 镜像文件：

&#x20;`sudo docker build -t quorum`

#### Step 1 - 运行 Quorum

在本步骤，我们将在服务器上运行 Quorum 服务。

在你的 `quorum` 二进制文件所在的根目录执行以下命令：

{% code overflow="wrap" %}
```shell
./quorum fullnode --listen /ip4/0.0.0.0/tcp/{portNumber} --listen /ip4/0.0.0.0/tcp/{websocketPortNumber}/ws --apiport {apiPortNumber} --peer /ip4/94.23.17.189/tcp/10666/p2p/16Uiu2HAmGTcDnhj3KVQUwVx8SGLyKBXQwfAxNayJdEwfsnUYKK4u --configdir peerConfig --datadir peerData --keystoredir keystore --debug=false
```
{% endcode %}

将命令中的：`{portNumber}，{websocketPortNumber}，{apiPortNumber}，`替换成不常用的端口号。

如果服务器有防火墙，请记得将以上端口对外开放。

初次运行 `quorum` 会请你生成一个密码，请注意妥善保管。再次运行时会用到。

#### Step 2 - 获取 jwt token

在本步骤，我们把即将用来运行 Rum App 的参数保存到本地。

如果你在上一步骤将 `quorum` 运行于 `tmux` 或者 `systemd` 系统服务中，你可以直接到 `quorum` 所在根目录继续执行命令获取 `jwt token`，否则你可能需要停掉正在运行的 `quorum` 服务。

在你的 `quorum` 二进制文件所在的根目录执行以下命令：

{% code overflow="wrap" %}
```shell
./quorum jwt create --configdir {quorumdir}/peerConfig chain --name node-all-groups
```
{% endcode %}

将命令中的`{quorumdir}` 替换成 `quorum` 所在的目录。

成功执行命令后，将 `token:` 后面的文本复制并保存到本地电脑，我们将在下一步用到它。

#### Step 3 - 用 Rum App 远程连接 Quorum 服务

在本步骤，我们将通过本地 Rum App 连接远程的 Quorum 服务，对节点进行管理。

首先请[下载 Rum App](ying-yong-xia-zai.md#rum-app) 并安装在本地电脑。

运行 Rum App，在启动界面选择“外部节点”。

![](<.gitbook/assets/Screenshot (49).png>)

&#x20;根据提示选择一个本地文件夹用于储存本地数据之后，接下来是按要求填写参数：

![](<.gitbook/assets/Screenshot (50).png>)

第一个空格填入 `http://{服务器IP地址}:{apiPortNumber}`

第二个空格填入第二步取得的 `jwt token`

点击“确定”按钮，等待一段时间就可以成功通过本地 Rum App 远程连接外部全节点了。

#### 结语

如果出现问题可以尝试：

1. 用 `./quorum -help` 来查看命令行有没有错误。
2. 注意防火墙是否已经解除对各端口的屏蔽。
3. 如果反复尝试失败请打开 Rum App 的调试功能检查问题所在。

[本文档是否有帮助？请到 GitHub 提交 issue 或 pull request，感谢您的帮助。](zen-yang-yun-hang-yi-ge-quan-jie-dian.md)
