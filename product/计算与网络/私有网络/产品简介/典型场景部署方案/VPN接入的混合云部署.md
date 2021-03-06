根据您不同的连接需求，腾讯云分别提供两种服务连接您的企业数据中心和私有网络：VPN 连接和专线接入，主要区别如下：
- [VPN 连接](https://cloud.tencent.com/product/vpn.html) 利用公网和 IPsec 协议在您的数据中心和私有网络之间建立加密的网络连接。VPN 网关的购买、生效和配置可以在几分钟内完成。但是 VPN 连接可能会受到 Internet 抖动、阻塞等公网质量问题而中断，当用户业务对网络连接质量要求不高时，是一种快速部署的高性价比选择。
- [专线接入](https://cloud.tencent.com/product/dc.html) 则提供了一个您专用的专线网络连接方案，施工时间较长，但可以提供高质量、高可靠的网络连接服务，当您的业务对网络质量和网络安全要求较高时，可以选择此方案进行部署。

下面将介绍如何使用VPN连接部署混合云：

## 应用场景
VPN 连接（VPN Connections）是一款通过 IPsec 加密通道连接您的企业数据中心和腾讯云私有网络（VPC）的服务，提供安全、可靠的加密通信。
目前 VPN 通道支持 IKE/IPesec 加密协议，最大支持 100M 带宽，如果您有特殊需求，我们也可以提供更符合您需求的定制化 VPN 接入服务,请填 [工单申请](https://console.cloud.tencent.com/workorder/category/create?level1_id=6&level2_id=168&level1_name=%E8%AE%A1%E7%AE%97%E4%B8%8E%E7%BD%91%E7%BB%9C&level2_name=%E7%A7%81%E6%9C%89%E7%BD%91%E7%BB%9C%20VPC)。

> 注：您需要为 VPN 通道占用的公网带宽付费，如果您的混合云互通带宽要求大于 200M，建议您选择情 [专线接入的混合云](https://cloud.tencent.com/document/product/215/7543) 部署方案。

VPN 通道的网络质量依赖于公网连接的网络质量，建议您在选择部署私有网络的地域时优先测试一下自有 IT 资源与腾讯云不同地域的网络时延，从而获得最优的混合云部署架构。

## 解决方案
**云上数据中心**：在腾讯云创建的某个私有网络，部署云上数据中心。
**连接方式**：VPN 连接。
**网段规划**：物理数据中心与需要连接的私有网络之间**网段不可重叠**。


## 操作步骤
在腾讯云创建的某个私有网络，部署云上数据中心，单击和查看 [操作指南](https://cloud.tencent.com/document/product/215/4927#.E6.93.8D.E4.BD.9C.E6.8C.87.E5.8D.97)，这里不重复说明。

**VPN 连接** 可以在控制台实现全自助配置，您需要完成以下几步才能实现使 VPN 连接生效：
1. 创建 VPN 网关
2. 创建对端网关
3. 创建 VPN 通道
4. 在自有 IPsec VPN 网关中加载配置文件
5. 设置路由表
6. VPN 通道激活
[单击查看详情](https://cloud.tencent.com/doc/product/215/4956#.E5.BF.AB.E9.80.9F.E5.85.A5.E9.97.A8)
