# MTBLS233-Pachyderm
In this page we introduce an metabolomics preprocessing workflow that you can run using [Pachyderm](https://github.com/pachyderm/pachyderm), a distributed data-processing tool built on software containers that enables scalable and reproducible pipelines.

## Introduction
The main goal of [the study](http://www.sciencedirect.com/science/article/pii/S000326701630647X) performed on MTBLS233 was to produce quantitative information of the highest possible number of reliable features in untargeted metabolomics. In order to do so, diverse approaches of mass spectromic acquisition parameter tuning were tested in order to maximize the number of spectral features.

We aimed at rereating the workflow used in the MTBLS233 study in a distributed manner by using Pachyderm. The workflow was originally implemeted in [OpenMS](https://www.openms.de/) v. 1.1.1. followed by the downstream analysis in [KNIME](https://www.knime.org/). Here we fire up the preprocessing pipeline using Pachyderm, a tool built on top of Kubernetes that allows to process the data in a distributed fashion and to keep track of the input/output data from every stage of our the pipeline (think “git for data”), such that it is possible to track the provenance of results and accurately reproduce scientific workflows.

## Run the preprocessing workflow

We assume that you have a Kubernetes cluster or Minikube up and running. Start by installing the Pachyderm client:
```bash
# For OSX:
brew tap pachyderm/tap && brew install pachyderm/tap/pachctl@1.3
# For Linux (64 bit):
$ curl -o /tmp/pachctl.deb -L https://github.com/pachyderm/pachyderm/releases/download/v1.3.17/pachctl_1.3.17_amd64.deb && sudo dpkg -i /tmp/pachctl.deb```

### Ingest the MTBLS233 dataset from MetaboLights

[MetaboLights](http://www.ebi.ac.uk/metabolights/) offers an FTP service, so we can ingest the MTBLS233 dataset with Linux commands. 

1. First open a Jupyter terminal: `New > Terminal`
2. Ingest the dataset using **wget**:

```bash
# Dataset A
wget ftp://ftp.ebi.ac.uk/pub/databases/metabolights/studies/public/MTBLS233/*alternate_pos_low_mr.mzML
# Dataset #B
wget ftp://ftp.ebi.ac.uk/pub/databases/metabolights/studies/public/MTBLS233/*alternate_pos_high_mr.mzML
```
