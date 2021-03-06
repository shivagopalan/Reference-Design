
### Title: Broad Reference Pipeline Benchmarking                         
### Date: 01 December 2016                                               
### Organizations: Broad Institute and Intel's Health and Life Sciences   

-------------------------------------------------------------------------

#### Table of Contents

 * [Introduction](https://github.com/Intel-HLS/Reference-Design/blob/master/README.md#introduction)
 * [Requirements](https://github.com/Intel-HLS/Reference-Design/blob/master/README.md#requirements)
 * [Installation](https://github.com/Intel-HLS/Reference-Design/blob/master/README.md#installation)
 * [Configuration](https://github.com/Intel-HLS/Reference-Design/blob/master/README.md#configuration)
 * [Running the script](https://github.com/Intel-HLS/Reference-Design/blob/master/README.md#running-the-script)

-------------------------------------------------------------------------

#### Introduction 

The Broad Reference Pipeline Benchmarking repo contains scripts utilized 
in the "Infrastructure for Deploying GATK* Best Practices Pipeline" 
[white paper](http://www.intel.com/content/www/us/en/healthcare-it/solutions/documents/deploying-gatk-best-practices-paper.html). 
Scripts for running and benchmarking the pipeline are provided in 
this git directory, but the resource/reference files, bioinformatics and 
benchmarking tools need to be downloaded separately. 

-------------------------------------------------------------------------

#### Requirements

* Data:
  - b37bundle - https://software.broadinstitute.org/gatk/documentation/article.php?id=1213
  - WES/WGS - Please refer to the Broad Institute.
  
* Bioinformatic Tools:
  - GATK - https://software.broadinstitute.org/gatk/download/
  - BWA - https://sourceforge.net/projects/bio-bwa/files/
  - Picard - https://broadinstitute.github.io/picard/
  
* Benchmarking Tool:
  - Workflow Profiler - https://01.org/workflow-profiler

* Yum packages: 
  - openssl 
  - openssl-devel 
  - epel-release 
  - gcc
  - gcc-c++
  - freetype 
  - freetype-devel 
  - libpng-devel
  - collectl
  - sysstat
  - strace 
  - gnuplot
  
* Recommended:
   - Python3

-------------------------------------------------------------------------

#### Installation

* Install as you would normally GATK, BWA, Picard, and Workflow profiler

* Python 3 and two of its packages are required to run the workflow profiler 
  and the pipeline scripts. If not already installed, you can install python3 
  from source. Download source file (https://www.python.org/downloads/)
   
  After downloading Python3 zip follow instructions below
  ```
	1. tar -xvzf Python-3.5.0.tgz
	2. cd Python-3.5.0.tgz
	3. mkdir install
	4. ./configure --with-ensurepip --prefix=<full path of current directory>/install/
	5. make -j4
	6. make install
	7. Update PATH to point to the bin directory of Python-3.5.0
	    a. export PATH=/path/to/python/Python-3.5.0/install/bin:$PATH 
	8.  Install additional modules
	    a. python3 -m pip install numpy
	    b. python3 -m pip install matplotlib
  ```
    
* The directory structure may resemble the flow below:
  ```
   ├── Broad_Ref_Pip
      ├── b37bundle                 # resource files and reference genome
      ├── broad                     # binformatic tools
      │   ├── bwa
      │   ├── gatk3.4
      │   └── picard
      ├── Python-3.5.0
      ├── Queue_Broad_BestPractices # scala scripts for scatter gather
      ├── scripts                   # perl pipeline
      └── workflow_profiler         # Benchmarking tool
  ```
   
-------------------------------------------------------------------------

#### Configuration 

Edit the directory paths for tools and data files listed in the pipeline scripts.
   
Move to pipeline script directory:
   ```
   cd /path/to/scripts
   ```
   
For the pipeline you would like to run, open the WES/WGS script 
   ```
   vi wgs_sample_calling_data_collection_gatk_best_practices_optimized.pl
   ```
	
Edit the input file extenstion so that it matches your input file
   ```
   # INPUT FASTQ FILES
   #OTHER FORMATS FOR FQ: "_1.fastq.gz; #"_1.fastq";
   my $fqFile1, $fqFile2;
   $fqFile1 = $inDataDir.$sample."_1.fq";
   $fqFile2 = $inDataDir.$sample."_2.fq";
   ```
   
Edit the Broad, Queue, and reference directory path relative to your file system.
   ```
   # Pipeline executables and its directories
   ### ENTER THE CORRECT PATH TO THE FOLLOWING 3 VARIABLES ###
   my $broadDir = '/path/to/bioinformatic_tools_GATK_Picard_BWA';
   my $QueueBroadBestPracticesDir = '/path/to/Queue_Broad_BestPractices';
   my $homosapiensrefgenomeDir = '/path/to/homosapiensrefgenome';
   ```
   
-------------------------------------------------------------------------

#### Running the script 

From the /path/to/workflow_profiler enter the command below to run the pipeline. 

```
python3 workflow_profiler.py <pipeline script to be run> <workflow dictionary> 
<name of fastq files> <number of threads> <path to fastq files> <path for output files> -Ap
```

Example:
```
python3 workflow_profiler.py wgs_sample_calling_data_collection_gatk_best_practices_optimized.pl 
gatk_best_practices Solexa-272221 36 /home/opt/data/broadwgs/ /home/opt/data/broadout/ -Ap
```

