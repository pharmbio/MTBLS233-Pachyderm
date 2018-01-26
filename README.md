# MTBLS233-Pachyderm
In this page we introduce an metabolomics preprocessing workflow that you can run using [Pachyderm](https://github.com/pachyderm/pachyderm), a distributed data-processing tool built on software containers that enables scalable and reproducible pipelines.

## Introduction
The main goal of [the study](http://www.sciencedirect.com/science/article/pii/S000326701630647X) performed on MTBLS233 was to produce quantitative information of the highest possible number of reliable features in untargeted metabolomics. In order to do so, diverse approaches of mass spectromic acquisition parameter tuning were tested in order to maximize the number of spectral features.

The workflow was originally implemeted in [OpenMS](https://www.openms.de/) v. 1.1.1. followed by the downstream analysis in [KNIME](https://www.knime.org/). Here we show you how to run the preprocessing workflow using Pachyderm, a tool built on top of Kubernetes that allows to process the data in a distributed fashion and to keep track of the input/output data from every stage of our the pipeline (think “git for data”), such that it is possible to track the provenance of results and accurately reproduce scientific workflows.

## Run the preprocessing workflow

Once you are logged into the master node, start by installing the Pachyderm client:

```bash
$ curl -o /tmp/pachctl.deb -L https://github.com/pachyderm/pachyderm/releases/download/v1.6.7/pachctl_1.6.7_amd64.deb && sudo dpkg -i /tmp/pachctl.deb
```

### Ingest the MTBLS233 dataset from MetaboLights

[MetaboLights](http://www.ebi.ac.uk/metabolights/) offers an FTP service, so we can ingest the MTBLS233 dataset in a terminal. 

1. First create a folder where you will sotre the data and navigate to it
2. Ingest the dataset using **wget**:

```bash
# Dataset retrieval
mkdir dataset
cd dataset
wget ftp://ftp.ebi.ac.uk/pub/databases/metabolights/studies/public/MTBLS233/00*alternate_pos_low_mr.mzML
```

### Add the MTBLS233 dataset to Pachyderm

Create a repo called `mrpo` and push the dataset into it. 

```bash
# Dataset upload 
pachctl create-repo mrpo
pachctl put-file mrpo master -c -r -p 3 -f . 
```

### Process the data

Now that the data is in the repository, it’s time to use the execute the pipeline. Four different jobs compose the pipeline, which can be found in the `./pipelines`directory.

```bash
pachctl create-pipeline -f ./path/to/pipelines/FileFilter.json
pachctl create-pipeline -f ./path/to/pipelines/FeatureFinderMetabo.json
pachctl create-pipeline -f ./path/to/pipelines/FeatureLinkerUnlabeledQT.json
pachctl create-pipeline -f ./path/to/pipelines/TextExporter.json
```
After the whole workflow has been successfully executed, the resulting CSV file generated by the `TextExporter` in OpenMS will be saved in the `TextExporter`repository. You can download the file simply by using: 

```bash
pachctl get-file TextExporter master <path-to-file-in-repo> > <local-path-output>
```

The `<path-to-file>` can be obtained by checking the list of files outputed to the `TextExporter` repository in a given branch:
```bash
pachctl list-file TextExporter master
```
