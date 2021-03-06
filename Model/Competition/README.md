## Introduction

基于 Pytorch 的Transformer中文关系抽取处理模型。


## 环境依赖:

- python >= 3.6

- torch >= 1.2
- hydra-core >= 0.11
- tensorboard >= 2.0
- matplotlib >= 3.1
- transformers >= 2.0
- jieba >= 0.39

## 主要目录

```
├── conf                      # 配置文件夹
│  ├── config.yaml            # 配置文件主入口
│  ├── preprocess.yaml        # 数据预处理配置
│  ├── train.yaml             # 训练过程参数配置
│  ├── hydra                  # log 日志输出目录配置
│  ├── embedding.yaml         # embeding 层配置
│  ├── model                  # 模型配置文件夹
│  │  ├── transformer.yaml    # transformer 模型参数配置
├── module                    # 可复用模块
│  ├── Embedding.py           # embedding 层
│  ├── Attention.py           # attention
│  ├── Transformer.py         # transformer
├── models                    # 模型目录
│  ├── BasicModule.py         # 模型基本配置
│  ├── Transformer.py         # Transformer 模型
├── utils                     # 常用工具函数目录
├── metrics.py                # 评测指标文件
├── serializer.py             # 预处理数据过程序列化字符串文件
├── preprocess.py             # 训练前预处理数据文件
├── vocab.py                  # token 词表构建函数文件
├── dataset.py                # 训练过程中批处理数据文件
├── trainer.py                # 训练验证迭代函数文件
├── main.py                   # 主入口文件（训练）
├── predict_test.py           # 测试入口文件（测试）            
├── README.md                 # read me 文件
```

## 数据说明

train.csv 文件，样式范例为：

| sentence                                                     | relation | head     | head_offset | tail                         | tail_offset |
| ------------------------------------------------------------ | -------- | -------- | ----------- | ---------------------------- | ----------- |
| 和讯股票消息信息安全概念午后走强，截至13：05，立思辰涨停，航天信息涨超9%，北信源涨超6%，蓝盾股份、网宿科技、绿盟科技、美亚柏科涨超5% | 竞争     | 航天信息 | 31          | 北信源                       | 40          |
| 新浪财经App：直播上线博主一对一指导A股变不变盘就看今天紧盯一指标新浪财经讯2月9日消息，周四晚间，多家上市公司晚间发布公告，以下为利好消息汇总：龙泉股份：签订5913万元合同龙泉股份近日与恒力石化（大连）炼化有限公司签订了《恒力炼化海水循环水预应力钢筒混凝土管（PCCP）制作安装工程施工合同》，合同总金额暂定人民币59，131，314.00元 | 合作     | 龙泉股份 | 74          | 恒力石化（大连）炼化有限公司 | 96          |
| 原标题：机会情报：工信部发文提升数据安全保护能力指导企业加大投入来源：中金网据汇信了解，工信部日前印发《电信和互联网行业提升网络数据安全保护能力专项行动方案》，提出通过集中开展数据安全合规性评估、专项治理和监督检查，督促基础电信企业和重点互联网企业强化网络数据安全全流程管理，及时整改消除重大数据泄露、滥用等安全隐患 | 监管     | 工信部   | 9           | 汇信                         | 39          |

最终预测结果：

| Subject    | Predicate | Object     |
| ---------- | --------- | ---------- |
| 奇安信集团 | 竞争      | 深信服科技 |
| 永和阳光   | 合作      | 西陇科学   |
| 深华发Ａ   | 子公司    | 视觉中国   |



安装依赖： pip install -r requirements.txt

开始训练：python main.py

每次训练的日志保存在 `logs` 文件夹内，模型结果保存在 `checkpoints` 文件夹内。



## Future Work

* 增加标注，对已有模型进一步微调，提高分类的准确率
* 探索用远程监督数据扩充当前数据集的可能性