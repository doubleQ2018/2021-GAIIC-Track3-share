# 2021-GAIIC-Track3-share

全球人工智能技术创新大赛【赛道三】

很高兴能获得这次的周周星，基本follow了之前周周星大佬们的idea，我这里稍微做个补充：

## 模型选择
1. ESIM：最初尝试的模型，疯狂调参后5折效果线上只能到0.88多，最后放弃
2. ELECTRA/BERT/NEZHA：先pre-train，再fine-tune

线上效果是ELECTRA > NEZHA > BERT，不过我的预训练都没有之前大佬的效果好，目前ELECTRA单折线上0.903，五折0.909，榜上的成绩是这3个模型融合结果

## 模型预训练
1. 均按照[苏神](https://github.com/bojone/oppo-text-match)思路，词频对齐后直接加载模型继续预训练
2. mask直接采用transformer官方动态随机的MLM
3. 训练参数也是官方的，基本训练200 epochs后拿来用
4. 参照SWA思路，把预训练最后几轮的model weights拿来取平均作为最后的model是有提升的，不过最后发现和直接把最后几轮的model预测的结果融合效果差不多

## 模型finetune
1. 直接使用CLS层embedding全连接到分类层
2. 用了CosineAnnealingWarmRestarts的权重衰减
3. fgm能稳定提升0.3%，我这的epsilon=0.1，提升不明显的可以多试试调调这个参数
