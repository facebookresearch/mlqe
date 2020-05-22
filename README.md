# MultiLingual Quality Estimation (MLQE) Dataset

This repository contains data for the 2020 Quality Estimation Shared Task:  
http://www.statmt.org/wmt20/quality-estimation-task.html  

## Training and development data

Check the 'data' folder 

## NMT models

Check the 'nmt-models' folder 

## Parallel data used to train the NMT data

Check 'http://www.statmt.org/wmt20/quality-estimation-task.html' 

## Additional parallel data

### German-English

[Europarl v9](http://www.statmt.org/europarl/v9/training/europarl-v9.de-en.tsv.gz)  
[ParaCrawl v3](https://s3.amazonaws.com/web-language-models/paracrawl/release3/en-de.bicleaner07.tmx.gz)  
[Common Crawl corpus](http://www.statmt.org/wmt13/training-parallel-commoncrawl.tgz)  
[News Commentary v14](http://data.statmt.org/news-commentary/v14/training/news-commentary-v14.de-en.tsv.gz)  
[Wiki Titles v1](http://data.statmt.org/wikititles/v1/wikititles-v1.de-en.tsv.gz)  
[Document-split Rapid corpus](https://s3-eu-west-1.amazonaws.com/tilde-model/rapid2019.de-en.zip)

### Chinese-English

[News Commentary v14](http://data.statmt.org/news-commentary/v14/training/news-commentary-v14.en-zh.tsv.gz)  
[Wiki Titles v1](http://data.statmt.org/wikititles/v1/wikititles-v1.zh-en.tsv.gz)  
[UN Parallel Corpus V1.0](https://stuncorpusprod.blob.core.windows.net/corpusfiles/UNv1.0-TEI.zh.tar.gz.00)  
[CWMT Corpus (casia2015, datum2015, datum2017, NEU)](http://mteval.cipsc.org.cn:81/agreement/wmt)

### Romanian-English

[SETIMES2](http://opus.nlpl.eu/SETIMES2.php)  
[Europarl v8](http://data.statmt.org/wmt16/translation-task/training-parallel-ep-v8.tgz)

### Estonian-English

[Europarl v8](http://data.statmt.org/wmt18/translation-task/training-parallel-ep-v8.tgz)  
[Rapid corpus of EU press releases](http://data.statmt.org/wmt18/translation-task/rapid2016.tgz)

### Sinhala-English

[Flores Iterative Back Translation](https://github.com/facebookresearch/flores#train-iterative-back-translation-models)

### Nepali-English

[Flores Iterative Back Translation](https://github.com/facebookresearch/flores#train-iterative-back-translation-models)

## Citation

If you use this data in your work, please cite:

```bibtex
@inproceedings{
  title={Unsupervised Quality Estimation for Neural Machine Translation},
  author={Marina Fomicheva, Shuo Sun, Lisa Yankovskaya, Fr\'{e}d\'{e}ric Blain, Francisco Guzm\'{a}n, Mark Fishel, Nikolaos Aletras, Vishrav Chaudhary, Lucia Specia},
  journal={arXiv preprint arXiv:2005.10608},
  year={2020}
}
```

## Changelog
- 2020-03-15: Adding details about training data for NMT models
- 2020-03-19: Releasing dataset


## License
The dataset is licensed under CC-BY-SA, see the LICENSE file for details.
