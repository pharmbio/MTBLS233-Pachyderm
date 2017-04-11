# MTBLS233-Pachyderm
In this page we introduce an metabolomics preprocessing workflow that you can run using [Pachyderm](https://github.com/pachyderm/pachyderm), a distributed data-processing tool built on software containers that enables scalable and reproducible pipelines.

## Introduction
The main goal of [the study](http://www.sciencedirect.com/science/article/pii/S000326701630647X) performed on MTBLS233 was to produce quantitative information of the highest possible number of reliable features in untargeted metabolomics. In order to do so, diverse approaches of mass spectromic acquisition parameter tuning were tested in order to maximize the number of spectral features.

We aimed at rereating the workflow used in the MTBLS233 study in a distributed manner by using Pachyderm. The workflow was originally implemeted in [OpenMS](https://www.openms.de/) v. 1.1.1. followed by the downstream analysis in [KNIME](https://www.knime.org/). Here we fire up the preprocessing pipeline using Pachyderm, a tool built on top of Kubernetes that allows to process the data in a distributed fashion and to keep track of the input/output data from every stage of our the pipeline (think “git for data”), such that it is possible to track the provenance of results and accurately reproduce scientific workflows.
