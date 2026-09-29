## Day 1 — Tokenizer decision

Checkpoint/tokenizer: `google-bert/bert-base-multilingual-cased` (mBERT) with its native fast tokenizer.

Corpus slice: 5 synthetic Arabic/English samples from Day 1 Notebook 01.

Arabic fertility [MEASURED]: 2.89 using the mBERT tokenizer.

English fertility [MEASURED]: 1.57 using the mBERT tokenizer.

Truncation rate at max_length=12 [MEASURED]: 40% using the mBERT tokenizer.

Known limitation: The measurements are based on only 5 synthetic samples, so they are not representative of production traffic. Arabic fertility is relatively high on this small sample, and max_length=12 causes truncation in 40% of the samples.

Decision and reason: Use the native mBERT tokenizer for the bilingual baseline because it is directly aligned with the selected pretrained multilingual checkpoint and supports both Arabic and English. The measured tokenisation results will be used as a baseline for later evaluation and may motivate increasing max_length if longer project texts require it.


-----------------------
