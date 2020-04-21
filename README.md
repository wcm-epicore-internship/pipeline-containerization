# pipeline-containerization
Configure, run and test bioinformatic pipelines in a container environment

### What is a container?
A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another.

### Working hypothesis
Configuring bioinformatics pipelines to run in containers provides portability and scalability to the software system.

### Bioinformatics Pipelines need portability
As the bioinformatics field is getting bigger and more patients’ and biomedical research data is accumulating, the new demands on the research infrastructure side are emerging. There is a “burning need” of having or developing the bioinformatics pipelines that perform quality control, statistical analysis, mapping, etc. Once such a pipeline is developed, it should be usable and portable. One such need is the ability to run the pipeline independently, without having specific system requirements. Configuring bioinformatics pipelines to run in containers provides portability and scalability to the software system. It makes the bioinformatics pipelines more available and easier to use, while preserving the same quality of data output.

### How can we achieve the portability?
There were a few ways to solve this issue: one was a virtual machine, another one was Docker software, and the last one was Singularity, a newer containerization software developed in academia for scientific purposes. The virtual machine (VM) is an emulator of a computer system. There are system VMs and process VMs. System VMs emulate a whole machine, while process VM is used to execute computer programs in a platform-independent environment. For the pipeline transferability purpose a process VM would suit. The main issue with the implementation here is that virtual machines are heavy, they require a lot of memory to run and maintain. So, containers seemed to be a better option. Choosing between Docker and Singularity we chose Singularity because it is a containerization software designed specifically for the scientific use.   

### What is a container and why we should use it?
A container is a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another.

Reasons why we should use a container:
-	Performance time: comparable to native 
-	Software restrictions: no restrictions. You can use your own software environment 
-	Reproducibility
-	Compatibility: support most Linux distributions (and in case when not, like it happened during this research project, some extra steps must be taken, including installing and running different compilers)
-	Portability: once the container is built, it can be run anywhere

One of the containers’ benefits is its architecture which enables us to divide an application into distributed objects or containers. When the application is broken down in such a manner, it is easy to place it wherever it is convenient: on different physical and virtual machines, in the cloud or not. Such flexibility provides an opportunity to establish a secure system with a minimum effort. Also, the system will run successfully even in the event of failure.
Singularity
Singularity is the containerization software, designed for the scientific purposes, the main tool of the project. It is a free, cross-platform and open-source computer program that performs operating-system-level virtualization also known as containerization.

### How to build a container?
Use the command Build
```
$sudo singularity build star.simg star.srec```

### Run container 
we don't need sudo, so just $ singularity <command> should work
STAR

### STAR Output files
*.bam - The aligned reads in the widely accepted BAM format (a binary version of the SAM format) for each sample.

*.bai - An index file (.bai file) for each sample which allows for easier viewing of the data in genome browsers such as UCSC Genome Browser (http://genome.ucsc.edu) or IGV (http://www.broadinstitute.org/igv).

*-SJ.out.tab - The high confidence collapsed splice junctions in tab-delimited format. Only junctions supported by uniquely mapping reads are reported.

*-Log.final.out - A text file containing the STAR aligner generated summary statistics for the alignment of each sample.

### Packages installed/ Pipeline
-	STAR (version, download link )
-	Flexbar (version, download link )
-	Samtools (version, download link )

### Run the Pipeline inside the container
Pass1

```cd STAR_bio
/home/username/STAR_bio/STAR-2.7.1a/bin/Linux_x86_64_static/STAR --genomeDir Data/mygenomedir/ --runMode genomeGenerate  --genomeFastaFiles referenceGenomes/gencode.vM22.transcripts.fa --limitGenomeGenerateRAM 6220135808  --genomeSAsparseD 3 --genomeSAindexNbases 12 -- genomeChrBinNbits 14
/home/username/STAR_bio/STAR-2.7.1a/bin/Linux_x86_64_static/STAR --genomeDir Data/mygenomedir/  --readFilesIn Data/HA_KO_high_NoIndex_L006_R1_001.fastq 
```

Pass2

```
STAR --genomeDir Data/mygenomedir/ --readFilesIn  Data/Sample_HA_KO_high/HA_KO_high_NoIndex_L006_R1_001.fastq.gz --limitGenomeGenerateRAM 99431595392 --runThreadN 12
```

### File infrastructure in the host file system:
a)	.fasta should be in the directory

b)	.fastq should be in the directory

c)	Directory DATA/genomdir should exist 

### Recipe file for Flexbar

    yum -y install xz-devel
    yum -y install cmake
    yum -y install unzip
    cd /
    wget http://packages.seqan.de/seqan-library/seqan-library-1.4.2.tar.bz2
    tar -xvjf seqan-library-1.4.2.tar.bz2

    wget https://github.com/seqan/flexbar/archive/v2.4.0.tar.gz
    tar xvzf v2.4.0.tar.gz
    mv seqan-library-1.4.2/include flexbar-2.4.0

    wget https://github.com/intel/tbb/archive/tbb_2019.zip
    unzip tbb_2019.zip
    cd tbb-tbb_2019
    gmake
    cp -R include/tbb ../flexbar-2.4.0/include/
    cd /
    cd flexbar-2.4.0
    cmake .
    export LD_LIBRARY_PATH=/YourPath/flexbar-master-linux:$LD_LIBRARY_PATH
    %environment
    export PATH=$PATH:/samtools-1.9
    
    
### Recipe for Samtools

```bootstrap: docker
  From: centos:7
 
  %post
      yum -y update
      yum -y install wget git gzip bzip2 gcc gcc-c++ make zlib-devel epel-release
      yum -y install python-pip
      cd /
      wget https://github.com/samtools/samtools/releases/download/1.9/samtools-1.9.tar.bz2
      bzip2 -d samtools-1.9.tar.bz2
      tar -xf samtools-1.9.tar
      cd samtools-1.9/
      ./configure --without-curses
      make
      make install
 
  %environment
      export PATH=$PATH:/samtools-1.9
 ```
 
 
 
 
 
 
 
