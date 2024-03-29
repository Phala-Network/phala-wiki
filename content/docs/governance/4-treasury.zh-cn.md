---
title: "4 Khala国库"
---

## 国库支出分类

### 国库支出提案

国库资金可以用于资助社区成员提交的支出提案。这些提案需要得到理事会的批准，其目标是开发出能够推动网络、维持基础设施部署、安全运营和营销活动等的想法。

### 小费

任何持有PHA的用户都可以提出小费提议，并得到理事会成员的认可。小费没有固定的金额价值：最终的金额价值是根据理事会发布的所有小费的中位数来决定的。

### 国库赏金机制

通过将支出提案的策展活动委托给一位称为管理人的专家来实现。

## 国库

国库是通过交易手续费、惩罚、Secure Worker 挖矿 (20%) 等方式积累的一部分资金。用户可以通过提出“支出提案”来获取国库资金，如果该提案获得[理事会](https://wiki.phala.network/zh-cn/docs/governance/2-join-the-council/)的批准，分发前会进入等待期，此等待期称为预算期，其持续时间由[治理](https://wiki.phala.network/zh-cn/docs/governance/)机制约束，当前默认设置为 1 天。国库试图在不耗尽资金的情况下在队列中花费尽可能多的提案。

如果国库结束一个预算期而没有花光所有的资金，它就会损失一部分资金——从而造成通缩压力。该百分比目前在 Phala 上为 0%。

当提案人希望从国库中获取资金时，他们必须质押至少为提案资金的5%的存款（其他情况请参见下文）。如果提案被拒绝，这笔押金将被罚没，如果提案被批准，将退还给提案人。

提案可能包括（但不限于）：

- 基础设施部署和持续的运维。
- 网络安全运营（监控服务、持续审计）。
- 生态系统建设（与友好链的合作）。
- 营销活动（广告、付费功能、合作）。
- 社区活动和对外活动（聚会、披萨派对、黑客空间）。
- 软件开发（钱包和钱包集成、客户端和客户端升级）。

国库最终由 [理事会](https://wiki.phala.network/zh-cn/docs/governance/3-voting-for-councillors/) 控制，资金的使用方式取决于他们的判断。

国库的资金分别来自于以下的来源：

1. 罚没：当Secure矿工或守门人因任何原因被罚没时，惩罚金额将被发送到国库，随之发送的还有将发送给报告验证人的实体（另一个验证人）的奖励。 奖励来自罚没资金，并因违规原因和和报告人数而异。
2. 交易手续费：每个区块的交易手续费有一部分上缴国库，其余部分上缴区块生产者。
3. Secure Worker Mining reward: 70%的PHA token将被用于Secure Worker Mining，且跟随Khala和Phala的网络进度，每个区块产生的挖矿奖励的20%将被用于资助国库。

## 创建国库支出提案

提案人必须存入提案金额的 5% 或 1 PHA（以较高价者为准）作为防止垃圾提案措施。 如果提案被拒绝，则此金额将被销毁，否则将予以退还。 这个质押数值受到[治理](https://wiki.phala.network/zh-cn/docs/governance/) 的约束，因此将来可能会发生变化。

请注意，用户无法在提交支出提案后撤消。 理事会将选择接受或拒绝该提案，如果该提议被拒绝，则质押资金被烧毁。

### 公布提案

为了尽量减少链上的存储，提案不包含具体文字信息。 当用户提交提案时，他们需要找到一种链下方式来解释他的提案。 大多数讨论发生在以下平台上：

- 建议财政类提案先在Phala论坛发布文本内容，说明细节。不发布文本内容的将不会被作为严肃提案讨论。
- 将论坛的提案内容在 [Phala Discord #Proposal 频道](https://discord.gg/YsRUMpvvxs) 或 [Phala 论坛](https://forum.phala.network/c/51-category/51) 中传播，以便更多的社区成员讨论 。

传播提案的解释最终由提案人决定 - 推荐的方式是使用官方 Discord 频道，如 [Phala Discord #Proposal 频道](https://discord.gg/YsRUMpvvxs)。

### 创建提案

创建提案的一种方法是使用 Polkadot-JS 网站。 在网站上，可以选择两种方式创建提案。一种方式为：点击[extrinsics](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fkhala.api.onfinality.io%2Fpublic-ws#/extrinsics) 标签——选择“`Treasury`“——选择`proposeSpend`，然后支出并输入所需金额和收款人：

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/e93580b2-3f2b-461d-b965-5052b3c289ec/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211012T151234Z&X-Amz-Expires=86400&X-Amz-Signature=7cf895d1494adccd6ed73afd2842c17054f1c4e2c9d38a577360bde8cba89306&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

另一种方式为：点击Governance——[Treasury](https://polkadot.js.org/apps/?rpc=wss%3A%2F%2Fkhala.api.onfinality.io%2Fpublic-ws#/treasury)标签，直接点击“提交提案”按钮：

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7f02ba48-6e65-40df-9b3a-49e0bb916c58/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211012T151311Z&X-Amz-Expires=86400&X-Amz-Signature=f4eb43ec8cb952c5688448c41f6d11dc4f8694361d360b7242b317b98a82665e&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

系统将自动收取[上述]()两个值中较高的一个值作为质押金额。

一旦提案被创建，您的提案将展示在国库板块，并且理事会可以开始对其进行投票。

请记住，提案没有元数据，因此需要由提案人自行创建提案描述和提案目的，便于理事会研究本提案并根据其投票。

此时，理事会成员可以提出动议来接受或拒绝支出提案。 有可能同时创建一个接受动议和另一个拒绝动议。 接受提案的比例和拒绝提案的比例不同，并且可能取决于财政部实施的网络。

接受支出提案的门槛至少是理事会的五分之三，拒绝提案的门槛至少是理事会的一半。

![](https://s3.us-west-2.amazonaws.com/secure.notion-static.com/b2c92e99-901b-4d77-8b8a-60316105b95e/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAT73L2G45O3KS52Y5%2F20211012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20211012T151334Z&X-Amz-Expires=86400&X-Amz-Signature=632e4b71de957cfb823820681e131bc78d42890e24ae12cd872598559034dc6a&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22)

## 小费

除了支出提案，国库有支出小费的独立系统。小费请求可以由任何人提出，并由理事会支持。小费没有固定金额；小费的最终金额是根据小费发出者希望打赏的小费的中位数决定。

目前，小费发出者即理事会成员。然而，成为小费发出者并不是理事会的直接责任，某些时间小费发出者可能与理事会属于不同账户组。

当超过一半以上的小费组认可一个小费请求时，小费将进入结束阶段。在此期间，小费组的其他成员仍然可以发布他们希望打赏的小费，但不是必须的。技术窗口关闭后，任何人都可以调用外部的 `close_tip`请求，小费将被支付。

小费有两种类型：公开小费和小费发出者发起的小费。对于公开小费，需要质押一小部分保证金。该保证金取决于小费消息的长度，以及链上定义的固定质押常数，目前为 1。公开小费有 20% 的发起人奖励，该奖励从小费的总金额中支付。小费发出者发起的小费请求，即理事会成员发起的小费请求，没有发起人费用也无需质押保证金。

为了更好地理解小费在支付之前所经历的过程，让我们考虑一个例子。

### 案例

Bob为 Phala 做了一些很棒的事情。 Alice注意到了这一点，并决定报告Bob应该得到一些国库小费。 理事会由Charlie、Dave和Eve三名成员组成。

Alice通过发出 `report_awesome` 请求开始申请小费。 这个申请需要两个参数，一个是申请原因和接收小费的地址。 Alice提交了Bob收款地址，申请原因提交了 [Phala 论坛](https://forum.phala.network/c/51-category/51) 上的帖子的 UTF-8 编码链接，链接中的内容解释了为什么 鲍勃值得获取小费。

如上所述，为了发布小费申请报告，Alice还必须锁定一定的资金。 锁定资金是在链参数列表中设置的基础存款加上原因中包含的每字节额外存款。 这就是为什么Alice提交了一个外部链接作为申请原因而不是直接的文字说明，这样做的申请费更便宜。

对她来说，如果小费得到小费发放者的批准，Alice可以申领到小费发起者奖励。

由于小费小组与理事会相同，因此理事会现在必须集体（但也是独立地）决定Bob应得的小费价值。

Charlie、Dave和Eve都查看了报告，他们根据自己对Bob为Phala 提供贡献的评估给出了小费估价。

Charlie的小费估价为10 PHA，Dave的小费估价为30 PHA，Eve的小费估价为100 PHA。

当三个小费发放者中的两位提供了小费估价后小费便可以进入关闭期。一旦小费小组中超过半数人发布了小费估价，关闭小费的倒计时将开始。在本案例中，第三个小费发放者在关闭期结束前发布了他们的小费，因此三个人都能够公开他们的小费估价。

现在支付给Bob的实际小费是所有小费估价的中位数，因此Bob将从国库获得30 PHA小费。

为了让Bob获得他的小费，一些帐户必须在小费的关闭期结束时调用外部的 `close_tip`请求。任何人都可以调用这个外部请求。

## 赏金支出

对于国库提案，理事会成员的管理能力存在实际限制：理事会成员可能不具备对所有提案中描述的活动进行适当评估的专业知识。即使个别议员具有这种专业知识，大多数成员也极不可能有能力处理如此多样化的主题。

赏金支出提案旨在将支出提案的策划活动委托给称为赏金经理的专家：他们可以定义为在国库的一部分上具有代理权的地址，目标是修复错误或漏洞、制定策略或监控一组与特定主题相关的任务：所有这些都是为了 Phala 生态系统的利益。

提议者可以提交赏金提议供理事会通过，随后指定赏金经理，这位赏金经理的背景和专业知识能够确定任务何时完成。赏金经理在赏金提案通过后由理事会选出，他需要增加押金才能担任该职位。如果他们有恶意行为，这笔存款可用于惩罚他们。然而，如果他们成功完成了让某人完成赏金工作的任务，他们将收到押金和赏金奖励的一部分。

在提交赏金的价值时，提案人需要提交包含对愿意在任务中投入时间和专业知识的赏金经理奖励的赏金价值：赏金经理奖励包含在赏金的总价值中。 从这个意义上说，赏金经理奖励是从赏金总价值中减去赏金奖励后的结果。

一般而言，赏金经理应该在悬赏计划试图解决的问题上拥有良好且平衡的背景和知识：他们至少应该了解悬赏计划涉及的主题，并同时展示项目管理技能或经验。 赏金经理拥有背景的这些推荐可以确保赏金计划的顺利进行。 赏金支出是对特定工作或特定目标集的奖励，需要执行以支付预定义的国库金额。 一旦指定的一组目标完成，分配支付地址的责任就委托给赏金经理。

理事会激活一项赏金支出后，它将需要专业知识的工作委托给可以关闭活跃赏金的赏金经理。关闭活动赏金会延迟支付到支付地址和赏金经理的费用。延迟阶段允许理事会在出现任何问题时采取行动。

为了将链上的任何提案以相同方式最小化链上存储，赏金提案中将不包含上下文信息。当用户提交赏金支出提案时，他们可能需要找到一种链下方式来解释该提案（任何可用的社区论坛都可以）。 [此模板](https://docs.google.com/document/d/1dQtjfFTkOcTXTt8pIpH2GYLNc9vc03teYfR1r8NfGO0/edit?usp=sharing) 可以作为所有必要信息的核对清单，这将帮助委员会做出明智的决定。

赏金的预定期限为 90 天，并有可能由赏金经理延长。为了保持任务管理的灵活性，赏金经理将能够在机制的下一次迭代中创建子赏金以将任务增加颗粒度和分配。

### 创建赏金提案

任何人都可以使用 Polkadot.JS 创建赏金提案：用户可以在治理标签下的专用赏金部分提交提案。 在 Phala 应用程序中查看和管理赏金的用户界面的开发仍在开发中，它将为理事会成员、赏金经理和赏金的受益人以及所有观察链上资金管理的用户提供服务。 目前，需要议员帮助发起理事会动议来开始赏金提案。

要提交赏金，请访问 [Polkadot JS](https://polkadot.js.org/apps/#/bounties)，然后点击网站顶部选项栏中的“治理”选项卡。之后，点击“赏金”并在界面的右上角找到“+添加赏金”按钮。 填写赏金标题、请求分配的金额（包括赏金经理费用）并确认。

在此之后，理事会成员将协助您创建理事会动议并投票以赏金提案。 您可以通过加入 Discord 服务器中的 #Khala-Direction [频道](https://discord.gg/MsqTGfu5Uy) 并发布对赏金提案的简短描述并附上论坛帖子链接方便理事会成员获取提案详情。

可以通过删除特定国库金额的专项资金来取消赏金。如果任务已完成则关闭赏金。 另一方面，可以通过修改赏金的到期块数以保持活跃状态来延长赏金的 90 天生命周期。

### 关闭赏金

赏金经理一旦批准完成任务，就可以关闭赏金。 赏金经理应确保事先在活动赏金上设置支付地址。 关闭活动赏金会延迟支付到支付地址和赏金经理费用。

可以通过选择国库选项卡，然后选择`Award_bounty`来关闭赏金，确保关闭正确的赏金并最终签署交易。 需要注意的是，在赏金完成后获得奖励的人，必须在赏金经理关闭分配后通过调用`Claim_bounty`从支付地址领取特定金额的支付。

## FAQ

### 是什么阻止了国库被理事会的大多数成员控制？

理事会的大多数成员可以决定国库支出提案的结果。如果一种敌对的心态出现，我们需要考虑到理事会可能会在某个时候作恶并试图窃取所有国库资金的可能性。有一种可能性是，随着国库的资金规模变得越来越大，会出现大量的金钱诱惑。

在 Phala 中，理事会主要由社区的知名成员组成。请记住，理事会由代币持有者投票选出，因此他们必须进行一些竞选活动或以其他方式获得认可才能获得选票。在发生袭击的情况下，理事会成员将失去他们的社会信誉。此外，理事会成员通常受到Phala链正常运作的额外激励。这种额外激励要么是因为他们经营依赖于链的业务，要么是因为他们（通过他们持有的）代币而获得直接的经济收益。

具体来说，有几种链上方法可以抵抗这种攻击。一，理事会的多数可能不是多数持币者。这意味着，如果他们试图进行攻击，多数持币者可以投票更换理事会——甚至扭转国库支出。他们将通过正常的公投来做到这一点。二，是资金支出存在时间延迟。它们仅在每个花费期间制定。这意味着将有一些时间来观察这种正在发生的攻击行为。然后，时间延迟允许链参与者有时间做出响应。响应可能以治理的形式出现，或者 - 在最极端的情况下，清算他们的资产并迁移到少数分叉上。但是出现这种情况的可能性非常低。
