# CVE_description_parser - CVE Software & Version Extraction (spaCy NER)

This repository contains a custom Named Entity Recognition (NER) model built using spaCy to extract affected software and version information from CVE descriptions, regardless of how the CVE is written.

CVE descriptions vary significantly in structure, wording, and context. Traditional regex-based approaches often fail to generalize across this variability. This project takes a machine learning–based approach to tackle that challenge.

## Project Overview

The model is trained to parse CVE descriptions and identify:
* Affected software / products
* Affected versions (including contextual ranges such as before, through, etc.)
* Instead of relying on brittle pattern matching, the model learns from annotated CVE data, allowing it to adapt better to:
* Inconsistent sentence structures
* Different version expression styles
* Contextual version constraints
* Vendor-specific phrasing

## How It Works

### Data Labeling

CVE descriptions were manually annotated using Doccano

Entities such as software names and versions were labeled directly in raw CVE text

### Data Conversion

Custom scripts were written to convert Doccano exports into spaCy-compatible training format

### Model Training

spaCy was used to train a custom NER pipeline

The model learns contextual patterns rather than fixed expressions

### Inference

A test script is provided to run the trained model against a text file containing a list of new CVE descriptions

The script filters duplicate entities and removes derived/partial overlaps (e.g. keeping “before 0.3.4” instead of “0.3.4”)

## 📂 Archive Contents
```
.
├── modelsdir/                # Trained spaCy model directory (beta)
├── testmodel_v2.py           # Script to test the model against cve descriptions
├── 10descriptions.txt        # Sample CVE descriptions for testing
└── README.md
```

## ▶️ Usage
### 1️⃣ Install Dependencies
pip install spacy

Ensure you are using the same spaCy version the model was trained on.

You might need some troubleshooting at this point. You might need a specific version of python and specific versions of required packages.

Incase you run into problems, get started with the following solutions:
* Create a python environment with a specific python version (3.9)

pyenv virtualenv 3.9 <environment-name>

* Install required packages

```
pip install -U pip setuptools wheel
pip install -U numpy==1.23.5
pip install -U spacy==3.5.4
```

### 2️⃣ Run the Test Script

```
python testmodel_v2.py <path_to_model> <sample_descriptions_file>
```

Example:

```
python testmodel_v2.py modelsdir/model-best 10descriptions.txt
```


Each description will be printed followed by the extracted entities, with outputs separated by a dotted line.

Example Output

```
lilypond-invoke-editor in LilyPond 2.19.80 does not validate strings before launching the program specified by the BROWSER environment variable, which allows remote attackers to conduct argument-injection attacks via a crafted URL, as demonstrated by a --proxy-pac-file argument.

LilyPond        SOFTWARE
2.19.80 VERSION
............................................................

A improper neutralization of special elements used in an sql command ('sql injection') in Fortinet FortiAnalyzer version 7.4.0 through 7.4.2, FortiManager version 7.4.0 through 7.4.2 allows attacker to escalation of privilege via specially crafted http requests.

FortiAnalyzer   SOFTWARE
FortiManager    SOFTWARE
Fortinet        VENDOR
7.4.0 through 7.4.2     VERSION
............................................................

Missing Authorization vulnerability in Speedcomp Linet ERP-Woocommerce Integration allows Exploiting Incorrectly Configured Access Control Security Levels. This issue affects Linet ERP-Woocommerce Integration: from n/a through 3.5.7.

Linet ERP-Woocommerce Integration       SOFTWARE
Speedcomp       VENDOR
from n/a through 3.5.7  VERSION
............................................................

LibreOffice supports the storage of passwords for web connections in the user’s configuration database. The stored passwords are encrypted with a single master key provided by the user. A flaw in LibreOffice existed where master key was poorly encoded resulting in weakening its entropy from 128 to 43 bits making the stored passwords vulerable to a brute force attack if an attacker has access to the users stored config. This issue affects: The Document Foundation LibreOffice 7.2 versions prior to 7.2.7; 7.3 versions prior to 7.3.3.

LibreOffice     SOFTWARE
Document Foundation     VENDOR
prior to 7.2.7  VERSION
prior to 7.3.3  VERSION
............................................................
```

## Current Status (Beta/Noob)

This model is currently in beta/noobmode.

It performs well across varied CVE writing styles

False positives/negatives may still occur, though they are minimal

Results are significantly more flexible than regex-based solutions

CVE descriptions evolve over time, and no single static approach can fully capture that variability. This project is an ongoing effort to address that challenge using machine learning.

## Roadmap

Planned improvements include:
* Expanded training data
* Improved version range normalization
* Reduced false positives
* Additional entity types / tags / labels
* Better handling of vendor-specific phrasing

## Contributions

Contributions, feedback, and real-world CVE examples are welcome.

If you have suggestions or improvements, feel free to open an issue or pull request or if you can find any of my social media handles.
