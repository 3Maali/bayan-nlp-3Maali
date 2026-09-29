# bayan-nlp-3Maali

## Day 1 — Text Processing & Tokenisation

### Results

- Unicode inspection: PASS
- Raw/model two-copy preprocessing: PASS
- PII masking: PASS
- spaCy sentence pipeline: PASS
- Mean Token Fertility: 1.36
- Truncation Rate: 0% at max length = 10
- IDs-to-embeddings pipeline: PASS
- Core checkpoint: `DAY1_NOTEBOOK1_CORE=PASS`

### Decision

Used a conservative preprocessing approach for Arabic. 
The raw text is preserved for auditability, while a separate model-ready copy is used for preprocessing and tokenisation.

### Notebook

`notebooks/01_text_processing_tokenization.ipynb`
