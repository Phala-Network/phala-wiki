---
title: 抵押池委托指南
draft: false
---

## 委托人

持币者（即委托人, Delegator）可通过将 PHA 委托给抵押池分得挖矿收益（类似于存币生息）。本指南将介绍如何在 Phala Network 上通过委托代币赚取收益。

## 如何委托

### 委托简介

Phala Network 协议允许持币者将 PHA 委托给抵押池，由抵押池的所有者使用 PHA 代币去挖矿抵押，获得的挖矿奖励会按委托份额占比分给委托人。

### 选择抵押池

进入 [Phala Network 委托页面](https://app.phala.network/delegate/)

![stakepool delegate](/images/docs/khala-user/stakepool-delegate.jpg)

委托页面首页是抵押池的详情列表，用户可以在该页面挑选抵押池并进行委托，该列表中所有数据均支持排序。关于如何选择抵押池，可参考以下建议：

1. 关注各数值的实时值，这里主要参考 **APR** 和 **Cap Gap** 这两个数值，分别代表实时年化收益率和剩余额度；

    $$
    APR = \frac{\text{抵押池1天应得的奖励} \times 365 \times (1-\text{国库手续费}) \times (1-\text{抵押池手续费})}{\text{抵押池总委托量}}
    $$

    - 其中，Worker 奖励规则请参见 [Phala Network 经济模型](https://mp.weixin.qq.com/s/E9p9619CS50T3oE_-QSIuQ)，国库手续费率是固定值为20%；抵押池手续费率由抵押池所有者所设置，这意味着 Worker 产出的挖矿奖励会按照一定比例返还给抵押池的所有者。
    - Cap Gap（剩余额度）：即某单个抵押池当前可接受的委托资金的最大值

2. 如果对资金灵活性要求较高，可以关注抵押池的**闲置资金**情况，以判断所委托资金所需的提取时长。

### 开始委托

点击抵押池后方的 「Delegate」按钮，在弹窗中输入委托资金数额，点击 「Confirm」提交交易，等待签名成功即可完成委托。

![stakepool delegate action](/images/docs/khala-user/stakepool-delegate-action.jpg)

## 查看委托情况

![stakepool claim](/images/docs/khala-user/stakepool-claim.jpg)

1. 点击 「My Delegate」，可查看自己的委托抵押池及其情况，其中 Claimable Rewards 是当前可以领取的奖励，**该奖励长期有效，直至被领取**。
2. 点击 「Delegate」， 可以再次向该抵押池委托。
3. 点击 「Withdraw」，可从该抵押池中提取委托资金，资金解锁所需时长与抵押池中**闲置资金量**相关。如果闲置资金充足，提交提取的资金可立即全部解锁；如果闲置资金不足，极端情况下，最长需要14天才能完成全部解锁。

## 风险提示

对于委托人来说，风险主要表现在以下方面：

1. 放入抵押池的资金提取出来有时间延迟，延迟时间长短与抵押池中的闲置资金量相关，最长需等待**14**天；
2. 极端情况下，如若 Worker 被惩罚过多，抵押池**资金会整体缩水**，继而影响到抵押池内的本金，不过这是**较低概率事件**。

<script>
  MathJax = {
    tex: {
      inlineMath: [['$', '$'], ['\\(', '\\)']],
      displayMath: [['$$','$$'], ['\\[', '\\]']],
      processEscapes: true,
      processEnvironments: true
    },
    options: {
      skipHtmlTags: ['script', 'noscript', 'style', 'textarea', 'pre']
    }
  };
  window.addEventListener('load', (event) => {
      document.querySelectorAll("mjx-container").forEach(function(x){
        x.parentElement.classList += 'has-jax'})
    });
</script>
<script type="text/javascript" id="MathJax-script" async
  src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-mml-chtml.js"></script>
