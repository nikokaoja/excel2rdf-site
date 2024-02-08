# Automatic Workflow

Here we describe how you can automatize the workflow described in [Manual Workflow](../ManualWorkflow) page.

The provided description makes use of the following Github repository:

[https://github.com/fair-data-collective/excel2rdf-template](https://github.com/fair-data-collective/excel2rdf-template)

which contains all necessary files and Github configuration to automatically convert and validate your Excel vocabularies.

Once familiar with the process, simply fork the above repository and update it per your project needs.

## Structuring Gihtub repository

Here we shortly describe the structure of [excel2rdf-template](https://github.com/fair-data-collective/excel2rdf-template) repository which can be used for:

- Maintaining (and version controlling)
- Converting and
- Validating your Excel controlled vocabularies

The structure is as follows:

```bash
├── .github
│   └── workflows
│       └── excel2rdf.yml
├── LICENSE
├── README.md
├── logs
│   ├── conversion.log
│   └── validation.log
├── vocabulary.ttl
└── vocabulary.xlsx
```

The most important files of this repository are:

- `vocabulary.xlsx`, the Excel template for building a controlled vocabulary that will serve as input for `xls2rdf` tool
- `vocabulary.ttl`, the resulting file generated when running `xls2rdf` on `vocabulary.xlsx`
- `conversion.log`, the file that stores logs of `xls2rdf` tool
- `validation.log`, the file that stores logs produced when running `qSKOS` on `vocabulary.ttl` to validate the converted vocabulary
- `excel2rdf.yml`, the Github action configuration file that automatizes the workflow described in [Manual Workflow](../ManualWorkflow) page.

## Setting up Github actions

Following the above structure, we set up the Github action via `excel2rdf.yml`, which is placed in `.github/workflows/` folder (it must be there for Github to automatically picks it up and execute it).

The content of `excel2rdf.yml` file looks like this

```bash
name: excel2rdf

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  convert-validate-deploy-vocabulary:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2

      - name: prepare # pulls repository and dowloads xls2rdf and qSKOS
        run: |
          git config user.name github-actions
          git config user.email github-actions@github.com
          git pull
          curl -L https://github.com/cmader/qSKOS/releases/download/2.0.3/qSKOS-cmd.jar -o qSKOS.jar
          curl -L https://github.com/sparna-git/xls2rdf/releases/download/2.1.1/xls2rdf-app-2.1.1-onejar.jar -o xls2rdf.jar

      - name: build # converts vocabulary.xlsx to vocabulary.ttl
        run: |
          java -jar xls2rdf.jar convert -i ./vocabulary.xlsx -o ./vocabulary.ttl -l en
          mv xls2rdf.log ./logs/conversion.log

      - name: validate # validates vocabulary.ttl against set of qSKOS tests
        run: |
          java -jar qSKOS.jar analyze -dc mil,bl ./vocabulary.ttl -o ./logs/validation.log

      - name: deploy # commits changes to the repository
        run: |
          rm qSKOS.jar
          rm xls2rdf.jar
          git add .
          git diff-index --quiet HEAD || git commit -m "generated new .ttl from .xlsx file"
          git push
```

The file `excel2rdf.yml` describes a single job named `convert-validate-deploy-vocabulary` consisting of the following steps:

- `prepare`: which configures git on docker `ubuntu-latest` running on a virtual machine and downloads `xls2rdf` and `qSKOS` applications from their associated Github repositories

- `build`: executes `xls2rdf` considering `vocabulary.xlsx` as its input, while producing `vocabulary.ttl` as its output, and moves and renames the log produced in this step to the folder `./logs/`

- `validate`: runs `qSKOS` on the produced `vocabulary.ttl` and stores the resulting log to the folder `./logs/`

- `deploy`: removes `qSKOS` and `xls2rdf` from the cloned repository, adds and commits changes of the cloned repository, pushes the commits to Github
