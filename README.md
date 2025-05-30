
# simpyQ

![Banner](https://img.shields.io/badge/version-1.0.2-blue)
![python3.x](https://img.shields.io/badge/python-3.x-brightgreen.svg)
[![Documentation Status](https://readthedocs.org/projects/ansicolortags/badge/?version=latest)](http://ansicolortags.readthedocs.io/?badge=latest)
[![GPLv3 license](https://img.shields.io/badge/License-GPLv3-blue.svg)](http://perso.crans.org/besson/LICENSE.html)
[![Open Source? Yes!](https://badgen.net/badge/Open%20Source%20%3F/Yes%21/blue?icon=github)](https://github.com/Naereen/badges/)
> CLI tool for querying simulation CSV data using natural language.
[![PyPI version](https://img.shields.io/pypi/v/simpyq.svg)](https://pypi.org/project/simpyq/)

Analyze and visualize simulation data from CSV files using natural language queries interpreted via spaCy and a rule-enhanced NLP pipeline.

---

##  Features

- Terminal-based CLI tool with live interactive mode  
- Natural language processing of signal queries (e.g., `mean of Vout`)  
- Signal name indexing and pretty terminal display using Rich  
- Plotting of signal vs time with auto-saving to file  
- Logging of results and errors to timestamped log files  
- Lightweight, extensible, and easy to adapt to other domains  

---

##  Installation

Note : you need to have Spacy installed on your machine before installing.
## 🧠 spaCy Setup (Required Before Using `simpyq`)

Before installing and using the `simpyq` CLI tool, you need to install [spaCy](https://spacy.io/usage) along with its transformer-based English language model.

### 1. Install spaCy (choose **CPU** or **CUDA** based on your system):

**For CPU-only systems:**
```bash
pip install -U pip setuptools wheel
pip install -U 'spacy[cuda11x]'
python -m spacy download en_core_web_trf



```bash
# Create environment (recommended)
conda create -n simpyq_env python=3.10
conda activate simpyq_env

# Install dependencies
pip install -r requirements.txt

# Or install manually
pip install pandas numpy matplotlib rich pyfiglet spacy
python -m spacy download en_core_web_trf

or if you have Spacy already installed then : pip install simpyq
```

---

##  Usage

```bash
# Run in one-shot mode (process query then exit)
python simpyq/cli.py path/to/data.csv "mean of Vout"

# Show available signals
python simpyq/cli.py path/to/data.csv --show

# Plot signal(s)
python simpyq/cli.py path/to/data.csv --plot Vout Iload --start 0.1 --end 0.9
```

###  Interactive Mode

```bash
# Start interactive REPL
python simpyq/cli.py path/to/data.csv
```

Then just type queries like:

```
mean of Vout
rms of Iin
(mean of Vout) + (rms of Iload) / 2
exit()
```

---

##  Example Queries

| Query                           | Description                          |
| ------------------------------- | ------------------------------------ |
| `mean of Vout`                  | Computes the average value of `Vout` |
| `rms of Iload`                  | Root-mean-square of `Iload`          |
| `max of Temp - min of Temp`     | Temperature swing                    |
| `integral of Pin`               | Energy under the `Pin` curve         |
| `rms of Vout + mean of Iin * 2` | Composite math query                 |
| `plot of Vout and Iin`          | (Use `--plot` flag instead)          |


---

##  Supported Operations

```python
OPERATIONS = {
    "mean": np.mean,
    "average": np.mean,
    "rms": lambda x: np.sqrt(np.mean(np.square(x))),
    "std": np.std,
    "variance": np.var,
    "max": np.max,
    "min": np.min,
    "abs max": lambda x: np.max(np.abs(x)),
    "abs min": lambda x: np.min(np.abs(x)),
    "sum": np.sum,
    "peak-to-peak": lambda x: np.ptp(x),
    "median": np.median,
    "integral": lambda x: np.trapz(x),
    "squared mean": lambda x: np.mean(np.square(x)),
    "derivative": lambda x: np.gradient(x),
    "diff": np.diff,
}
```

---

##  How It Works

- Loads CSV data and extracts signal names  
- Injects them as named entities into the spaCy NLP pipeline  
- Parses user query and extracts operations and signals  
- Computes the operations and evaluates full expressions  
- Plots and saves results as needed  

---

##  NLP Training / Fine-tuning

You can customize the NLP model to better understand domain-specific expressions.

**To train or adapt:**

1. Edit `get_nlp()` to support additional patterns  
2. Add `EntityRuler` rules:

```python
patterns = [{"label": "ELECTRONIC_SIGNAL", "pattern": name} for name in signal_names]
ruler.add_patterns(patterns)
```

3. (Optional) Fine-tune spaCy model with labeled data:

```bash
python -m spacy train config.cfg --output ./model --paths.train ./train.spacy --paths.dev ./dev.spacy
```

---

##  License

GPL v3 License

---

##  Acknowledgments

Thanks to the open-source community and tools like:

- [spaCy](https://spacy.io)  
- [NumPy](https://numpy.org)  
- [Matplotlib](https://matplotlib.org)  
- [Rich](https://github.com/Textualize/rich)  
```
````
