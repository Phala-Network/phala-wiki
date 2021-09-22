---
title: Khala 链上身份
---

Phala Network 提供了一个身份系统，允许参与者将个人信息添加到他们的链上账户，然后要求[身份裁判](#身份裁判)验证这些信息。在你的身份被添加后，可以应用到Phala或Khala的很多应用场景中，比如在Secure Worker 挖矿的场景中，这对吸引委托人给你投票将起到一定的信任作用。

![](/images/docs/khala-user/identity-0.png)

## 设置链上身份

用户可以通过注册默认字段（例如真实姓名、显示名称、网站、Twitter 昵称、Riot 昵称等）以及一些他们想要证明的额外自定义字段来设置身份（请参阅[身份验证](#身份验证)）

用户必须在绑定预留资金，才能将他们对于每个法定名称以外的字段信息存储在链上：`identity_reserve_funds` 和 `identity_field_funds` 
这些资金是锁定而不是消耗——当身份被清除时，它们会被退回。

金额数量也可以通过 polkadot.js/apps 上的 [Chain state constants](https://polkadot.js.org/apps/#/chainstate/constants) 功能查询来读取。

首先选择`identity` 作为 `selected constant query`.

然后在右侧可以通过单击栏末尾的“加号”图标来选择要查看的参数并将它们添加到网页上显示。

每个字段最多可存储 32 个字节的信息，因此数据必须小于该值。通过 [Extrinsics UI](https://polkadot.js.org/apps/#/extrinsics) 手动输入数据时，[UTF8 to bytes](https://onlineutf8tools.com/convert-utf8-to-bytes) 转换器可以提供帮助。

添加身份信息的最简单方法是单击帐户旁边的齿轮图标，然后选择`设置链上身份`。

![](/images/docs/khala-user/identity-1.png)

出现的弹窗会提供一些默认身份信息的选项。

![](/images/docs/khala-user/identity-2.png)


要添加默认字段之外的自定义字段，请使用 Extrinsics UI 功能。首先单击`添加项目`并添加你喜欢的任何字段名称来提交原始交易信息。下面的示例添加了一个字段 `steam`，它是用户的 [Steam](https://store.steampowered.com) 昵称。第一个值是以字节为单位的字段名称（“steam”），第二个是以字节为单位的帐户名称（“theswader”）。你还必须提供显示名称，否则，如果我们在仍然选择`无`选项的情况下提交它，身份模块会认为它已被删除。也就是说，每次对字段进行更改时，都需要重新提交整个字段集：写入操作始终是`覆盖`，而不是`追加`。

![](/images/docs/khala-user/identity-3.png)

请注意任何自定义的身份字段在UI上都不是默认显示的。

此类自定义身份信息的呈现最终取决于 UI/dapp 开发者。在 PolkadotJS 上，团队倾向于只显示官方字段。如果要检查自定义值是否仍然存储，请使用 [Chain State UI](https://polkadot.js.org/apps/#/chainstate) 查询活跃帐户的身份信息：

![](/images/docs/khala-user/identity-4.png)

然后由你自己的 UI 或 dapp 来随心所欲地处理这些数据。数据仍可通过 Polkadot API 进行查询，因此您不必依赖 PolkadotJS UI。

对于高级玩家，你最多可以定义100个身份字段。

### 格式提醒

请注意以下提醒：

因为字段支持不同的格式，从原始字节到各种散列，UI 无法告诉如何对遇到的给定字段进行编码。 PolkadotJS UI 当前将它遇到的原始字节编码为 UTF8 字符串，这使得这些值在屏幕上可读。但是，鉴于对可以放入这些字段的值没有限制，不同的 UI 可能会将它们解释为，例如 IPFS 哈希或编码位图。这意味着任何存储为原始字节的字段都将无法被该特定 UI 读取。随着格式标准的具体化，使用体验将变得更容易，但就目前而言，显示用户信息的每个自定义实现都可能必须对采用的方法做出主动操作，或者支持多种格式，然后尝试多种编码，直到输出内容合理为止.

## 身份验证

用户在链上提交身份信息后，可以向身份裁判请求判断。用户在链上声明自己愿意为判断支付的最高费用，比这个价格低的身份裁判可以提供身份验证。

当身份裁判提供身份验证时，他们可以选择最多六档置信度：
- 未知：默认值，暂未判断。 
- 合理：数据看似合理，但没有进行深入检查（例如正式的 KYC 流程） 执行。 
- 已知良好：身份裁判已证明信息正确。 
- 过时：身份信息曾经很好，但现在已经过时了。 
- 低质量：身份信息质量低或不准确，但可以通过更新进行修复。 
- 错误：身份信息错误，可能是恶意申请。 

第七个状态是`已付费用`，用于当用户请求验证并且正在进行中时的状态。处于这种状态或`错误`的信息是无法修改的；它只能通过完全去除身份来去除。


身份裁判通过进行合理验证来才能获取社区信任，同时也可能会因发布错的验证而被其他人取代。 要在提交身份信息后进行身份验证，请转到 ["Extrinsics UI"](https://polkadot.js.org/apps/#/extrinsics) 并选择 `identity` 模块，然后选择 `requestJudgement`。给`reg_index` 输入您想要被提供验证服务的身份裁判编号，给 `max_fee` 输入你愿意为这些验证确认支付的最高金额。 如果你不知道选择哪个身份裁判，请首先通过转到 ["Chain State UI"](#) 并选择 `identity.registrars()` 来查看可用的身份裁判以获取完整列表。

### 申请验证

无论你是在 Khala 还是 Phala 网络上，请求验证都遵循相同的过程。从你在上面进行的查询中选择身份裁判之一。

![](/images/docs/khala-user/identity-5.png)

以上操作将使你的身份从未经验证变为`“等待验证”`： 此时，需要与身份裁判直接联系 - 联系信息应该已经在他们的身份中，如上所示。每个身份裁判都有自己的一套程序来验证您的身份和价值，只有在你满足他们的要求后，该过程才会继续。 身份裁判确认身份后，您的帐户名称旁边应该会出现一个绿色对勾，并展示对应的身份置信度：

![](/images/docs/khala-user/identity-6.png)

_请注意，即使在验证后更改单个字段的值也会取消你的帐户验证信息，你将需要重新开始验证过程。但是，你仍然可以在验证过程中更改字段 - 由身份裁判负责来关注是否被更改了。_


### 取消身份验证

当你决定不想接受身份裁判的验证时（例如你意识到输入了错误的数据或选择了错误的身份裁判）。在这种情况下，在提交验证请求后且在你的身份被验证之前，可以使用外部调用取消判决。

首先跳到 ["Extrinsics UI"](https://polkadot.js.org/apps/#/extrinsics) 并选择 `identity` 模块，然后选 `cancelRequest`。确保你从正确的帐户（你最初请求验证的帐户）调用这个交易函数。对于`reg_index`，输入你在使用的身份裁判编号。 提交交易后，之前请求的验证就会被取消。



### 身份裁判

Khala上将会存在多个身份裁判，但是当前只有一个身份裁判：

- 身份裁判#0 :
  - 名字：Marvin Tong
  - 联系方式: marvin@phala.network
  - 地址: 42McARWf3FMrGa2RnhhVansbCHuRktiiyHpjsW5nDkDefwHr
  - 费用: 5 $PHA


在邮件申请中请遵循下面的格式:

> 我在申请Khala网络的身份验证，信息如下:

```
下面只是示例，实际只需提交你在链上已经更新的身份信息即可
{
  "account": "42McARWf3FMrGa2RnhhVansbCHuRktiiyHpjsW5nDkDefwHr",
  "display": "Marvin Tong",
  "web": "https://phala.network",
  "email": "marvin@phala.network"
  "twitter": "https://twitter.com/marvin_tong"
}
```
身份裁判#0 将会回复下一步怎么做

## 子账号

用户还可以通过在主账号下设置“子账号”来关联账号，每个子账号都有自己的身份。系统为每个子账户预留一部分保证金。比如你可以用子账号来运行多个Secure Worker。比如一个实体叫“未来云计算”，他可以注册多个子账户来代表每个验证者的 Stash 账户。

一个账户最多可以有 100 个子账户。 要在现有帐户上注册子帐户，你当前需要使用 [Extrinsics UI](https://polkadot.js.org/apps/#/extrinsics)。在那里，选择身份模块，然后选择`setSubs`作为要使用的交易函数。对于要添加到父发件人帐户的每个子帐户，单击` Add Item`。放入每个父级的身份数据字段的值是子帐户的可选名称。如果忽略，子账户将继承父名并显示为`parent/parent` 而不是`parent/child`。

![](/images/docs/khala-user/identity-7.png)

请注意，每个子帐户都需要存入 `identity_sub_reserve_funds`。 您可以再次使用 [polkadot.js/apps](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Frpc.polkadot.io#/chainstate/constants) 通过查询来验证`identity.subAccountDeposit` 的金额数值。

## 清空和注销身份信息

**清空：**

用户可以清空自己的身份信息，退还押金。清除身份也会清除所有子账户身份并返还对应的押金。 

要清除身份的步骤： 
1. 导航到 [帐户 UI](https://polkadot.js.org/apps/#/accounts)。 
2. 点击您要清除的账户对应的三个点，然后选择`设置链上身份`。 
3. 选择`清除身份`，并签署并提交交易。 

**强制注销：** 议会可以强制注销它认为是错误的身份。这导致押金被罚没。
