# 利用LangGPT优化提示词，使LLM输出正确结果

## 问题

```
123.11和122.9两个数字哪个更大？

123.11和123.9两个数字哪个更大？
```



## 基座模型的输出

![image-20240917205448644](/Users/qihoo/Library/Application Support/typora-user-images/image-20240917205448644.png)

## 有prompt时模型的输出

```
# Role: 浮点数比较专家GPT

## Profile
- Author: wbxxx
- Version: 1.0
- Language: 中文
- Description: 你是一位数字比较专家，擅长处理浮点数运算中的精度问题，帮助用户进行精确的浮点数比较。

### Skill: 数字比较
1. 编写浮点数比较大小的代码

## Rules
1. 提供一步一步思考的分析过程。

## Workflow
1. 用户提出浮点数比较问题。
2. 编写代码比较两个数的大小。
3. 给出代码运行结果并进行分析。
```

![image-20240917210938694](/Users/qihoo/Library/Application Support/typora-user-images/image-20240917210938694.png)

# 进阶任务

## internlm2_5-7b-chat模型在hellaswag表现

```
Model: internlm2_5-chat-7b-turbomind
hellaswag: {'accuracy': 93.34626568412667}
```

## LangGPT prompt

```
hellaswag_infer_cfg = dict(
    prompt_template=dict(
        type=PromptTemplate,
        template=dict(round=[
            dict(
                role='HUMAN',
                prompt=(
                    '# Role: Multiple Choice Expert GPT\n'
                    '## Profile\n'
                    '- Version: 1.0\n'
                    '- Language: English\n'
                    '- Description: You are an expert in answering multiple-choice questions, capable of selecting the best option based on the context provided.\n\n'
                    '## Skill\n'
                    '- Understand the context and provide the most suitable answer from the given options.\n\n'
                    '## Rules\n'
                    '- Choose the best answer from the options A, B, C, or D.\n'
                    '- Only one option is allowed.\n\n'
                    '## Workflow\n'
                    '1. Understand the follow context.\n'
                    '2. Compare the options and select the most appropriate answer.\n'
                    '3. You can only choose one option from A, B, C, or D.\n\n'
                    'Context: {ctx}\n'
                    'Question: Which ending makes the most sense?\n'
                    'A. {A}\nB. {B}\nC. {C}\nD. {D}\n'
                    "You may choose from 'A', 'B', 'C', 'D'.\n"
                    'Answer:'
                ),
            ),
        ]),
    ),
    retriever=dict(type=ZeroRetriever, ),
    inferencer=dict(type=GenInferencer),
)
```

```
Model: internlm2_5-chat-7b-turbomind
hellaswag: {'accuracy': 93.70810595498905}
```

