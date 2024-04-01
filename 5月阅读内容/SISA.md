 Machine Unlearning 的工作流大致分为三个部分：学习，遗忘和验证
 ![[Pasted image 20240508220241.png]]
 学习是正常的训练已知的模型和算法，遗忘是数据点，特征，甚至具体任务，通过后门攻击等方式进行验证
 # SISA：sharded(分片)，isolated(独立)，sliced and aggregated training(分片和聚合训练)

# 详细过程
![[Pasted image 20240508220627.png]]
![[Pasted image 20240508221337.png]]
# 总结
SISA 的思想很简单但也很巧妙，能够通过数据的切分减少 retrain 训练的规模，同时因为分片操作能够不用在 retrain 过程中从头训练。
但是因为需要 shard & slice 减少单个模型的训练数据量，可能导致 sub-model 训练效果不充分，得到 weak-learner；同时因为需要存储每个 checkpoint 的状态用来 retrain，会带来比较大的负担，所以只能算是比较标准的 baseline 供后续研究。
