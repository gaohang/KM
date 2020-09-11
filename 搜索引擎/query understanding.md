

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