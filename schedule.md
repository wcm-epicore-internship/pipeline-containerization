**See this link for markdown https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet**

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

STAR aligner: https://github.com/alexdobin/STAR



## Getting started with Singularity

1. create a recipe file eg star.srec

```
bootstrap: docker
From: centos:7

%post
	yum -y update
	yum -y install wget git gzip bzip2 gcc gcc-c++ make zlib-devel epel-release
	yum -y install python-pip
	cd /
  echo "Hello World" > hello_world.txt
	wget https://github.com/alexdobin/STAR/archive/STAR_2.3.1z9.tar.gz
	tar -xzf STAR_2.3.1z9.tar.gz
	cd STAR_2.3.1z9/source
	make
	
%environment
	export PATH=$PATH:/STAR_2.3.1z9/source
```

2. Build the image

```
sudo singularity build star.simg star.srec
```

3. Connect and check star runs
```
sudo singularity shell star.simg
```

4. Run the star pipeline
eg https://github.com/STAR-Fusion/STAR-Fusion/wiki

The object is to run the star program by itself (not the whole pipieline) with a sample fastq file

Once this is working we will start adding other components of the star pipeline



## Singularity on epicore08

```
ssh epiapps@pascal.med.cornell.edu
> (enter password - SEE NOTE BELOW)
ssh epicore08
cd /scratch001/epiapps/shaina
```
**Note: if wrong password is used 3 times account is locked out so take care to use correct password** 


## Vscode & Git
* Download Visual Studio Code onto your laptop
* Clone this project from git into your workspace in VSCode
* Add the singualrity scripts so far
* Commit and push to git







