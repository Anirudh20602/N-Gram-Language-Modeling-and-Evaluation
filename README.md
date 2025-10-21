
# NLP Homework 2 — N-Gram Language Modeling and Evaluation

This repository contains the full implementation for **Homework 2**: training and evaluating N-gram Language Models on the **Penn Treebank (PTB)** dataset using:
- **3.1** Maximum Likelihood Estimation (MLE) for N = 1, 2, 3, 4
- **3.2.1** Add‑1 (Laplace) Smoothing (Trigram)
- **3.2.2** Linear Interpolation (Trigram) and Stupid Backoff (Trigram)

All evaluation is performed with **Perplexity (PP)** on the **test** set. Validation is used for tuning interpolation weights and (optionally) the backoff discount parameter.

---

## Results (from `NLP_HW_2.ipynb`)

| Model | Dev PP | Test PP |
|---|---:|---:|
| MLE Unigram (N=1) | — | **639.30** |
| MLE Bigram (N=2) | — | **INF** |
| MLE Trigram (N=3) | — | **INF** |
| MLE 4-gram (N=4) | — | **INF** |
| Trigram + Add‑1 (Laplace) | — | **3307.92** |
| Trigram + Linear Interp (0.2, 0.3, 0.5) | 224.51 | — |
| Trigram + Linear Interp (0.1, 0.3, 0.6) | 256.53 | — |
| **Trigram + Linear Interp (0.3, 0.3, 0.4)** | **211.61** | **195.75** |
| Trigram + Stupid Backoff (α = 0.4) | — | **185.67** (pseudo‑PP) |

> **Note**: Stupid Backoff is not a normalized probability distribution. The reported “perplexity” is a **pseudo‑PP** computed from the backed‑off scores for comparison.

Generated sentences (5) from best model (Linear Interpolation) are included in the notebook output.

---

## File Layout

```
.
├─ NLP_HW_2.ipynb               # Main notebook — all parts runnable independently
├─ NLP_HW2_Final_Report.tex     # Overleaf-ready LaTeX report (complete)
└─ README.md                    # This file
```

> If you are using Google Colab, you only need the notebook **NLP_HW_2.ipynb** and the three PTB text files at the specified paths.

---

## Dataset

Use the PTB (Penn Treebank) dataset with the following **exact** file paths (as used in the notebook):

- Train:  `/content/ptb.train.txt`  
- Valid:  `/content/ptb.valid.txt`  
- Test:   `/content/ptb.test.txt`

Each file is expected to be **pre-tokenized**: one sentence per line, tokens split by whitespace.  
If needed, upload to Colab via the left file pane or with Python I/O and ensure the paths match.

---

## Environment

- Python 3.9+
- Standard library only (no external dependencies required):
  - `collections.Counter`
  - `math`

> If you want to run locally, any standard Python environment (3.8–3.12) works.

---

## How to Run (Google Colab)

1. **Open the notebook** `NLP_HW_2.ipynb` in Google Colab.
2. **Upload the dataset files** to the exact paths:
   - `/content/ptb.train.txt`
   - `/content/ptb.valid.txt`
   - `/content/ptb.test.txt`
3. **Execute the cells** in order or jump to the part you want:
   - **3.1 MLE (N=1–4)** — one self-contained cell.  
   - **3.2.1 Add‑1 (Laplace) (Trigram)** — separate self-contained cell.  
   - **3.2.2 Linear Interpolation + Stupid Backoff (Trigram)** — separate self-contained cell (includes Dev tuning and Test evaluation).  
   - **4.4 Generation** — uses the best model (interpolation) to print 5 sentences.
4. **Expected outputs** appear in the notebook cell outputs:
   - Perplexity numbers for each model.
   - A short tuning printout for interpolation lambdas (Dev set).
   - Final Test PP for the best interpolation weights.
   - Stupid Backoff pseudo‑PP.
   - Five generated sentences.

### Re-running only a section
Each major section is **self-contained**. You can run only that cell if you:
- Have executed the small helper functions block (if separated), or
- Use the version where each section contains its own helpers (as in the provided cells).

---

## How to Run (Local Jupyter)

1. Install Jupyter if needed:
   ```bash
   pip install notebook
   ```
2. Start Jupyter:
   ```bash
   jupyter notebook
   ```
3. Place the PTB text files at the paths (or edit the constants at the top of each cell):
   ```text
   /content/ptb.train.txt
   /content/ptb.valid.txt
   /content/ptb.test.txt
   ```
4. Open and run `NLP_HW_2.ipynb` as in Colab.

> **Tip**: If you are running locally and prefer project-relative paths, change the constants:
> ```python
> TRAIN_PATH = "data/ptb.train.txt"
> VALID_PATH = "data/ptb.valid.txt"
> TEST_PATH  = "data/ptb.test.txt"
> ```
> Then place the files in `./data/`.

---

## Reproducibility Notes

- PTB is pre-tokenized; the notebook uses **whitespace split** and adds start/end tokens (`<s>`, `</s>`) for $n \ge 2$.
- **OOV handling**: tokens not in training vocab are treated as `<unk>` if your preprocessing maps them; for pure MLE, unseen n‑grams yield `INF` PP as required.
- **Linear Interpolation**:
  - Try multiple $(\lambda_1,\lambda_2,\lambda_3)$ with $\sum \lambda_i = 1$ on the **validation** set.
  - Use the **best Dev** weights to report **Test** PP.
- **Stupid Backoff**:
  - Default $\alpha = 0.4` (common choice). You can sweep α on the validation set (e.g., {0.2, 0.3, 0.4, 0.6}).
  - Report that “perplexity” is **pseudo‑PP** (scores are not normalized).

---

## Troubleshooting

- **Got `INF` on MLE bigram/trigram/4‑gram?** That’s correct — pure MLE assigns zero to unseen test n‑grams.
- **Add‑1 PP very large?** Expected. Laplace over-smooths; use Interpolation for lower PP.
- **Mismatch in file paths**: Verify the three PTB files are present exactly at `/content/...`.  
- **Memory/Speed**: On PTB, counting with `Counter` is fast and light. If needed, reduce memory by streaming sentences line‑by‑line.

---

## Compiling the LaTeX Report

You can upload `NLP_HW2_Final_Report.tex` to **Overleaf** and compile with default settings.  
The report already includes:
- Introduction, Dataset/Preprocessing
- Methods for 3.1–3.2
- Full Results (with your actual PP values)
- Analysis (4.1–4.4)
- Compliance checklist
- Conclusion

## License
This homework scaffold and code are provided for educational use in the course context.
