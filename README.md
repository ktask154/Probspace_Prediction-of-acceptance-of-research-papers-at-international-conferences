# 

[link](https://comp.probspace.com/competitions/paper_acception)

## Task
論文のタイトル、アブストラクト、キーワードなどの自然言語情報から、その論文が国際学会で採択されるかどうかを予測

<br />

### 評価指標
Accuracy

<br />

## シングルモデル
| exp | model |
----- | -----
 63 | deberta-v3-base
 64 | bert-base-uncased
 65 | deberta-base
 66 | roberta-base
 68 | bart-base
 69 | electra-base-discriminator
 70 | funnel-transformer/medium
 71 | albert-base-v2
 72 | deberta-v3-small
 74 | distilbert-base-uncased
 75 | distilbart-cnn-12-6
 76 | muppet-roberta-base
 77 | gpt-neo-125m
 
 
 <br />
 
・input text : title + abstract <br />
・cv : StratifiedKFold(n=10) <br />
・Last Hidden State Output : Max Pooling <br />
・epoch : 5 <br />
・Loss : BCEWithLogitsLoss <br />
・optimizer : AdamW <br />
・LR : Layer-wise Learning Rate Decay (LLRD) <br />
・敵対学習方法(AWP) <br />
・MLM pretraining 10 epoch (probability=0.15) <br />

<br />

## アンサンブル
・exp078 Stacking(CatBoost)

  01予測したシングルモデルの予測結果+タイトル(アブスト)のワード数+タイトル(アブスト)のキーワード数+タイトル(アブスト)に含まれているキーワード数
  
  CV : 0.7384
  
  Public : 0.7314
  
  Private : 0.7332
  
  

<br />

・exp079 Voting

  13個のシングルモデルの1予測された個数をカウントし、閾値(=2)をを超えたものを1予測
  
  CV : 0.7394
  
  Public : 0.7387
  
  Private : 0.7332
  
 
 
<br />
<br />

## ベストシングルモデル
| exp |model | cv | public | private |
----- | ---- | -- | ------ | -------
012 | deberta-v3-base | 0.7079 | 0.7397 | 0.7368

・input text : abstract <br />
・cv : StratifiedKFold(n=5) <br />
・Last Hidden State Output : Mean Pooling <br />
・Loss : BCELoss

