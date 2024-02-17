# UNIX Assignment

## Data Inspection

### Attributes of fang_et_al_genotypes
here is my snippet of code used for data inspection:

Inspecting Data with cat

$ cat fang_et_al_genotypes

Inspecting Data with head

$ head cat fang_et_al_genotypes

Inspecting Data with tail

$ tail fang_et_al_genotypes

Counting the number 

$ wc fang_et_al_genotypes

 2783  2744038 11051939 fang_et_al_genotypes.txt

Inspecting the file size

$ du -h fang_et_al_genotypes.txt

6.7M    fang_et_al_genotypes.txt

Inspecting the number of columns in a file with awk

$  awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt

By inspecting this file I learned that:
- The number of lines with comand wc is 2783, and the number of words is 2744038. 
- The size of the file with command du -h is 6.7M
- The number of columns with awk command is 986

### Attributes of snp_position.txt
here is my snippet of code used for data inspection:

Inspecting Data with cat

$ cat snp_position.txt

Inspecting Data with head

$ head -n 3 cat snp_position.txt

Inspecting Data with tail

$ tail -n 3 snp_position.txt

Counting the number 

$ $ wc snp_position.txt

 984 13198 82763 snp_position.txt

Inspecting the file size

$ du -h snp_position.txt

49K     snp_position.txt

Inspecting the number of columns in a file with awk

$ awk -F "\t" '{print NF; exit}' snp_position.txt

15

By inspecting this file I learned that:
- The number of lines with comand wc is 984, and the number of words is 13198.
- The size of the file with command du -h is 49k
- The number of columns with awk command is 15

## Data Processing

### Maize Data

### Organize Data

#### here is my snippet of code used for data processing
#### There is an explanation below each code (starts with $) with my brief description of what this code does 

$ cut -f3 fang_et_al_genotypes.txt | sort | uniq | wc -l

Extract, sort data, and count unique groups
In this case, there are 17 distinct groups in the third column of the file fang_et_al_genotypes.txt

$grep -E "(ZMMIL|ZMMLR|ZMMMR)" fang_et_al_genotypes.txt > maize_genotypes.txt

The code search patterns as Extended Regular Expressions (ERE) containing "ZMMIL", "ZMMLR", or "ZMMMR" and saves them to "maize_genotypes.txt"

$grep -E "(ZMPBA|ZMPIL|ZMPJA)" fang_et_al_genotypes.txt > teosinte_genotypes.txt

The code search patterns as Extended Regular Expressions (ERE) containing "ZMPBA", "ZMPIL", or "ZMPJA" and saves them to "maize_genotypes.txt"

$head -n 1 fang_et_al_genotypes.txt > head.txt

This code extracts the first line and saves it to a new file named "head.txt".

#### For maize data:
$cat maize_genotypes.txt >> head.txt

This code add the contents of the file "maize_genotypes.txt" to the end of the file "head.txt".

$mv head.txt maize_genotypes.txt

This code renames the file "head.txt" to "maize_genotypes.txt".

#### For teosinte data:

$ cat teosinte_genotypes.txt >> head.txt

This code add the contents of the file "teosinnte_genotypes.txt" to the end of the file "head.txt".

$mv head.txt teosinte_genotypes.txt

This code renames the file "head.txt" to "teosinte_genotypes.txt".

### Extract Data
$cut -f 3-986 maize_genotypes.txt > maize_genotypes_extract.txt

This command extracts columns 3 to 986 and saves them to a new file called "maize_genotypes_extract.txt".

$ cut -f 3-986 teosinte_genotypes.txt > teosinte_genotype_extract.txt

This command extracts columns 3 to 986 and saves them to a new file called "teosinte_genotypes_extract.txt".

$cut -f 1,3,4 snp_position.txt > snp_position_extract.txt

This command extracts the first, third, and fourth columns and saves them to a new file called "snp_position_extract.txt".

### Transpose Data
$awk -f transpose.awk maize_genotypes_extract.txt>transposed_maize.txt

This code uses awk to transpose to a new data called "transposed_maize.txt"

$awk -f transpose.awk teosinte_genotype_extract.txt > transposed_teosinte.txt

This code uses awk to transpose to a new data called "transposed_tesosinte.txt"

### Sort Data

$sort -k1,1 snp_position_extract.txt >sorted_snp.txt

This code sort the snp_position_extract data based on the first column

$sort -k1,1 transposed_maize.txt>sorted_maize.txt

This code sort the transposed_maize data based on the first column

$sort -k1,1 transposed_teosinte.txt>sorted_teosinte.txt

This code sort the transposed_teosinte data based on the first column

### Join Data

$join -1 1 -2 1 -t $'\t' sorted_snp.txt sorted_maize.txt > joined_maize.txt

