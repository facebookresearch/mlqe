Copyright (c) Facebook, Inc. and its affiliates.

All rights reserved.


This source code is licensed under the license found in the LICENSE file in the root directory of this source tree.

## Requirements:

* fairseq: https://github.com/pytorch/fairseq
* mosesdecoder: https://github.com/moses-smt/mosesdecoder
* subword-nmt: https://github.com/rsennrich/subword-nmt
* flores: https://github.com/facebookresearch/flores

fairseq `v0.8.0` was used to train the models and generate translations

## Set up:

Given a testset consisting of original sentences and translations:

* `SRC_LANG`: source language
* `TGT_LANG`: target language
* `INPUT`: input prefix, such that the file `$INPUT.$SRC_LANG` contains source sentences and `$INPUT.$TGT_LANG`
contains the target sentences
* `OUTPUT`: output path to store generated MT
* `MOSES_DECODER`: path to mosesdecoder installation
* `BPE_ROOT`: path to subword-nmt installation
* `BPE`: path to BPE model
* `MODEL_DIR`: directory containing the NMT model `.pt` file as well as the source and target vocabularies.
* `TMP`: directory for intermediate temporary files
* `GPU`: if translating with GPU, id of the GPU to use for inference

We do not actually care about the target sentences, but they are required by `fairseq-preprocess` script, so if
reference translations are not available, just create a dummy reference file e.g. copying the source sentences. 

## Translation

For **Romanian-English**, **Estonian-English**, **English-German** and **English-Chinese**:

Preprocess the input data:
```
for LANG in $SRC_LANG $TGT_LANG; do
  perl $MOSES_DECODER/scripts/tokenizer/tokenizer.perl -threads 80 -a -l $LANG < $INPUT.$LANG > $TMP/preprocessed.tok.$LANG
  python $BPE_ROOT/apply_bpe.py -c ${BPE} < $TMP/preprocessed.tok.$LANG > $TMP/preprocessed.tok.bpe.$LANG
done
```

Binarize the data for faster translation:

```
fairseq-preprocess --srcdict $MODEL_DIR/dict.$SRC_LANG.txt --tgtdict $MODEL_DIR/dict.$TGT_LANG.txt --source-lang ${SRC_LANG} --target-lang ${TGT_LANG} --testpref $TMP/preprocessed.tok.bpe --destdir $TMP/bin --workers 4
```

Translate

```
CUDA_VISIBLE_DEVICES=$GPU fairseq-generate $TMP/bin --path ${MODEL_DIR}/${SRC_LANG}-${TGT_LANG}.pt --beam 5 --source-lang $SRC_LANG --target-lang $TGT_LANG --no-progress-bar --unkpen 5 > $TMP/fairseq.out
grep ^H $TMP/fairseq.out | cut -d- -f2- | sort -n | cut -f3- > $TMP/mt.out
```

Post-process

```sed -r 's/(@@ )| (@@ ?$)//g' < $TMP/mt.out | perl $MOSES_DECODER/scripts/tokenizer/detokenizer.perl -l $TGT_LANG > $OUTPUT```

For **Sinhala-English** and **Nepalese-English** the pipeline is slightly different:

Preprocess:

```
bash $FLORES/scripts/indic_norm_tok.sh $SRC_LANG $INPUT.$SRC_LANG > $TMP/preprocessed.tok.$SRC_LANG
perl $MOSES_DECODER/tokenizer/tokenizer.perl -threads 80 -a -l $TGT_LANG < $INPUT.$TGT_LANG > $TMP/preprocessed.tok.$TGT_LANG
python $FLORES/scripts/spm_encode.py --model $BPE --output_format piece --inputs $TMP/preprocessed.tok.$SRC_LANG $TMP/preprocessed.tok.$TGT_LANG --outputs $TMP/preprocessed.tok.bpe.$SRC_LANG $TMP/preprocessed.tok.bpe.$TGT_LANG
```

Binarize

```
fairseq-preprocess --srcdict ${MODEL_DIR}/dict.${SRC_LANG}.txt --source-lang ${SRC_LANG} --target-lang ${TGT_LANG} --testpref ${TMP}/preprocessed.tok.bpe --destdir $TMP/bin --workers 4 --joined-dictionary
```

Translate

```
CUDA_VISIBLE_DEVICES=${GPU} fairseq-generate ${TMP}/bin --path ${MODEL_DIR}/${SRC_LANG}-${TGT_LANG}.pt --beam 5 --source-lang ${SRC_LANG} --target-lang ${TGT_LANG} --no-progress-bar --remove-bpe=sentencepiece --lenpen 1.2 > ${TMP}/fairseq.out
grep ^H ${TMP}/fairseq.out | cut -d- -f2- | sort -n | cut -f3- > ${OUTPUT}
```

For **Russian-English**:
Follow the instructions on fairseq webpage: https://github.com/pytorch/fairseq/tree/master/examples/wmt19

