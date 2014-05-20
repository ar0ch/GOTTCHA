# Genomic Origin Through Taxonomic CHAllenge (GOTTCHA)

GOTTCHA is an application of a novel, gene-independent and signature-based metagenomic
taxonomic profiling method with significantly smaller false discovery rates (FDR) that is 
laptop deployable. Our algorithm was tested and validated on twenty synthetic and mock 
datasets ranging in community composition and complexity, was applied successfully to data
generated from spiked environmental and clinical samples, and robustly demonstrates 
superior performance compared with other available tools.

-------------------------------------------------------------------
## SYSTEM REQUIREMENT

Linux (2.6 kernel or later) or Mac (OSX 10.6 Snow Leopard or later) operating system
with minimal 8 GB of RAM is recommended. Perl v5.8 or above is required. The C/C++
compiling enviroment might be required for installing dependencies. Systems may vary.
Please assure your system have essential software building package (e.g. build-essential
for Ubuntu, XCODE for Mac...etc) installed properly before running the installing 
script.

GOTTCHA was tested successfully on our Linux servers (Ubuntu 12.10 w/ Perl v5.14.2; 
Ubuntu 10.04 w/ Perl v5.10.1) and Macbook Pro laptops (MAC OSX 10.8 w/ XCODE v5.1).

-------------------------------------------------------------------
## QUICK START 

This is an example for profiling "test.fastq" using GOTTCHA with a species-level
pre-computed bacterial database. The testing FASTQ file comes along with GOTTCHA package
in the "test" directory. More details are stated in the INSTRUCTION section.

1. Obtaining GOTTCHA package:

        $ git clone https://github.com/LANL-Bioinformatics/GOTTCHA.git gottcha

2. Installing GOTTCHA:

        $ cd gottcha
        $ ./INSTALL.sh

3. Downloading lookup table and species level database from our SFTP server.
   Using '9001gottcha' as password when the request prompts:
 
        $ sftp -o "Port 33001" gottcha@img-gp.lanl.gov:/data/gottcha/GOTTCHA_lookup.tar.gz ./
        $ sftp -o "Port 33001" gottcha@img-gp.lanl.gov:/data/gottcha/GOTTCHA_BACTERIA_c3514_k24_u24.species.tar.gz ./

4. Unpacking and decompressing archives:

        $ tar -zxvf GOTTCHA_lookup.tar.gz
        $ tar -zxvf GOTTCHA_BACTERIA_c3514_k24_u24.species.tar.gz
	
5. Running gottcha.pl: 

        $ bin/gottcha.pl             \
             --threads 8             \
             --outdir ./             \
             --input test/test.fastq \
             --database database/GOTTCHA_BACTERIA_c3514_k24_u24.species

6. Enjoying the result at './test.gottcha.tsv'.

-------------------------------------------------------------------
## DETAIL INSTRUCTIONS

The detail of steps in the above section will be descrbed in this section. Note that all 
instructions in this document use pre-computed databases downloaded from our SFTP site.

If you are looking for instructions building a CUSTOM database and/or running GOTTCHA
step-by-step, please read README_FULL.md.

-------------------------------------------------------------------
### Obtaining GOTTCHA

