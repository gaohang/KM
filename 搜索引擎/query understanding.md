

## Query Correction

https://queryunderstanding.com/spelling-correction-471f71b19880

- **Overview**

  ### 错误检测->候选召回->纠错排序

- Offline, before any queries take place:

  - **Indexing Tokens.** Building the index used at query time for candidate generation.
  - **Building a language model.** Computing the model to estimate the a priori probability of an intended query.
  - **Building an error model.** Computing the model to estimate the probability of a particular misspelling, given an intended query.

  At query time:

  - **Candidate generation.** Identifying the spelling correction candidates for the query.
  - **Scoring.** Computing the score or probability for each candidate.
  - **Presenting suggestions.** Determining whether and how to present a spelling correction.





## imusic query correction

### log

日志是offline工作的前提，所需信息（kw，isManaul, isQC, exp/clk, #search_result）

### offline kvPair2redis

- 关联KV：low ctr（log：Nd搜索无结果；Nd搜索次数；Nd点击率）  ->  high ctr（log & meta）

  若keyword存在联想建议(通过prefix match查询es suggest index)，则不进行如下关联

  注意 redis kv 过期时间，例如，若log取last90d，redis有效期90d，则keyword 在90天后失效

- 关联方法：

1. 中文编辑距离

2. 中文转移概率（按照易错汉字Map文件）

3. 英文编辑距离

4. 拼音编辑距离

   

### online service

上游 keyword  preprocessing 

**-> operation management 运营后台-> white list白名单（hot meta info） -> redis（offline） -> online service** ->

下游 Rewrite/NER



