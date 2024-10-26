提示样本数量区别：
Few-Shot CoT：一些样本
One-Shot CoT：单样本
Zero-Shot CoT:没有提示样本
提示工程：简单来说就是类似角色设定——模型回答之前，先告诉它“你是一个很有经验的XX专家”，模型应该就会在开始胡说八道之前三思。

Few-Shot CoT，指的就是在带有示例的提示过程中，加入思考的步骤，从而引导模型给出更好的结果。
One-Shot CoT，只带有一个提示示例，加入了思考步骤
Zero-Shot CoT，就是直接告诉模型要一步一步地思考，慢慢地推理。

## 运用few-Shot CoT
**创建 FewShotPromptTemplate 对象**
```
# 3. 创建一个FewShotPromptTemplate对象
from langchain.prompts.few_shot import FewShotPromptTemplate
prompt = FewShotPromptTemplate(
    examples=samples,
    example_prompt=prompt_sample,
    suffix="鲜花类型: {flower_type}\n场合: {occasion}",
    input_variables=["flower_type", "occasion"]
)
print(prompt.format(flower_type="野玫瑰", occasion="爱情"))
```
在这个步骤中，它首先创建了一个SemanticSimilarityExampleSelector对象，这个对象可以根据语义相似性选择最相关的示例。然后，它创建了一个新的FewShotPromptTemplate对象，这个对象使用了上一步创建的选择器来选择最相关的示例生成提示。
然后，我们又用这个模板生成了一个新的提示，因为我们的提示中需要创建的是红玫瑰的文案，所以，示例选择器example_selector会根据语义的相似度（余弦相似度）找到最相似的示例，也就是“玫瑰”，并用这个示例构建了FewShot模板。这样，我们就避免了把过多的无关模板传递给大模型，以节省Token的用量

## 思维树
- CoT的核心思想是通过生成一系列中间推理步骤来增强模型的推理能力。在Few-Shot CoT和Zero-Shot CoT两种应用方法中，前者通过提供链式思考示例传递给模型，后者则直接告诉模型进行要按部就班的推理。
- ToT进一步扩展了CoT的思想，通过搜索由连贯的语言序列组成的思维树来解决复杂问题。我通过一个鲜花选择的实例，展示了如何在实际应用中使用ToT框架。
示例
假设一个顾客在鲜花网站上询问：“我想为我的妻子购买一束鲜花，但我不确定应该选择哪种鲜花。她喜欢淡雅的颜色和花香。”  
AI（使用ToT框架）：
	**思维步骤1**：理解顾客的需求。
	**思维步骤2**：考虑可能的鲜花选择。
	**思维步骤3**：根据顾客的需求筛选最佳选择。
	**思维步骤4**：给出建议。
