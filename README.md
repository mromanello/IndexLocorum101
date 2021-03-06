# IndexLocorum101

[![Binder](https://mybinder.org/badge_logo.svg)](https://mybinder.org/v2/gh/mromanello/IndexLocorum101/master)

A template project for those wanting to create an *index locorum* for their publications.

## Installation

See [`INSTALL.md`](INSTALL.md).

## Configuration

This template comes with batteries included, but you will have to adapt a bit the configuration. The project configuration file is in [`config/project.ini`](config/project.ini).

Make sure you change the path for the following settings:
- `preproc.treetagger_home`: the path to `TreeTagger`

## Data preparation

This template project comes a short example document, i.e.[Bryn Mawr ClassicalReview 2013-01-10](http://bmcr.brynmawr.edu/2013/2013-01-10.html).

The documents to be processed need to be placed in the sub-folder `orig` within your working directory.

In this example project `general.working_dir = ./data`, thus the input files are placed in `./data/orig/`. The script will then create further subfolders to store temporary or intermediate files.

## Running the pipeline

When you install the `CitationExtractor` (version `>= 1.7.0`) the bash command `citedloci-pipeline` will be automatically installed in your system, which allows you to run the pipeline.

For a detailed explanation of each pipeline step, please refer to the Jupyter notebook [`step-by-step.ipynb`](step-by-step.ipynb).

### 1. Pre-processing

```bash
citedloci-pipeline do preproc --config=config/project.ini
```

At this point you should have a tokenized and PoS-tagged file at `data/iob/bmcr_2013-01-10.txt` (if you've kept the default project settings).

Try:

```bash
cat data/iob/bmcr_2013-01-10.txt
```

### 2. Named entity recognition

```bash
citedloci-pipeline do ner --config=config/project.ini
```

At this point you should have a JSON file with entities annotated at `data/json/bmcr_2013-01-10.json`.

Try:

```bash
# requires jq, see https://stedolan.github.io/jq/download/
cat data/json/bmcr_2013-01-10.json|jq ".entities"
```

### 3. Relation extraction

```bash
citedloci-pipeline do relex --config=config/project.ini
```

At this point you should have a JSON file with relations annotated at `data/json/bmcr_2013-01-10.json` (it overwrites the previous one).

Try:

```bash
# requires jq, see https://stedolan.github.io/jq/download/
cat data/json/bmcr_2013-01-10.json|jq ".relations"
```

### 4. Named entity disambiguation

```bash
citedloci-pipeline do ned --config=config/project.ini
```

At this point you should have a JSON file with entities disambiguated at `data/json/bmcr_2013-01-10.json` (it overwrites the previous one).

Try:

```bash
# requires jq, see https://stedolan.github.io/jq/download/
cat data/json/bmcr_2013-01-10.json|jq ".entities"
```
