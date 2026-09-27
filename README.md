# Shakespeare LSTM Text Generation

A word-level text generator trained on Shakespeare's *Complete Works*. The Jupyter notebook walks through downloading and cleaning the text, preparing next-word examples, training an LSTM language model, evaluating it, and generating continuations from seed phrases.

## Requirements

- Python 3.10 or newer
- TensorFlow and Jupyter
- Internet access for the initial Project Gutenberg download, unless using a local text file

Install the dependencies in a virtual environment:

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install tensorflow jupyter ipykernel
```

## Run the Notebook

Start Jupyter from the project directory:

```powershell
jupyter notebook
```

Open `lstm_text_generation.ipynb` and run its cells from top to bottom. The notebook downloads the text automatically, trains the model, evaluates it, and prints generated continuations for three seed phrases. The default training run took about 20 minutes on the CPU-only Windows environment used for this project; runtime depends on your hardware.

To try the architecture and context-length comparison, set `RUN_BONUS_EXPERIMENTS = True` in the bonus cell and run that cell. Its three comparison trials take additional time.

## Dataset

The notebook uses Project Gutenberg's [The Complete Works of William Shakespeare, ebook 100](https://www.gutenberg.org/ebooks/100). It removes Gutenberg's wrapper when detected, lowercases the text, removes punctuation, and tokenizes by whitespace. For a local/offline dataset, place a UTF-8 text file named `shakespeare.txt` beside the notebook.

The notebook samples the first 100,000 tokens by default and caps the vocabulary at 8,000 entries to keep training bounded. These settings can be increased for broader coverage at the cost of memory and training time. Check the dataset's legal status and terms for your jurisdiction and intended use.

## Model and Results

The model uses a 96-dimensional embedding, one 128-unit LSTM layer, and a softmax output over the vocabulary. It predicts the next word using sparse categorical cross-entropy and Adam. Training uses a chronological 90/10 training-validation split, early stopping, and a best-validation checkpoint.

On the recorded run, training stopped after epoch 7. Validation loss was 6.7297, next-word accuracy was 7.42%, and perplexity was 836.93. The samples show recognizable word-level patterns but are often incoherent; this is a small demonstration model, not a high-quality literary generator.

The optional comparison, trained for three epochs per configuration, recorded:

| Context length | LSTM layers | Validation accuracy | Perplexity |
| ---: | ---: | ---: | ---: |
| 12 | 1 | 6.16% | 842.61 |
| 24 | 1 | 6.04% | 853.92 |
| 24 | 2 | 3.62% | 886.56 |

These short-run results do not establish that deeper models or longer contexts are generally worse; they only describe this experiment and configuration.

## Files

- `lstm_text_generation.ipynb`: preprocessing, model training, evaluation, generation, and optional comparison.
- `.gitignore`: excludes the local virtual environment, notebook checkpoints, and generated Keras model files.

The trained `best_lstm_text.keras` file is not tracked. The notebook recreates the model, while faithful text generation also depends on the vocabulary mapping produced during preprocessing.
