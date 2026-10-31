# Odia historical letterpress Tesseract training

This repository holds the corpus, scripts and test images used to train `ori_hist.traineddata`. The model is in [tessdata_contrib/ori_hist](https://github.com/tesseract-ocr/tessdata_contrib/tree/main/ori_hist).

## Requirements

- Tesseract 5 with the training tools (`text2image`, `lstmtraining`, `combine_tessdata`)
- GNU Make 4 or later, for [tesstrain](https://github.com/tesseract-ocr/tesstrain)
- Python 3
- `Chapakala19Regular.ttf` from [Chapakala](https://github.com/ofdn/Chapakala), placed in `fonts/`

## Steps

Run each step from the repository root:

```bash
mkdir -p tessdata_best fonts
./00-setup.sh
./01-generate-images.sh
./02-extract-model.sh
./03-create-lstmf.sh
./04-train.sh
./05-package.sh
./06-test.sh <image> <gt.txt>
```

`00-setup.sh` clones tesstrain and downloads `ori.traineddata` from tessdata_best. The model is fine-tuned from it.

## Corpus

`corpus/ori.training_text` has 5,251 lines. Glyph-coverage tables come first: conjuncts with every matra, kerning pairs, spacing pairs and the full character inventory. Odia Wikipedia sentences follow, cleaned to at least 85% Odia characters and cut at 80 characters.

To rebuild the corpus, pass the folder of font-testing files to `corpus/build-quality-corpus.py`:

```bash
python3 corpus/build-quality-corpus.py <folder>
```

## Contents

| Path | Purpose |
|------|---------|
| `corpus/ori.training_text` | Training corpus |
| `corpus/build-quality-corpus.py` | Builds the corpus from font-testing files and Wikipedia text |
| `corpus/clean-corpus.py` | Removes non-Odia characters from the corpus |
| `test-images/001.png` to `003.png` | Crops from 1875 Odia books |
| `test-images/1875_1.png` | A page of the 1875 Odia Bible |
| `test-images/Rath1910-1.png` | A page of a 1910 Odia text |
| `test-images/*.gt.txt` | Ground truth typed by hand |

## Credits

Subhashish Panigrahi trained the model and designed Chapakala 19, a revival of a 19th-century Odia letterpress typeface.

## Licence

Code: Apache 2.0, the same as Tesseract. Odia Wikipedia text keeps CC BY-SA.
