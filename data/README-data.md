Each source-target language ($sl-$tl) directory has a `*.tsv` file (training and dev) with the following columns:

1) index: segment id
2) original: original sentence
3) translation: MT output
4) scores: list of DA scores by all annotators - the number of annotators may vary
5) mean: average of DA scores
6) z_scores: list of z-standardized DA scores
7) z_mean: average of z-standardized DA scores
8) model_scores: NMT model score for sentence

`*.doc_ids` files contain the name of the article where each original segment came from

`word-probas` directory contains the following files:
 
* `word_probas.*.$sl$tl`: log-probabilities from the NMT model for each decoded token including the <eos> token
* `mt.*.$sl$tl`: the actual output of the NMT model before any post-processing, corresponding to the log-probas
 above (the <eos> token is not printed, so the number of log-probabilities equals the number of tokens plus 1)

