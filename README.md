# oncoprint-otp-project
a set of scripts to prepare a project organized by OTP for oncoprint plotting

## Prerequisites

- A project directory structure set up by https://github.com/naveedishaque/otp-project-softlinker
- ~~Developed using perl 5, version 26, subversion 1 (v5.26.1) built for x86_64-linux-gnu-thread-multi~~ 

## Installation and usage
```
# First download the repo
git clone https://github.com/naveedishaque/oncoprint-otp-project.git

# Enter the cirectory and setup the environment using conda
cd oncoprint-otp-project
conda env create -f environment.yaml
```

## Usage
First we activate the conda environment to have to correct software stack
```
conda activate oncoprint-otp-project
```
Then we create a table of mutations observed in the cohort. This should point to the `results_per_pid` directory created by the the [otp softlinker script].(https://github.com/naveedishaque/otp-project-softlinker). This script looks for all available calls from the SNV, indel, SV and CNV workflows run by OTP. It calculates kataejis events based on the definition of 6 somatic mutations in a 1kbp window, and annotates these to genes. Structural variations are assigned to genes if they are directly overlapping the gene (`sv_direct`), close to a gene (<100,000bp, `sv_close`), or just annotated to the nearest gene regardless of distance (`sv_near`). CNVs are annotation to genes as amplifed/deleted if the copy number is greater or less than 0.5 from the expected ploidy, high gain if the gene copy number is greater than 2.5x of the expected ploidy, or a homozygous deletion if the copy number of the gene is below 0.5.
```
perl make_oncoprint_table.pl -i [input results_per_pid directory] -o [output prefix]
```
This should create a set of files including `onco_print...csv` and `sample_info...csv`. You can now plot these as on oncoprint. The following script has many parameters, which are still unfortunately only explained within the script iteself.
```
R -f make_oncoprint_plot.R --no-save --no-restore --args --input_table <onco_print table from make_oncprint_table.pl> -sampleinfo_table <sample_info table from make_oncprint_table.pl> --min_recurrence=4 --cnas_num=6
```
## IntOGen

The script also produces the input to run IntOGen. To setup IntOGen:

```
conda create -n intogen_manual python=3.5.1  -c conda-forge
conda activate intogen_manual
conda install perl=5.20.3.1  -c conda-forge
pip install numpy scipy pandas drmaa==0.7.9
cpan App::cpanminus
cpanm Digest::MD5
cpanm DBI
cpanm threads
pip install intogen==3.0.8 oncodriveclust==1.0.0 oncodrivefm==1.0.3
conda install python=3.5.3  -c conda-forge # it turns out that 3.5.1 has an older version of the typing library, so you need >=3.5.3. I didn't have time to try using the right version of python from the start :/
intogen --setup
```
You then need to download MutSigCV and set it up to run with MCR. First setting up MCR
```
# install MCR
cd /path/to/where/to/install/
wget https://ssd.mathworks.com/supportfiles/MCR_Runtime/R2013a/MCR_R2013a_glnxa64_installer.zip
unzip MCR_R2013a_glnxa64_installer.zip
./install  -mode silent -agreeToLicense yes -outputFile myapp_log.txt -destinationFolder /path/to/where/to/install/MATLAB_Runtime

```
To install MutSigCV check out their github: https://github.com/getzlab/MutSig2CV

Then, finally to run IntOGen:
```
# instructons coming soon
```

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/your/project/tags).

 - v1.0.1: added conda environment.yaml, and modified `threshold_cnv=0.5` (from previous 0.3)
 - v1.0.0: first working version (equivalent to v0.7)

## Authors

Naveed Ishaque

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details
../otp-project-softlinker/README.md (END)                                                                                                                                                         
