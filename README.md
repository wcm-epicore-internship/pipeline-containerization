# pipeline-containerization
Configure, run and test bioinformatic pipelines in a container environment

### What is a container?
A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another.

### Working hypothesis
Configuring bioinformatics pipelines to run in containers provides portability and scalability to the software system.

### Road map
1. Research: Containers vs Virtual Machines, Containers vs Java Virtual Machine, Docker vs Singularity, strengths and weaknesses, use cases.
1. Download the star-aligner pipeline for post processing of RNASeq data (from gitlab), and get familiar with the tools/ dependencies and system requirements: https://scu-git.med.cornell.edu/git/luce/star-aligner
1. Collate test data set - Download raw data from our data portal PubShare (https://abc.med.cornell.edu/pubshare) - use Project EC-DB-1675.
1. Run (non-containerized) pipeline test data set on 2 or more different machines
1. Compare output, run time, resource use, etc
1. Configure pipeline to run in container, including scripts for set up and management
1. Run containerized pipeline on the same machines
1. Compare output, run time, resource use, etc
1. Run containerized pipeline in the cloud
1. Compare output, run time, resource use, etc
1. Chart and report comparisons

### Notes
Examples of SCU supported singularity containers: https://scu-git.med.cornell.edu/git/scu-singularity
