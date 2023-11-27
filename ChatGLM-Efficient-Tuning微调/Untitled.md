# Baichuan2-13B-base模型推理

------

### Base 模型推理方法示范

```python
from transformers import AutoModelForCausalLM, AutoTokenizer
tokenizer = AutoTokenizer.from_pretrained("/home/user/hub/model/Baichuan2-13B-Base", trust_remote_code=True)
model = AutoModelForCausalLM.from_pretrained("/home/user/hub/model/Baichuan2-13B-Base", device_map="auto", trust_remote_code=True)
inputs = tokenizer('登鹳雀楼->王之涣\n夜雨寄北->', return_tensors='pt')
inputs = inputs.to('cuda:0')
pred = model.generate(**inputs, max_new_tokens=64, repetition_penalty=1.1)
print(tokenizer.decode(pred.cpu()[0], skip_special_tokens=True))
登鹳雀楼->王之涣
夜雨寄北->李商隐
```

1、编写简单字段数据集

2、编写构造数据集脚本

3、根据新数据集进行glm3的指令监督微调

4、模型效果训练验证