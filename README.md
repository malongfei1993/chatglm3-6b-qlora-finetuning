# 基于清华的chatglm3-6B+QLoRA进行finetune.
---
背景
---
* RFQ文件大多为excel。
* RFQ文件中包含很多需要提取的关键参数，而关键参数由于RFQ的客户区别大，RFQ内容及格式有很大区别，希望提取RFQ关键参数进行标准化，从而可以进行后续分析等重要工作。
* 验证规则的方式提取关键参数，第一文档种类多，规则不一，工作量大，第二规则需要配置和操作，无法全自动提取关键参数。
* 提取的准确率可以90%左右，因为目标是整理标准化数据进行后续分析，可以允许一定的错误。
---
目标
---
* 分析文本数据，提取关键内容，代替人工，提高工作效率。
* 使用垂直领域数据进行微调，让chatglm3做数据提取任务同时输出为符合json格式要求。
---
数据集
---
有colab可以直接打开
---
准备
16G显存
python3.8
注意：transformer==4.34.0（唯一需要注意的版本）
额外：本人使用的是colab，9美元pro足够
---
数据预处理
---
* extract_json_from_excel.ipynb 准备数据csv文件，包含content列和summary列，content为输入，summary为输出。
---
训练
---
* qlora_chatglm3.ipynb
finetuing前后对比
---
推理
---
* chatglm3-peft-inference.ipynb，可以看到，微调后的输出参数提取的更加准确，比如水平分辨率也能正确提取，同时格式固定，方便进行后续提取分析。
* 微调前后结果对比(截取部分内容)：
```python
原始输出：
[{
"参数": "Screen size min. (diagonal)",
"需求规格": "12.1",
"供应商反馈": "OK"
}]


ChatGLM3-6B微调后：
{
"参数": "diagonal size",
"需求规格": "12.1",
"供应商反馈": "OK"
}

原始输出：
[{
"参数":"horizontal resolution",
"需求规格":"1280 x 800",
"供应商反馈": "OK"
}]


ChatGLM3-6B微调后：
{
"参数": "horizontal resolution",
"需求规格": "1280",
"供应商反馈": "OK"
}


原始输出：
[{
"参数": "frame rate",
"需求规格": "It should be 50/60 Hz",
"供应商反馈": "It should be 60HZ"
}]


ChatGLM3-6B微调后：
{
"参数": "frame rate",
"需求规格": "50/60 Hz",
"供应商反馈": "60HZ"
}
```


