# CRISPRCATCH
A tool for analyzing ecDNA structure. It works with data comes from PFGE experiment. 

## Prerequisites:
This tool use some pre developed tool and must install them before using this tool. These tool are:
- The [jluebeck/PrepareAA](https://github.com/jluebeck/PrepareAA) is manadtory for PFGE and variable `$PreAA` should set to the directory you install PrepareAA. For adding this variable you can change your directory to where you install PrepareAA and run the following commands:
        
        echo export PreAA=$PWD >> ~/.bashrc
        source ~/.bashrc
- The [jluebeck/AmpliconArchitec](https://github.com/jluebeck/AmpliconArchitect) is manadtory for PFGE and variable `$AA_SRC` should set in the bashrc. You can find more information about how to set `$AA_SRC` on AA Github page.
- The [jluebeck/CycleViz](https://github.com/jluebeck/CycleViz) is manadtory for PFGE and variable `$CV_SRC` should set in the bashrc. You can find more information about how to set `$CV_SRC` on CycleViz Github page.
- [samtools](http://www.htslib.org/) (PFGE supports versions >= 1.0)
- [bwa mem](https://github.com/lh3/bwa)
- [picard](https://github.com/broadinstitute/picard). Variable `$PICARD` should set the directory picard is installed.
- [intervaltree](https://github.com/chaimleib/intervaltree)
## Installation
After installing prerequisites, please clone the PFGE repo consider running the following to add a line to your `.bashrc` file:

        cd PFGE/
        echo export PFGE=$PWD >> ~/.bashrc
        source ~/.bashrc
        
For making sure that you install all requiements correctly, please download these two fastq files: [fastq1](https://drive.google.com/file/d/1iYOMtjag3mnZdw5Bqm2cdatE_OwJXFk3/view?usp=sharing) and [fastq2](https://drive.google.com/file/d/1-Vbj6lAsQtQyeXZyT2jZi08HiPT3uUaJ/view?usp=sharing). Then run the following command, if every things installed properly, after ~30 min the pipeline should generate GBM39 amplicon structure.

`python3 wrapper.py -f1 absolute_path_to_file1 -f2 absolute_path_to_file2 -b i -sname GBM39 -o path_to_output_dir -t 1 -r hg19 -bulk test_data/GBM39_AA_graph.txt -lmax 1370 -lmin 1200 -chr chr7 -g_start 55256396 -g_end 55256397`


## Usage
PFGE takes as an input sequence data comes from PFGE experiment for a band. Sequence data in `.fastq` format, expected length of band and outputed best candidates structure for band's structure. 

### Command line arguments
- `-f1` Absolute Path to band_r1.fastq file
- `-f2` Absolute Path to band_r2.fastq file
- `-b` Analyzed band label name
- `-sname` Analyzed Cell-line name
- `-o` Output folder directory for saving results
- `-t` Number of threads
- `-r` Reference genome version. It can be hg19 or hg38.
- `-bulk` Absolute Path to the AA breakpoint graph file for bulk cell-line
- `-lmax` Maximum estimated length for structure in terms of Kbp. 
- `-lmin` Minimum estimated length for structure in terms of Kbp. 
- `-chr` Chromosome of target cite. Please write 'chr' + chromosome number.
- `-g_start` Target cite starting position in bp.
- `-g_end` Target cite ending position in bp.
- `-bed` Absolute path to bed file that spcifying amplicon region. It is not required field, PFGE can generate this from AA breakpoint graph file on bulk. 
- `-min_sup` specifying minimum number of reads for calling a breakpoint. Default value is 2. If you have high coverage data please set it to 4.
- `-sdv` specifying insert_size sdv for filtering reads for calling breakpoints. Default value is 8.5.

example command: 

`python3 wrapper.py -f1 SNU16_i_R1.fastq -f2 SNU16_i_R2.fastq -b i -sname SNU16 -o output/ -t 10 -r hg19 -bulk SNU16_AA_amplicon1_graph.txt -lmax 1810 -lmin 1660 -chr chr10 -g_start 123353331 -g_end 123353350 -sdv 8.5 -min_sup 2`

## s_wrapper.py(super wrapper)
If you want to run the pipeline for bunch of bands together, you can use 's_wrapper.py'.
### Command line arguments
- `-sname` Analyzed Cell-line name
- `-o` Output folder directory for saving results
- `-t` Number of threads
- `-r` Reference genome version. It can be hg19 or hg38.
- `-bulk` Absolute path to the AA breakpoint graph file for bulk cell-line
- `-bed` Absolute path to bed file that spcifying amplicon region. It is not required field, PFGE can generate this from AA breakpoint graph file on bulk. 
- `-min_sup` specifying minimum number of reads for calling a breakpoint. Default value is 2. If you have high coverage data please set it to 4.
- `-sdv` specifying insert_size sdv for filtering reads for calling breakpoints. Default value is 8.5.
- `-csv` specifying a csv file containing bands information. Please take a look at 'example.csv' and fill the information as needed. Please use the absolute path for column read1 and read2.
example command: 

`python3 pipeline.py -csv SNU16_i.csv -sname SNU16 -t 10 -r hg19 -o output/ -bulk SNU16_AA_amplicon1_graph.txt -sdv 8.5 -min_sup 2 -bed DNU16.bed`

