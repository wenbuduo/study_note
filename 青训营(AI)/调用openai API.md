## 理论部分

### 两个模式
- **Chat Model，聊天模型**，用于产生人类和AI之间的对话，代表模型当然是gpt-3.5-turbo（也就是ChatGPT）和GPT-4。当然，OpenAI还提供其它的版本，gpt-3.5-turbo-0613代表ChatGPT在2023年6月13号的一个快照，而gpt-3.5-turbo-16k则代表这个模型可以接收16K长度的Token，而不是通常的4K。（注意了，gpt-3.5-turbo-16k并未开放给我们使用，而且你传输的字节越多，花钱也越多）
- **Text Model，文本模型**，在ChatGPT出来之前，大家都使用这种模型的API来调用GPT-3，文本模型的代表作是text-davinci-003（基于GPT3）。而在这个模型家族中，也有专门训练出来做文本嵌入的text-embedding-ada-002，也有专门做相似度比较的模型，如text-similarity-curie-001。
- 上面这两种模型，提供的功能类似，都是接收对话输入（input，也叫prompt），返回回答文本（output，也叫response）。但是，它们的调用方式和要求的输入格式是有区别的。
### 文本模式
```
response = client.completions.create( 
model="gpt-3.5-turbo-instruct", 
temperature=0.5, 
max_tokens=100, 
prompt="请给我的花店起个名")
print(response.choices[0].text.strip())
```
参数说明
![[Pasted image 20241102194641.png]]
当你调用OpenAI的Completion.create方法时，它会返回一个响应对象，该对象包含了模型生成的输出和其他一些信息。这个响应对象是一个字典结构，包含了多个字段。
![[Pasted image 20241102194758.png]]
choices字段是一个列表，因为在某些情况下，你可以要求模型生成多个可能的输出。每个选择都是一个字典，其中包含以下字段：
- text：模型生成的文本。
- finish_reason：模型停止生成的原因，可能的值包括 stop（遇到了停止标记）、length（达到了最大长度）或 temperature（根据设定的温度参数决定停止）。
### Chat 模型
```
{  
    'id': 'chatcmpl-2nZI6v1cW9E3Jg4w2Xtoql0M3XHfH',  
    'object': 'chat.completion',  
    'created': 1677649420,  
    'model': 'gpt-4',  
    'usage': {'prompt_tokens': 56, 'completion_tokens': 31, 'total_tokens': 87},  
    'choices': [  
            {  
            'message': {  
             'role': 'assistant',  
            'content': '你的花店可以叫做"花香四溢"。'  
            },  
        'finish_reason': 'stop',  
        'index': 0  
        }  
    ]  
}
```

有两个专属于对话的概念：
**消息**，消息就是传入模型的提示。此处的messages参数是一个列表，包含了多个消息。每个消息都有一个role（可以是system、user或assistant）和content（消息的内容）。系统消息设定了对话的背景（你是一个很棒的智能助手），然后用户消息提出了具体请求（请给我的花店起个名）。模型的任务是基于这些消息来生成回复。 
**角色**，在OpenAI的Chat模型中，system、user和assistant都是消息的角色。每一种角色都有不同的含义和作用。
- system：系统消息主要用于设定对话的背景或上下文。这可以帮助模型理解它在对话中的角色和任务。例如，你可以通过系统消息来设定一个场景，让模型知道它是在扮演一个医生、律师或者一个知识丰富的AI助手。系统消息通常在对话开始时给出。
- user：用户消息是从用户或人类角色发出的。它们通常包含了用户想要模型回答或完成的请求。用户消息可以是一个问题、一段话，或者任何其他用户希望模型响应的内容。
- assistant：助手消息是模型的回复。例如，在你使用API发送多轮对话中新的对话请求时，可以通过助手消息提供先前对话的上下文。然而，请注意在对话的最后一条消息应始终为用户消息，因为模型总是要回应最后这条用户消息。
字段含义：
![[Pasted image 20241102195401.png]]
### 两种模式区别
chat模式设计的主要优点包括：
1. 对话历史的管理：通过使用Chat模型，你可以更方便地管理对话的历史，并在需要时向模型提供这些历史信息。例如，你可以将过去的用户输入和模型的回复都包含在消息列表中，这样模型在生成新的回复时就可以考虑到这些历史信息。
2. 角色模拟：通过system角色，你可以设定对话的背景，给模型提供额外的指导信息，从而更好地控制输出的结果。当然在Text模型中，你在提示中也可以为AI设定角色，作为输入的一部分。
然而，对于简单的单轮文本生成任务，使用Text模型可能会更简单、更直接。例如，如果你只需要模型根据一个简单的提示生成一段文本，那么Text模型可能更适合。从上面的结果看，Chat模型给我们输出的文本更完善，是一句完整的话，而Text模型输出的是几个名字。这是因为ChatGPT经过了对齐（基于人类反馈的强化学习），输出的答案更像是真实聊天场景。

## 实验部分
### 环境配置
只分享个人的学习过程，遇到问题时的解决办法有部分参考CSDN
首先在anaconda 当中建立虚拟环境（openai_demo），打开anaconda prompt 输入
```
conda create -n openai-demo python=3.8
conda active openai-demo #激活环境
pip install openai --upgrade #安装最新的openai库
pip install langchain
pip install langchain-openai
```
pycharm中建立新项目，并且配置成虚拟环境的解释器
![[Pasted image 20241102173105.png]]

## 申请API KEY
可以进入openai官网，找到申请keys的页面[API keys - OpenAI API](https://platform.openai.com/api-keys)create 一个新的就可以，这里注意生成后的密钥只会出现一次，记得点copy，之后可以ctrl+v，在剪切板中寻找。
 ![[Pasted image 20241102134519.png]]
申请完了之后，在python文件中配置环境后就可使用
```
import os
os.environ["OPENAI_API_KEY"] = '你的密钥'  
proxy_url = 'http://127.0.0.1'  
proxy_port = '代理端口'  
# Set the http_proxy and https_proxy environment variables  
os.environ['http_proxy'] = f'{proxy_url}:{proxy_port}'  
os.environ['https_proxy'] = f'{proxy_url}:{proxy_port}'
```
如果你能购买调用API的额度，不妨可以尝试直连，本人是通过国内中转，感觉更简单稳定。
接着就是调用API，进行问答：
```
client = OpenAI(  
    base_url='中转网址',  
    api_key='替换为自己的key'  
)  
completion = client.chat.completions.create(  
  model="gpt-3.5-turbo",  
  messages=[  
    {"role": "user", "content": "背一下咏鹅"},  
  ]  
)
print(completion.choices[0].message)  # 回答
```
对于图像处理需要调用GPT4接口，调用方法如上，代码可以看课程示例
