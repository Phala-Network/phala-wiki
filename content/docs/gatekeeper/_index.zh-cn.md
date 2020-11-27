---
title: "守门人教程"
weight: 3
draft: false
---

## 什么是守门人

守门人（Gatekeeper）是对 Phala 网络至关重要的角色。守门人负责区块的打包和密钥管理，是意外情况下保证网络可用性的重要途径。守门人需要使用性能较好的设备、在网络情况良好的环境登录，且必须时刻保持在线。因此，**守门人可以获得可观的收益**，但同时需要对自己和提名人的PHA抵押额负责。如果因为频繁掉线或其他不良行为导致被惩罚，则无论是名誉还是PHA损失都会是巨大的。

运行守门人节点，安全是第一要素。你可以查看[搭建安全Polkadot验证人节点](https://wiki.polkadot.network/docs/en/maintain-guides-secure-validator)查看可能影响守门人安全运行的因素。Web3 基金会也维护了一个你可以自行部署的[验证人节点参考实现](https://github.com/w3f/polkadot-secure-validator)（[此处](https://www.youtube.com/watch?v=tTn8P6t7JYc)有视频教程)。在配置守门人节点的过程中，可以把这个代码库当作 _一个初始模版_ ，在上面自行修改、裁剪。

如果您有任何疑问，可前往 [Phala论坛中文区](https://forum.phala.network/c/9-category/9) ，按照此 [模板](https://forum.phala.network/t/topic/461/2) 进行提问，我们将在看到后第一时间回复您。

### 守门人需要抵押多少 PHA

在测试网上做守门人需要抵押 **10** 个测试币。

[点此查看如何领取测试币](https://www.yuque.com/docs/share/e016c4c3-bc63-4689-920e-6f7c0434e4c0?#)

您可以在 [这个页面](https://wiki.polkadot.network/docs/en/faq#what-are-the-ways-to-find-out-the-minimum-stake-necessary-for-the-validators) 找到估算所需 PHA 数额的方法。
守门人将根据 [Phragmen算法](https://wiki.polkadot.network/docs/en/learn-phragmen) 进行选举。要想入选，您和[提名人](https://wiki.polkadot.network/docs/en/maintain-nominator)的保证金之和必须不低于其他守门人。赢得提名人的支持非常重要。此外，也要注意在您的 Stash 账号和 [Controlloer 账号](https://wiki.polkadot.network/docs/en/learn-keys)上都要留有足够的 PHA。

> 例：
> - 守门人A抵押了 10 PHA 作为保证金，他的提名人为他抵押了 100 PHA。则守门人A的总保证金为 110 PHA
> - 守门人B抵押了 100 PHA，但没有提名人为他抵押
> - 入选的会是A

需要注意，一旦将您的一部分资金设为保证金，</br>

**它们可能会因为您的不良行为或操作不当而被罚没**。</br>
**它们可能会因为您的不良行为或操作不当而被罚没**。</br>
**它们可能会因为您的不良行为或操作不当而被罚没**。</br>

因此再次建议您仔细阅读此篇教程的操作。

此外，

**请妥善保管您的助记词**。</br>
**请妥善保管您的助记词**。</br>
**请妥善保管您的助记词**。</br>