Joined the sorted snp and sorted maize data to a new data called "joined_maize.txt"

$join -1 1 -2 1 -t $'\t' sorted_snp.txt sorted_teosinte.txt > joined_teosinte.txt

Joined the sorted snp and sorted teosinte data to a new data called "joined_teosinte.txt"

### To organize the Final Data

$ grep -v "multiple" joined_maize.txt |grep -v "unknown"| sort -k3,3n > increasing_maize.txt

This code filters out lines containing "multiple" or "unknown" from "joined_maize.txt", sorts (increasing) the remaining lines based on the third column (position)

$ grep -v "multiple" joined_maize.txt |grep -v "unknown"| sort -k3,3nr > decreasing_maize.txt

This code filters out lines containing "multiple" or "unknown" from "joined_maize.txt", sorts (decreasing) the remaining lines based on the third column (position)

$ grep -v "multiple" joined_teosinte.txt |grep -v "unknown"| sort -k3,3n > increasing_teosinte.txt

This code filters out lines containing "multiple" or "unknown" from "joined_teosinte.txt", sorts (increasing) the remaining lines based on the third column (position)

$ grep -v "multiple" joined_teosinte.txt |grep -v "unknown"| sort -k3,3nr > decreasing_teosinte.txt

This code filters out lines containing "multiple" or "unknown" from "joined_teosinte.txt", sorts (increasing) the remaining lines based on the third column (position)

### Maize Final Data: files

- 10 files (1 for each chromosome) with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ?

$for i in {1..10}; do awk '$2=='$i'' increasing_maize.txt|sort -k3,3n > maize/maize_chr"$i"_increasing_maize_final.txt; done

This code iterates over chromosome numbers 1 through 10, filters SNPs with the current chromosome number, and sorts them based on the third column. Thus, this code is to order SNPS based on increasing position values and with missing data encoded by this symbol: ?, and for each chromosome.

- 10 files (1 for each chromosome) with SNPs ordered based on decreasing position values and with missing data encoded by this symbol: -

$sed 's/?/-/g' decreasing_maize.txt> decreasing_maize_missingdata.txt

This code replaces all occurrences of '?' with '-' in the file "decreasing_maize.txt" and saves the modified content into a new file named "decreasing_maize_missingdata.txt". Then, continue wit the next command below to produce the decreasing file.

$for i in {1..10}; do awk '$2=='$i'' decreasing_maize_missingdata.txt > maize/maize_chr"$i"_decreasing_maize_final.txt; done

This code is to order SNPS based on decreasing position values and with missing data encoded by this symbol: "-", and for each chromosome.

- 1 file with all SNPs with unknown positions in the genome (these need not be ordered in any particular way)

$grep "unknown" joined_maize.txt>maize/maize_unknown.txt

This code searches for lines containing the word "unknown".

- 1 file with all SNPs with multiple positions in the genome (these need not be ordered in any particular way)

$grep "multiple" joined_maize.txt>maize/maize_multiple.txt

This code searches for lines containing the word "multiple".

### Teosinte Final Data: files

- 10 files (1 for each chromosome) with SNPs ordered based on increasing position values and with missing data encoded by this symbol: ?

$for i in {1..10}; do awk '$2=='$i'' increasing_teosinte.txt|sort -k3,3n > teosinte/teosinte_chr"$i"_increasing_teosinte_final.txt; done

This code iterates over chromosome numbers 1 through 10, filters SNPs with the current chromosome number, and sorts them based on the third column. Thus, this code is to order SNPS based on increasing position values and with missing data encoded by this symbol: ?, and for each chromosome.

- 10 files (1 for each chromosome) with SNPs ordered based on decreasing position values and with missing data encoded by this symbol: -

$sed 's/?/-/g' decreasing_teosinte.txt> decreasing_teosinte_missingdata.txt

This code replaces all occurrences of '?' with '-' in the file "decreasing_teosinte.txt" and saves the modified content into a new file named "decreasing_tesosinte_missingdata.txt".
Then, continue wit the next command below to produce the decreasing file.

$for i in {1..10}; do awk '$2=='$i'' decreasing_teosinte_missingdata.txt > teosinte/teosinte_chr"$i"_decreasing_teosinte_final.txt; done

This code is to order SNPS based on decreasing position values and with missing data encoded by this symbol: "-", and for each chromosome.

- 1 file with all SNPs with unknown positions in the genome (these need not be ordered in any particular way)

$grep "unknown" joined_teosinte.txt>teosinte/teosinte_unknown.txt

This code searches for lines containing the word "unknown".

- 1 file with all SNPs with multiple positions in the genome (these need not be ordered in any particular way)

$grep "multiple" joined_teosinte.txt>teosinte/teosinte_multiple.txt

This code searches for lines containing the word "multiple".