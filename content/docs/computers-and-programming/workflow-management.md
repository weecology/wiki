---
title: "Workflow Tools - snakemake & targets"
linkTitle: "Workflow Tools"
summary: " "
---

## Introduction

Workflow tools let you automate the running and rerunning of code with multiple steps.
For example we use it for managing our image processing workflow for our [Everglades research](https://everglades.weecology.org/).

## Python - snakemake

### Getting Started

* [Official snakemake tutorial](https://snakemake.readthedocs.io/en/stable/tutorial/tutorial.html) 
* [C. Titus Brown's draft book on using snakemake for bioinformatics](https://farm.cse.ucdavis.edu/~ctbrown/2023-snakemake-book-draft/)

### Handling complex inputs with input functions

In our workflows with deal with complex input-output structures, like having early phases of the pipeline work on one flight (file) at a time and later phases work on all of the files from a given site and year as a group.

This can be accomplished by defining custom input functions.

* [Introduction to Input Functions](https://www.embl.org/groups/bioinformatics-rome/blog/2022/10/guest-post-snakemake-input-functions-by-tim-booth/)
* See our [Everglades workflow Snakefile](https://github.com/weecology/EvergladesTools/blob/main/Zooniverse/Snakefile)

### Testing snakemake with partial wildcards

When testing big workflow it is often useful to run the workflow on a subset of the data.
For example our Everglades workflow runs on all years, sites, and flight at once, but we might want to test a site year-site combination when making a change.
To prepare to do this replace your Wildcards object with the component lists for the main workflow. E.g.,

```python
ORTHOMOSAICS = glob_wildcards("/{year}/{site}/{flight}.tif")
FLIGHTS = ORTHOMOSAICS.flight
SITES = ORTHOMOSAICS.site
YEARS = ORTHOMOSAICS.year
```

The components are just lists, so you can then replace them with whatever pieces of the full workflow you want to test. E.g.,:

```python
ORTHOMOSAICS = glob_wildcards("/{year}/{site}/{flight}.tif")
TEST = glob_wildcards("/blue/ewhite/everglades/orthomosaics/2022/StartMel/{flight}.tif")
FLIGHTS = TEST.flight # ORTHOMOSAICS.flight
SITES = ["StartMel"] * len(FLIGHTS) # ORTHOMOSAICS.site
YEARS = ["2022"] * len(FLIGHTS) #ORTHOMOSAICS.year
```

## R - targets