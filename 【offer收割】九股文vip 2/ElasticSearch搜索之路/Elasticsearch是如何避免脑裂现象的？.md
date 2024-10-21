# 👌Elasticsearch是如何避免脑裂现象的？

Elasticsearch 通过多种机制来避免脑裂（split-brain）现象。脑裂现象是指在分布式系统中，由于网络分区或其他原因导致集群被分成多个孤立的子集，每个子集都认为自己是独立的集群并选出自己的 Master 节点。这会导致数据不一致和系统不稳定。为避免这种情况，Elasticsearch 采用以下几种方法：

### 1.`minimum_master_nodes`设置
`minimum_master_nodes`是 Elasticsearch 中一个关键配置参数，用于确保在选举 Master 节点时，必须有至少一定数量的 Master-Eligible 节点参与投票。这个参数的设置可以有效防止脑裂现象。具体来说：

+ **公式**：`minimum_master_nodes = (N / 2) + 1`，其中 N 是集群中 Master-Eligible 节点的总数。
+ **示例**：如果集群中有 3 个 Master-Eligible 节点，则`minimum_master_nodes`应设置为`(3 / 2) + 1 = 2`。这意味着至少需要 2 个 Master-Eligible 节点参与投票才能选出一个 Master 节点。

通过设置`minimum_master_nodes`，可以确保在网络分区的情况下，只有包含多数节点的分区能够进行 Master 选举，防止多个子集群各自选出自己的 Master。

### 2. Zen Discovery 和 Quorum-Based 机制
Elasticsearch 使用 Zen Discovery 模块来管理节点发现和 Master 选举过程。Zen Discovery 采用了类似于 Quorum-Based 的共识机制，确保只有在获得多数节点（Quorum）同意的情况下，才能选出 Master 节点。

+ **Quorum**：Quorum 是指在分布式系统中，必须有多数节点同意某个操作才能执行。对于 Master 选举，Quorum 通常是集群中 Master-Eligible 节点数的一半加一。
+ **选举过程**：在 Master 选举过程中，只有获得 Quorum 支持的节点才能成为新的 Master 节点。这确保了在网络分区的情况下，只有包含多数节点的分区能够选出 Master，防止脑裂。

### 3. 节点故障检测和处理
Elasticsearch 具有完善的节点故障检测机制，能够及时发现和处理节点故障，防止因节点失联导致的脑裂现象。

+ **故障检测**：节点之间会定期发送心跳消息（Ping）来检测彼此的状态。如果一个节点在一定时间内没有收到其他节点的心跳消息，会认为该节点失联。
+ **故障处理**：当检测到节点故障时，集群会重新评估 Master 节点的状态。如果当前的 Master 节点失联，剩余的 Master-Eligible 节点会发起新的选举过程。

### 4. 集群状态的传播和确认
Elasticsearch 通过集群状态的传播和确认机制，确保所有节点对集群状态的一致性理解。

+ **状态传播**：Master 节点会定期将集群状态（如节点列表、分片分配等）广播给所有节点。
+ **状态确认**：每个节点在接收到新的集群状态后，会进行确认并返回确认消息给 Master 节点。只有在多数节点确认新的集群状态后，该状态才会生效。

### 5. 网络分区处理
Elasticsearch 具有处理网络分区的能力，确保在网络分区的情况下，集群能够保持一致性和稳定性。

+ **分区检测**：当网络分区发生时，集群中的节点会检测到其他节点失联。
+ **分区恢复**：当网络分区恢复后，集群会重新进行节点发现和状态同步，确保所有节点对集群状态的一致性理解。

### 6. 弹性伸缩和自动恢复
Elasticsearch 支持弹性伸缩和自动恢复机制，能够在节点加入或离开集群时自动进行调整，确保集群的稳定性和一致性。

+ **弹性伸缩**：当新的节点加入集群时，集群会自动重新分配分片，以平衡负载。
+ **自动恢复**：当节点失联或恢复时，集群会自动进行状态同步和数据恢复，确保数据的一致性和可用性。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/lp0sn3kt3pknx4ak>