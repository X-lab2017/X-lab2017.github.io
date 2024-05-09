<meta name="referrer" content="no-referrer"/>
# Using knowledge units of programming languages to_recommend reviewers for pull requests_ an empirical study

## 摘要
为给定的代码更改（本文指pr）寻找合适的代码审查者。使用的基本工具叫 **KU**（knowledge unit）。设计了一个推荐系统 **KUREC** 。其中，文章根据检测到的 KU 生成开发人员的的专业知识档案。同时，文章将 KUREC 与其他七个基准推荐器做了比较。效果与第一梯队的 RF 并列第一且 KUREC 的推荐更加稳定，因为其四分位距更小，所以可能更加值得信赖。
文章还设计了组合推荐器，得出结论是组合推荐器优于包括 KUREC 在内的所有单一推荐器。目前比较好的组合方式是与 AD_HYBRID 相结合的推荐器。文章提出期望：

- 未来可以将 KU-based 推荐器作为基准。
- 未来可以研究与 KU 相结合的组合推荐器。
##  结论

基本是对摘要的复述和一些对未来研究的展望

## KU

KU 定义：文章将 KU 定义为特定编程语言的一个或多个构件所提供的一整套关键能力。
KU 数据来源： 10个 github 上积极维护的 java 项目， 包括 29000 条 commits 和 65000 条pr。
KU 定义的基本原理中要避免KU与上下文的过度关联
在具体的 KU 定义过程中，使用 Oracle 的 java 考试作为 KU 但是会排除一些子主题，例如解释概念的主题和不能被自动提取的主题等等。
**KU 包括一系列，有如下，即一些列特定类型功能的代码块**
![909e3fabef2c17a74452b4011c5dd17c_5_Table_1_614574616.png](https://cdn.nlark.com/yuque/0/2024/png/39023202/1712030565364-46e72608-7fe6-4a31-b833-81f2d1aa19cd.png#averageHue=%23ededed&clientId=u0bf7242b-61f3-4&from=drop&height=1566&id=u929e89ef&originHeight=1566&originWidth=1041&originalType=binary&ratio=2&rotation=0&showTitle=false&size=260762&status=done&style=none&taskId=uf25209b9-245d-4746-8ad0-ad117c168d5&title=&width=1041)

## 如何发现KU

如何分析的 Java 源代码是通过 Eclipse 的 JDT 框架进行分析，它会给出源码的抽象语法树 （AST）并且提供这个树的每个元素，包括名称、类型和表达式等等。
通过这个框架以及事先定义的 KU ，文章为每个文件的 KU 进行计数，用一个向量存储，包括每个 KU 的计数。

## 数据收集过程

收集目的项目的 commit 和 pr 。首先项目是积极维护的，标签要求 java 。 star 不少于 100. 过去一年有提交，之后进行降序排序。这个步骤需要下载目标项目的提交历史，中间使用 PyDriller 分析这个历史数据，从而获得诸如提交信息、id、作者姓名和日期等信息。
这里【3】中说的用 KU 表示开发人员专业知识我的理解是，针对某个开发者提交的commit，分析其改动部分的 KU ，通过这改动部分的 KU 对开发人员进行表示。具体如何通过 KU 表示开发者在下一节
![909e3fabef2c17a74452b4011c5dd17c_9_Figure_2_-1337592130.png](https://cdn.nlark.com/yuque/0/2024/png/39023202/1712032549578-466933d3-6ed6-427a-9a84-ee03f8f98835.png#averageHue=%23e6e6e6&clientId=u0bf7242b-61f3-4&from=drop&height=1026&id=u75b8e219&originHeight=1026&originWidth=1032&originalType=binary&ratio=2&rotation=0&showTitle=false&size=111365&status=done&style=none&taskId=ua0931c25-b543-4fd7-941b-3617fe942c6&title=&width=1032)
![image.png](https://cdn.nlark.com/yuque/0/2024/png/39023202/1712035303556-2e52c112-4090-4c9e-adea-85ca75d51627.png#averageHue=%23e9e8e8&clientId=u0bf7242b-61f3-4&from=paste&height=301&id=u31a07447&originHeight=602&originWidth=2026&originalType=binary&ratio=2&rotation=0&showTitle=false&size=113786&status=done&style=none&taskId=u87808efa-59d5-4444-bd3e-ea9e072dabb&title=&width=1013)

## 如何通过 KU 定义开发者专业档案（对开发者进行表示）

1. 通过每个 commit 中改动的部分挖掘 KU ，并于开发者联系起来。
2. 对开发者进行表示的时候不是针对部分 commit ，而是整个 java 文件的 commit 与 pr
3. 通过 KU 表示开发者的方法是用一个 28 位的 KU 向量表示。每个位置对应一个 KU 。为进行标准化。每个 KU 对应一个比例值，每个比例值的计算方式为该开发者该类型的 KU 总数 除以 所有开发者的该类型的所有 KU 数。得到一个 28 维向量，每个值是一个比例。 这个向量叫Pku (D)

![image.png](https://cdn.nlark.com/yuque/0/2024/png/39023202/1712035629750-31f06f9d-54b2-462e-a071-975aec5dffde.png#averageHue=%23eeeeed&clientId=u0bf7242b-61f3-4&from=paste&height=448&id=u4643641f&originHeight=896&originWidth=1972&originalType=binary&ratio=2&rotation=0&showTitle=false&size=179699&status=done&style=none&taskId=u40f25754-91aa-4360-891f-963b36c4df5&title=&width=986)

## 如何证明基于 KU 分组的开发者档案对代码审查者推荐有帮助

1. 先 PCA 降维，运用K-means聚类算法进行聚类
2. 因为 k-means 的聚类集群是人工设定的参数，所以文章从 2-100 进行试验，并用轮廓系数的中位数进行比较。（为什么用中位数而不是传统的平均数是因为中位数对异常值的表现更加稳健，例如只有少数糟糕聚类对象的聚类应该被认为是一个整体良好的聚类）在产生的中位数轮廓为0.90中选择最大的 K 但不最大化 K 。
3. 结果有：基于 KU 的分类可以将开发者分为 71 个不同的类型，表面该方法具有良好的分类能力。这里区分不同类型的聚类还用到了**基尼系数**作为区分的手段。其中，这样的聚类方法中有57%的比例是单例集群，即集群中只有一个人。表面开发人员具有独特的 KU 配置。
4. 在文章示例中的观察结果是基尼系数为0.96 即存在一个包含大多数人的集体。
5. 文章通过条形图表面不同聚类的开发者在与不同 KU 的关联程度，从图表结果可以看到不同聚类关联的 KU 显著不同。
6. 文章的工作分析了具有良好经验的开发者，其定义为在某方面 KU 高于整个数据集的中值 KU（数值上的体现是概率值的比较）。

## KUREC的原理

### 评估方法中的关键假设

对于任何一个给定的Pull Request（PR），其代码审查者最有可能是那些在该PR更改文件中涉及的知识单元（KUs）方面的专家。换言之，如果一个审查者对PR中修改的代码所使用的特定编程语言特性或功能有深入了解和实践经验，那么他/她就被视为最适合对此PR进行审查的人选。

### 构建方式

基于历史的 pr 数据，训练测试 82开。这里提到了一些之前的研究也是采用基于历史的数据。也许以后可以用。

### 推荐方式

基于开发者的专业配置文件和审查配置文件给出分数，根据分数降序排序进行推荐，推荐完成后更新配置文件。（如何更新？）专业配置文件是基于 commit。审查配置文件基于 pr。
专业分数的计算同时考虑开发者最近是否有参与 这个 commit 或 pr 相关的 KU 的工作，也就是新鲜度。
**推荐器准确度的计算方式在论文23页**

## 相关工作可以以后阅读的内容

1. 评估推荐准确度的研究
2. 推荐准确性和审查工作负载分布之间的平衡
3. **开发人员的专业表示**
4. **KU**

## 潜在的问题

1. 没有针对第三方库建立 KU 而是基于原项目
2. 使用 java 考试中的题目帮助建立的 KU 可能涵盖的知识是有限的
3. 实验得出的开发人员分类最佳数量不一定好
4. 仅针对java项目进行了研究
5. 使用 KU 的结果不一定是泛化（通用）的，可以尝试结合其他指标在不同的主题下进行研究，比如不同的编程语言。

## 本文与开发者技能定义的关联

这应该是我阅读这些文献的目的和意义。

1. 对开发者进行数据化的表示，也就是专业配置文件和审查配置文件，开发者在28个 KU 上的一个 28维向量
2. 如何利用这些配置文件进行专业分数计算，具体的计算方式在论文p20
