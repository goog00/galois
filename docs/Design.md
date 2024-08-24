# 设计文档

## 常见

### 为什么矩阵乘法采用Padding的策略, 而不是拼接?

我们的编译器设计的时候, 最小单元的shape是根据物理单元来决定的, 如果使用padding, 我们后期可以调整模型的形状来适配它.
采用拼接策略的话, 会非常复杂, 并且有的硬件可能会不支持

### Cache建模

在apple m1芯片上, 目前对缓存的模型并不敏感, 我们后续有具体需求再进一步建模. 需要参考Blis库的一些策略.
i,k,j三个维度.  我们会把k维度(中间维度)当做时间维度, 该维度下, cache的需求会大量减少.

### 为什么不采用前端, 而是直接用C++函数调用IR来表示算子?

C++函数调用IR会拥有更强的表达能力. 并且可以很好的判定shape和type的合法性. 前端无法很好的判定这些东西

### 数据的输入也需要用一个明确的node

我们Tensor应该鼓励由编译器分发的不同机器上, 所以Input节点可能还需重新设计