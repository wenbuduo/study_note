**论文原文：**[proceedings.mlr.press/v54/mcmahan17a/mcmahan17a.pdf](https://proceedings.mlr.press/v54/mcmahan17a/mcmahan17a.pdf)
本地训练模型，参数取加权平均传到中心服务器，服务器平均后广播回本地
### FedAvg的优势

与其他联邦学习算法相比，FedAvg有以下优点：

- 低通信开销：由于只需要上传本地模型参数，因此通信开销较低。  
    
- 支持异质性数据：由于本地设备可以使用不同的数据集，因此FedAvg可以处理异质性数据。  
    
- 泛化性强：FedAvg算法通过全局模型聚合，利用所有设备上的本地数据训练全局模型，从而提高了模型的精度和泛化性能。  
    

### FedAvg的缺点

尽管FedAvg具有许多优点，但它仍然存在一些缺点：

- 需要协调：由于需要协调多个本地设备的计算，因此FedAvg需要一个中心化的协调器来执行此任务。这可能会导致性能瓶颈或单点故障。  
    
- 数据不平衡问题：在FedAvg算法中，每个设备上传的模型参数的权重是根据设备上的本地数据量大小进行赋值的。这种方式可能会导致数据不平衡的问题，即数据量较小的设备对全局模型的贡献较小，从而影响模型的泛化性能。
# 伪代码
![[Pasted image 20240508222200.png]]
详解：
1. 服务器初始化全局模型参数 $w_0$；  
    
2. 所有本地设备随机选择一部分数据集，并在本地计算本地模型参数 $w_i$；  
    
3. 所有本地设备上传本地模型参数 $w_i$ 到服务器；  
    
4. 服务器计算所有本地模型参数的加权平均值 $\bar{w}$，并广播到所有本地设备；  
    
5. 所有本地设备采用 $\bar{w}$ 作为本地模型参数的初始值，重复步骤2~4，直到全局模型收敛。
### 代码实现 Code
```
def fedavg(self):
        # FedAvg with weight
        total_samples = sum(self.num_samples)
        base = [0] * len(self.weights[0])
        for i, client_weight in enumerate(self.weights):
            total_samples += self.num_samples[i]
            for j, v in enumerate(client_weight):
                base[j] += (self.num_samples[i] / total_samples * v.astype(np.float64))

        # Update the model
        return base
```
