# HyperGCN: A New Method of Training Graph Convolutional Networks on Hypergraphs

## Q：为什么这么做

- 将标签分配给图/超图中最初未标记的顶点
- 显式拉普拉斯正则化假设每个边/超边中的顶点之间存在相似性，但图卷积网络 (GCN) 的隐式正则化避免了这种限制

## Q:做了什么

基于超边中顶点的相似性，它们可以有效地用于超边不编码相似性的组合优化。

### 1.**超图拉普拉斯算子**：

定义了一个针对超图的拉普拉斯算子，用于超节点信号处理。这个算子是一个非线性函数，计算每个超边中差异最大的节点对之间的差异。

### **2.HyperGCN方法**：

通过上述拉普拉斯算子构建简单图，然后在该图上运行图卷积网络（GCN）。这种方法通过考虑每个超边中最具代表性的一对节点来简化超图结构。

### 3.**超图拉普拉斯算子的改进**：

讨论了一种加入中介节点的改进超图拉普拉斯算子，它满足与原始算子相同的性质，但考虑了更多的节点信息。

加入中介节点的改进超图拉普拉斯算子是对传统超图拉普拉斯算子的一种改进。在传统算法中，每个超边通常由一对节点代表，但这种方法可能忽略了超边中其他节点的信息。改进的算法通过引入“中介节点”的概念来解决这个问题。在这种方法中，超边中除了最具代表性的一对节点外，其他节点作为中介节点也被考虑在内，这样可以更全面地捕捉超图的结构信息。这种改进使得超图拉普拉斯算子能够更准确地反映超图中各个节点间的复杂关系。

### 4.**FastHyperGCN方法**：

使用初始特征构建拉普拉斯矩阵，称为FastHyperGCN。这种方法减少了训练时间，因为拉普拉斯矩阵只在训练前计算一次。

## Q:应对了什么挑战

展示了 HyperGCN 在 SSL 和组合优化方面的有效性。赋予节点重要性的方法改善了 SSL 的结果。 HyperGCN 可以通过此类方法进行增强，以进一步提高性能。

## Q:现有的方法是什么

### 1.超图学习：

超图神经网络及其变体 [23, 24] 使用团扩展来扩展超图的 GCN。 Powerset 卷积网络[47]利用信号处理工具来定义集合函数上的卷积。另一项工作使用数学上有吸引力的张量方法 [40, 6]，但它们仅限于均匀超图。

### 2.超图谱理论：

1. **非线性拉普拉斯算子在超图中的应用：**
   - 研究人员已经充分利用了超图结构，特别是通过非线性拉普拉斯算子。在数学和图论中，拉普拉斯算子是一种用于描述图形结构的矩阵，它可以捕获图中节点的连接模式。
2. **Cheeger型不等式和超图扩展：**
   - 这些算子使得可以为超图建立类似Cheeger的不等式，将拉普拉斯算子的第二小特征值与超图的扩展性联系起来。在图论中，Cheeger不等式提供了图的切割大小和其拉普拉斯算子的第二小特征值之间的关系。类似地，在超图中，这种关系有助于理解超图结构的特性。
3. **基于总变化的拉普拉斯算子：**
   - 提到了一种从超图的总变化（total variation）概念（Lovasz超图割的扩展）衍生出的拉普拉斯算子。这种方法考虑了每个超边中最不相似的顶点。这对于理解和分析超图中复杂的连接模式非常有用。
4. **非线性算子的扩展应用：**
   - 这些算子已经被扩展到不同的设置中，包括：
     - **有向超图**：考虑在超边的“尾部”使用上确界（supremum），在“头部”使用下确界（infimum）。
     - **次模超图**：对不同的超边割使用不同的次模权重。
     - **次模函数最小化**：这个概念推广了超图SSL（自监督学习）目标。
     - 提及了一种考虑每个超边中所有顶点的拉普拉斯算子。

### 3.图的自监督

1. **基于图的自监督学习（Graph-based SSL）：**
   - 自监督学习（SSL）是一种利用未标记数据进行训练以提高学习准确性的方法。在基于图的SSL中，这种方法特别有效，因为图结构数据通常包含丰富的、未被标注的关系信息。
   - 研究表明，使用未标记数据可以显著提高学习的准确度。这种技术在许多领域都非常受欢迎，并且已经有一些有影响力的书籍讨论了这个话题。

### 4.图神经网络在组合优化问题中的应用

- 组合优化是数学和计算机科学中的一个重要领域，涉及在一组有限选项中找到最优解的问题。很多组合优化问题都是NP难（NP-hard）问题，这意味着找到精确解的难度非常大。
- 图神经网络（GNNs）是一种专门用于处理图结构数据的深度学习模型。最近的研究表明，GNN可以作为一种基于学习的方法，有效地解决一些NP难问题，例如：
  - 最大独立集问题（maximal independent set）
  - 最小顶点覆盖问题（minimum vertex cover）
  - 旅行商问题（traveling salesman problem）的决策版本
  - 图着色问题（graph colouring）
  - 团优化问题（clique optimisation）

## Q:作者的核心贡献是什么

- 我们方法的更快变体对于最稠密的 k 子超图和现实世界超图（DBLP 和 Pubmed）上的 SSL 的合成数据（密度也更高）训练时间较短。
- 应用于实际超图上的 SSL 和组合优化问题

## 挑战

获得的图近似的质量高度依赖于权重初始化。

# Tips:

- HGNN:state-of-the-art hypergraph neural networks
- 图深度学习这一主题的全面文献综述 [5] 和广泛调查 [20, 4]。
- [4]Peter W. Battaglia, Jessica Hamrick, Victor Bapst, Alvaro Sanchez-Gonzalez, Vinícius Flores Zambaldi, Mateusz Malinowski, Andrea Tacchetti, David Raposo, Adam Santoro, Ryan Faulkner, Çaglar Gülçehre, Francis Song, Andrew Ballard, Justin Gilmer, George Dahl, Ashish Vaswani, Kelsey Allen, Charles Nash, Victoria Langston, Chris Dyer, Nicolas Heess, Daan Wierstra, Pushmeet Kohli, Matthew Botvinick, Oriol Vinyals, Yujia Li, and Razvan Pascanu. Relational inductive biases, deep learning, and graph networks. arXiv:1806.01261, 2018. 2.
-  [5] Michael Bronstein, Joan Bruna, Yann LeCun, Arthur Szlam, and Pierre Vandergheynst. Geometric deep learning: Beyond euclidean data. IEEE Signal Process., 2017. 2 and 3.
- [20] William L. Hamilton, Rex Ying, and Jure Leskovec. Representation learning on graphs: Methods and applications. IEEE Data Eng. Bull., 40(3):52–74, 2017.
