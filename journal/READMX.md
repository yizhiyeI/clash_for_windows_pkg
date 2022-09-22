# 区块链
  ezos也正在拥抱以Rollup为中心的路线图；NEAR也正在对数据可用性分片进行设计。本文主要讨论以太坊的模块化趋势。
  
  * ![图片](https://user-images.githubusercontent.com/79394963/191827905-ba9e72b5-e587-4a76-bf29-a704f10c4b18.png)
  
  “区块链不可能三角”又限制了所有属性同时实现极致发展的可能，所以单纯地基于单片区块链的思路做扩容是无法解决以太坊的困境的。
  
  * ![图片](https://user-images.githubusercontent.com/79394963/191828043-46f757a1-6202-40fa-8800-851d8ca77863.png)
  
  
  * 模块化混合扩容：layer1（data sharding）+layer2（rollups）

架构设计主要分为四层：执行层、结算层、共识层、数据可用性层。目前很多情况下，行业内也会把执行层和结算层统一称执行层，共识层和数据可用性层统一称为共识层。

* ![图片](https://user-images.githubusercontent.com/79394963/191828358-1f157787-ed51-45b0-9b82-6c0cd8aeafb1.png)

执行层（Execution Layer）：负责处理链上交易、执行链上订单并验证转账和智能合约的执行，主要将以Rollup为主。模块化区块链发展到一定阶段后，用户通常是基于执行层与区块链进行交互，包括签名交易、部署智能合约以及转移资产等。执行层解决了区块链的可扩展性。

结算层（Settlement Layer）：结算层用于验证Rollup等执行层的执行结果以及解决争议，并结算出状态承诺。

共识层（Consensus Layer）：共识层通过全节点网络下载和执行区块的内容，就状态转换的有效性达成共识，从而提供排序和最终确定性，并以PoS机制验证出块。

数据可用性层（Data Availability Layer）：保证交易数据可以被使用（保证存储且可验证与可用）。需要将验证状态转换有效性所需的数据发布并存储在这一层，一旦遭遇恶意区块提议者扣留交易数据的事件，数据可用性层的数据可用作验证。

* ![图片](https://user-images.githubusercontent.com/79394963/191828523-3cfd2015-98b4-4466-b94f-ab2ba822a573.png)

anksharding引入的核心机制主要：PBS和DAS。

PBS（Proposer builder separation）是指构建区块时区块提议者（Proposer）和区块构建者（builder）分离。由Proposer提议区块，Builder竞拍交易的排序权并计算区块头，Proposer根据Builder的计算结果打包交易并将区块头写入区块完成出块。在PBS之前的区块提议者（Merge前是Miner，Merge后是Validator）可以通过查看mempool中有哪些交易并采取一些策略来获得MEV的机会以最大化他们的挖矿收益。引入PBS机制后，这种角色分离机制结合Builder排序权的竞拍机制可以一定程度上解决MEV问题，最后的MEV收益相当于会被全网验证者共享。除此之外，PBS还有助于解决分片与信标链的同步问题、以太坊网络的抗审查问题等。

* ![图片](https://user-images.githubusercontent.com/79394963/191828675-01b6252f-2ea9-4ad6-9732-688494add75c.png)
DAS（数据可用性抽样，Data Availability Sampling）是解决区块链状态爆炸的有效方法。让验证节点检查区块可用性，通过使用DAS检查，轻客户端可以通过仅下载一些随机选择的块来验证一个块是否已发布。由于DAS可以对区块数据做并行化验证，所以未来数据分片（Data Sharding）的数量即使很多，也不会增加单个验证节点的负担，反而会刺激更多验证节点加入，从而保证验证节点的充分去中心化。

最终，Danksharding能够通过PBS实现以太坊的中心化出块，通过DAS实现去中心化验证，并且具备一定程度的抗审查性，从而确保以太坊成为可扩展的共识层和数据可用性层，并且能够承接住执行层的更多Rollups。（PS：中心化出块、去中心化验证也是Vitalik在Endgame中提出的对以太坊未来发展的构想。）