The source codes can be downloading from [here](https://bitbucket.org/poeli/gottcha).
The pre-computed databases need to be downloaded separately from our SFTP server.
Please see below in the [Obtaining Pre-computed Databases] section.
       
You can use "git" to obtain the package:

        $ git clone https://github.com/LANL-Bioinformatics/GOTTCHA.git gottcha

or download the compressed archive in
 [zip](https://github.com/LANL-Bioinformatics/GOTTCHA/archive/master.zip).

-------------------------------------------------------------------
### Installation

The GOTTCHA profiling and database generating scripts are primarily Perl-based, and require at
least Perl v5.8 with dependencies installed properly (listed in README_FULL.md).
The splitrim tool is written in [D](http://www.dlang.org) that requires an appropriate D 
compiler to complie it. GOTTCHA utilizes [BWA](https://github.com/lh3/bwa) with BWA-MEM algorithm
for read mapping. You can either keep "dmd" and "bwa" in your system path or simply 
run the installing script - INSTALL.sh. This script will check and try to install missing 
tools and dependencies:

    	$ ./INSTALL.sh

After running INSTALL.sh successfully, the binaries and related scripts will be stored
in ./bin directory.

-------------------------------------------------------------------
### Obtaining Pre-computed Databases

Databases of unique genome segments at multiple taxonomic levels (e.g. family, species, 
genus, strain-level, etc.) are used for taxonomic classification of reads. Variants of 
these databases, in which all human 24-mers were removed were also generated and used in 
this study. These 24-mers were derived from the GRCh37.p10 (Genome Reference Consortium),
HuRef (J. Craig Venter Institute), and CHM1_1.0 (Washington U. School of Medicine) 
assemblies and include unplaced scaffolds. For example, 
GOTTCHA_BACTERIA_c3514_k24_u24_xHUMAN3x.species.tar.gz is a GOTTCHA bacterial 
signature database that was produced by eliminating shared 24-mer (k24) sequences at 
species level and 3 human genomes (xHuman3X) from 3514 bacterial contigs (c3514; replicons
including chromosomes and plasmids) and retained minimum 24bp of unique fragments (u24).

The compressed database archives are available for users to download at our SFTP 
server with the credential below:
 
 > SFTP server: img-gp.lanl.gov   
 > Port: 33001   
 > username: gottcha   
 > password: 9001gottcha   
 
GOTTCHA requires a taxanomic lookup table (GOTTCHA_lookup.tar.gz) and a pre-computed
database (e.g: GOTTCHA_BACTERIA_c3514_k24_u24_xHUMAN3x.species.tar.gz) to classify reads.
These signature databases could be huge. We highly recommend users also download 
corresponding *.md5 file for verification.

You can use the 'sftp' command to download both archives, one at a time:

        $ sftp -o "Port 33001" gottcha@img-gp.lanl.gov:/data/gottcha/GOTTCHA_lookup.tar.gz ./
        $ sftp -o "Port 33001" gottcha@img-gp.lanl.gov:/data/gottcha/GOTTCHA_BACTERIA_c3514_k24_u24_xHUMAN3x.species.tar.gz ./

Then use 'tar' to unpack and decompress both archives:

        $ tar -zxvf GOTTCHA_lookup.tar.gz
        $ tar -zxvf GOTTCHA_BACTERIA_c3514_k24_u24_xHUMAN3x.species.tar.gz

Files will be expanded to ./database directory by default.

Here is a list of the available pre-computed databases. Note that these databases are
also available in FASTA format at /data/gottcha/FASTA/:

  * GOTTCHA_BACTERIA_c3514_k24_u24.class.tar.gz (4.6GB)
  * GOTTCHA_BACTERIA_c3514_k24_u24.family.tar.gz (4.6GB)
  * GOTTCHA_BACTERIA_c3514_k24_u24.genus.tar.gz (4.5GB)
  * GOTTCHA_BACTERIA_c3514_k24_u24.order.tar.gz (4.6GB)
  * GOTTCHA_BACTERIA_c3514_k24_u24.phylum.tar.gz (4.6GB)
  * GOTTCHA_BACTERIA_c3514_k24_u24.species.tar.gz (4.3GB)
  * GOTTCHA_BACTERIA_c3514_k24_u24.strain.tar.gz (3.9GB)
  * GOTTCHA_BACTERIA_c3514_k24_u24_xHUMAN3x.genus.tar.gz (4.5GB)
  * GOTTCHA_BACTERIA_c3514_k24_u24_xHUMAN3x.species.tar.gz (4.3GB)
  * GOTTCHA_BACTERIA_c3514_k24_u24_xHUMAN3x.strain.tar.gz (3.8GB)
  * GOTTCHA_VIRUSES_c3498_k85_u24.genus.tar.gz (71MB)
  * GOTTCHA_VIRUSES_c3498_k85_u24.species.tar.gz (68MB)
  * GOTTCHA_VIRUSES_c3498_k85_u24.strain.tar.gz (68MB)
  * GOTTCHA_VIRUSES_c3498_k85_u24_xHUMAN3x.genus.tar.gz (71MB)
  * GOTTCHA_VIRUSES_c3498_k85_u24_xHUMAN3x.species.tar.gz (68MB)
  * GOTTCHA_VIRUSES_c3498_k85_u24_xHUMAN3x.strain.tar.gz (68MB)

-------------------------------------------------------------------
### Running GOTTCHA

The procedure includes 3 major steps: split-trimming the input data, mapping reads 
to a GOTTCHA database using BWA and profiling/filtering the result. These steps have
been wrapped into a sigle script 'gottcha.pl'. User will need to provide a FASTQ file
as input and specify the location and name of the database.

Here is the general usage to run GOTTCHA:

 > $ bin/gottcha.pl -i <FASTQ> -d <PATH/DATABASE_PREFIX>

We provided a testing FASTQ file and example output in ./test. The following command
is an example to run "test.fastq" through GOTTCHA using a species level database with
8 threads:

        $ bin/gottcha.pl             \
             --threads 8             \
             --mode all              \
             --input test/test.fastq \
             --database database/GOTTCHA_BACTERIA_c3514_k24_u24_xHUMAN3x.species

In this case, we specify the output mode to "all" using "--mode all" option that
gives us two output files and all intermediate ouptuts stored in "test_temp"
directory. Both outputs are plain text files in tab-separated values format:
a summary table "test.gottcha.tsv" and a full information table 
"test.gottcha_full.tsv".

-------------------------------------------------------------------
### Interpreting Results

GOTTCHA reports profiling results in a neat summary table (*.gottcha.tsv) by default.
The tsv file will list the organism(s) at all taxonomic levels from STRAIN to PHYLUM,
their linear length, total bases mapped, linear depth of coverage, and the normalized 
linear depth of coverage. The linear depth of coverage (LINEAR_DOC) is used to calculate 
relative abundance of each organism or taxonomic name in the sample.

Summary table:

> Column          | Description
> --------------- | -------------------------------------------------------------------
> LEVEL           | taxonomic rank
> NAME            | taxonomic name
> REL_ABUNDANCE   | relative abundance (equivalent to NORM_COV by default)
> LINEAR_LENGTH   | number of non-overlapping bases covering the signatures
> TOTAL_BP_MAPPED | sum total of all hit lengths recruited to signatures
> HIT_COUNT       | number of hits recruited to signatures
> LINEAR_DOC      | linear depth-of-coverage (TOTAL_BP_MAPPED / LINEAR_LENGTH)
> NORM_COV        | normalized linear depth-of-coverage (LINEAR_DOC / SUM(LINEAR_DOC in certain level))

There are two report modes available. Other than a summary table, "full" report 
mode will report a table with more detail information from unfiltered results. The
"all" report mode will keep all output files that were generated by each profiling
step.

-------------------------------------------------------------------
### Visulizing Results using Krona

[Krona](http://sourceforge.net/p/krona/home/krona/) is an interactive browser that allows 
to explore hierarchical data with pie charts. Assumed you have Krona installed properly,
we are going to create a Krona chart from a text file listing abundance and lineages. 
You will find <PREFIX>.lineage.tsv file when you run gottcha.pl in "all" output mode.

Use 'ktImportText' and save the chart to "test.krona.html":

        $ ktImportText test_temp/test.lineage.tsv -o test.krona.html

------------------------------------------------------------------
## CITATION

Tracey Allen K. Freitas, Po-E Li, Matthew B. Scholz, Patrick S. G. Chain: Accurate Metagenome 
characterization using a hierarchical suite of unique signatures. (in submission)

-------------------------------------------------------------------
## AUTHORS

Tracey Allen K. Freitas, Po-E Li, Matthew B. Scholz, Patrick S. G. Chain
Bioscience Division, Los Alamos National Laboratory, Los Alamos, NM 87545

-------------------------------------------------------------------
## ACKNOWLEDGEMENTS

We would like to thank Jason Gans for critical discussions on classification and machine 
learning techniques, and Shihai Feng for the generation of synthetic datasets.
