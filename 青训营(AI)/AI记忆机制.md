![[Pasted image 20241122161445.png]]
ConversationChain最主要的特点是，它提供了包含AI 前缀和人类前缀的对话摘要格式，这个对话格式和记忆机制结合得非常紧密。
减少幻觉的方法，补充输入下面的句子：
**“如果** **AI** **不知道问题的答案，它就会如实说它不知道。”**（If the AI does not know the answer to a question, it truthfully says it does not know.）
- **{history}** 是存储会话记忆的地方，也就是人类和人工智能之间对话历史的信息。
- **{input}** 是新输入的地方，你可以把它看成是和ChatGPT对话时，文本框中的输入。
记忆历史记录的方法：
-  ConversationBufferMemory：直接记录，会浪费很多Token
- ConversationBufferWindowMemory(缓冲窗口记忆):设置 k=1，这意味着窗口只会记住与AI之间的最新的互动，即只保留上一次的人类回应和AI的回应。
- ConversationSummaryMemory:对话总结记忆,将对话历史进行汇总，然后再传递给 {history} 参数，**不仅仅利用了LLM来回答每轮问题，还利用LLM来对之前的对话进行总结性的陈述，以节约Token数量**。
- ConversationSummaryBufferMemory：对话总结缓冲记忆，它是一种混合记忆模型。
### 四种记忆总结如下：
![[Pasted image 20241124233745.png]]
在你的客服聊天机器人设计中，你会首先告知客户：“亲，我的记忆能力有限，只能记住和你的最近10次对话哦。如果我忘了之前的对话，请你体谅我。” 当有了这样的预设，你会为你的ChatBot选择那种记忆机制？
1. ConversationBufferWindowMemory
这种记忆机制通过指定一个窗口（例如最近的10次对话）来存储和管理对话历史。这里的`k`值代表要保留的对话轮次。将`k`值调整为更高的数值（如15或20），可以让模型在更长的对话历史中进行上下文推理，从而更好地理解客户的需求和提供更连贯的回复。然而，过高的`k`值可能会导致模型的记忆和处理效率下降，特别是在大型对话中。建议在实际应用中根据系统的资源和响应时间进行调整。
2. ConversationSummaryBufferMemory
这种记忆机制通过对对话内容进行总结来节省内存。通过调整`max_token_limit`值，可以控制生成的摘要长度。将其设置为较低的值（如50 Token）会生成更简洁的摘要，可能会丢失一些细节，但适合快速的响应需求。而将其设置为更高的值（如200 Token）则允许更加详细的摘要，有助于保留重要信息，便于模型在后续对话中更好地理解上下文。