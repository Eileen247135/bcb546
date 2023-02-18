#UNIX Assignment

##Data Inspection

###Attributes of `fang_et_al_genotypes`


By inspecting this file I learned that:

* point 1 The size of `fang_et_al_genotypes.txt` is 11M.

```
$ du -h fang_et_al_genotypes.txt
```
* point 2 The number of column is 986.

```
$ awk -F "\t" '{print NF; exit}' fang_et_al_genotypes.txt
```
* point 3 The number of lines is 2783.

```
$ wc -l fang_et_al_genotypes.txt
```

* point 4 The number of words is 2744038.

```
wc -w fang_et_al_genotypes.txt
```
* point 5 The number of characters is 11051939.

```
wc -m fang_et_al_genotypes.txt
```


###Attributes of `snp_position.txt`



* point 1 The size of `snp_position.txt` is 84K.

```
$ du -h snp_position.txt
```
* point 2 The number of columns is 15.

```
$ awk -F "\t" '{print NF; exit}' snp_position.txt
```
* point 3 The number of lines is 984.

```
$ wc -l snp_position.txt
```

* point 4 The number of words is 13198.

```
$ wc -w snp_position.txt
```
* point 5 The number of characters is 82763.

```
$ wc -m snp_position.txt
```

##Data Processing

###Maize Data



####Prepare maize files
* Extract ZMMIL, ZMMLR and ZMMMR from Group column from fang_et_al_genotypes.txt
```
$ awk '$3 ~ /Group|ZMMIL|ZMMLR|ZMMMR/' fang_et_al_genotypes.txt > fang_maize.txt
```
* Then, transpose this file.
```
$ awk -f transpose.awk fang_maize.txt > fang_maize_trans.txt
```
* Sort the transposed file first column and add its Sample_ID header on the first row.
```
$ head -n 1 fang_maize_trans.txt > fang_maize_trans_header1.txt
```
```
$ tail -n +4 fang_maize_trans.txt | sort -k1,1 > fang_maize_trans_sort.txt
```
```
$ cat fang_maize_trans_header1.txt fang_maize_trans_sort.txt > fang_maize_trans_w_header1.txt 
```
* Sort snp_position.txt first column and add its SNP_ID header on the first column. Then, extract the column of SNP_ID (1st columm), Chromosome (3rd column) and Position (4th column) using cut command.
```
$ head -n 1 snp_position.txt > snp_header.txt
```
```
$ tail -n +2 snp_position.txt | sort -k1,1 > snp_sort.txt
```
```
$ cat snp_header.txt snp_sort.txt > snp_w_header.txt
```
```
$ cut -f 1,3,4 snp_w_header.txt > snp_w_header_s.txt
```
* Join sorted snp_position.txt file and sorted transposed maize fang_et_al_genotypes.txt file. Firstly, the header of Sample_ID in transposed maize fang_et_al_genotypes.txt file should be changed to SNP_ID, which is the same as the sorted snp_position.txt file.
```
$ sed 's/Sample_ID/SNP_ID/' fang_maize_trans_w_header1.txt > fang_maize_trans_w_header1_c.txt
```
```
$ wc -l snp_w_header_s.txt
   #count the number of rows in snp_w_header_s.txt
 ``` 
 ``` 
 $ wc -l fang_maize_trans_w_header1_c.txt  #count the number of rows in fang_maize_trans_w_header1_c.txt
 ``` 
 ```
 $ join -1 1 -2 1 -t $'\t' snp_w_header_s.txt fang_maize_trans_w_header1_c.txt > maize_join_header1_c.txt
 ```


#### Question 3
* Extract rows containing 'unknown' and 'Chromosome' using grep command in the joined file maize_join_header1_c.txt
```
$ grep -E "(Chromosome|unknown)" maize_join_header1_c.txt > maize_snp_unknown.txt
```

#### Question 4
* Extract rows containing 'multiple' and 'Chromosome' column using grep command in the joined file maize_join_header1_c.txt
```
$ grep -E "(Chromosome|multiple)" maize_join_header1_c.txt > maize_snp_multiple.txt
```

#### Question 1
* Question 1 preparation: Sort the 3rd column 'Position' based on increasing numerical position values and add the header on the 1st row. Then, replace the missing data '?/?' with '?' using sed command.
```
$ head -n 1 maize_join_header1_c.txt > maize_join_header1_c_header1.txt
```
```
$ tail -n +2 maize_join_header1_c.txt | sort -k3,3n >maize_join_header1_c_content.txt
```
```
$ cat maize_join_header1_c_header1.txt maize_join_header1_c_content.txt | sed 's!?/?!?!g' > maize_join_header1_c_sort.txt
```
* Use awk to extract each chromosome from the 2nd column 'Chromosome'
```
$ awk '$2 ~ /Chromosome|1/ && $2 !~ /10/' maize_join_header1_c_sort.txt > maize_join_header1_c_sort_chr1.txt 
```
```
$ awk '$2 ~ /Chromosome|2/' maize_join_header1_c_sort.txt > maize_join_header1_c_sort_chr2.txt
```
```
$ awk '$2 ~ /Chromosome|3/' maize_join_header1_c_sort.txt > maize_join_header1_c_sort_chr3.txt
```
```
$ awk '$2 ~ /Chromosome|4/' maize_join_header1_c_sort.txt > maize_join_header1_c_sort_chr4.txt
```
```
$ awk '$2 ~ /Chromosome|5/' maize_join_header1_c_sort.txt > maize_join_header1_c_sort_chr5.txt
```
```
$ awk '$2 ~ /Chromosome|6/' maize_join_header1_c_sort.txt > maize_join_header1_c_sort_chr6.txt
```
```
$ awk '$2 ~ /Chromosome|7/' maize_join_header1_c_sort.txt > maize_join_header1_c_sort_chr7.txt
```
```
$ awk '$2 ~ /Chromosome|8/' maize_join_header1_c_sort.txt > maize_join_header1_c_sort_chr8.txt
```
```
$ awk '$2 ~ /Chromosome|9/' maize_join_header1_c_sort.txt > maize_join_header1_c_sort_chr9.txt
```
```
$ awk '$2 ~ /Chromosome|10/' maize_join_header1_c_sort.txt > maize_join_header1_c_sort_chr10.txt
```

#### Question 2
* Question 2 preparation: Sort the 3rd column 'Position' based on decreasing numerical position values and add the header on the 1st row. Then, replace the missing data '?/?' with '-' using sed command.
```
$ tail -n +2 maize_join_header1_c.txt | sort -k3,3nr >maize_join_header1_c_content2.txt
```
```
$ cat maize_join_header1_c_header1.txt maize_join_header1_c_content2.txt | sed 's!?/?!-!g' > maize_join_header1_c_sort2.txt
```
* Use awk to extract each chromosome from the 2nd column 'Chromosome'
```
$ awk '$2 ~ /Chromosome|1/ && $2 !~ /10/' maize_join_header1_c_sort2.txt > maize_join_header1_c_sort2_chr1.txt
```
```
$ awk '$2 ~ /Chromosome|2/' maize_join_header1_c_sort2.txt > maize_join_header1_c_sort2_chr2.txt
```
```
$ awk '$2 ~ /Chromosome|3/' maize_join_header1_c_sort2.txt > maize_join_header1_c_sort2_chr3.txt
```
```
$ awk '$2 ~ /Chromosome|4/' maize_join_header1_c_sort2.txt > maize_join_header1_c_sort2_chr4.txt
```
```
$ awk '$2 ~ /Chromosome|5/' maize_join_header1_c_sort2.txt > maize_join_header1_c_sort2_chr5.txt
```
```
$ awk '$2 ~ /Chromosome|6/' maize_join_header1_c_sort2.txt > maize_join_header1_c_sort2_chr6.txt
```
```
$ awk '$2 ~ /Chromosome|7/' maize_join_header1_c_sort2.txt > maize_join_header1_c_sort2_chr7.txt
```
```
$ awk '$2 ~ /Chromosome|8/' maize_join_header1_c_sort2.txt > maize_join_header1_c_sort2_chr8.txt
```
```
$ awk '$2 ~ /Chromosome|9/' maize_join_header1_c_sort2.txt > maize_join_header1_c_sort2_chr9.txt
```
```
$ awk '$2 ~ /Chromosome|10/' maize_join_header1_c_sort2.txt > maize_join_header1_c_sort2_chr10.txt
```


###Teosinte Data



#### Prepare teosinte files
* Extract ZMPBA,ZMPIL and ZMPJA from Group column from fang_et_al_genotypes.txt
```
$ awk '$3 ~ /Group|ZMPBA|ZMPIL|ZMPJA/' fang_et_al_genotypes.txt > fang_teosinte.txt
```
* Then, transpose this file.
```
$ awk -f transpose.awk fang_teosinte.txt > fang_teosinte_trans.txt
```
* Sort the transposed file first column and add its Sample_ID header on the first row.
```
$ head -n 1 fang_teosinte_trans.txt > fang_teosinte_trans_header1.txt
```
```
$ tail -n +4 fang_teosinte_trans.txt | sort -k1,1 > fang_teosinte_trans_sort.txt
```
```
$ cat fang_teosinte_trans_header1.txt fang_teosinte_trans_sort.txt > fang_teosinte_trans_w_header1.txt
```
* Use the above files created (snp_w_header_s.txt) to do next step, which sorted and extracted the column of SNP_ID (1st columm), Chromosome (3rd column) and Position (4th column) using cut command in snp_position.txt file.

* Join sorted snp_position.txt file and sorted transposed teosinte fang_et_al_genotypes.txt file. Firstly, the header of Sample_ID in transposed teosinte fang_et_al_genotypes.txt file should be changed to SNP_ID using sed command, which is the same as the sorted snpposition.txt file.

```
$ sed 's/Sample_ID/SNP_ID/' fang_teosinte_trans_w_header1.txt > fang_teosinte_trans_w_header1_c.txt
```
```
$ wc -l snp_w_header_s.txt 
```
```
$ wc -l fang_teosinte_trans_w_header1_c.txt 
```
```
$ join -1 1 -2 1 -t $'\t' snp_w_header_s.txt fang_teosinte_trans_w_header1_c.txt > teosinte_join_header1_c.txt
```

#### Question 3
* Extract rows containing 'unknown' and 'Chromosome' columns using grep command in the joined file teosinte_join_header1_c.txt

```
$ grep -E "(Chromosome|unknown)" teosinte_join_header1_c.txt > teosinte_snp_unknown.txt
```


#### Question 4
* Extract rows containing 'multiple' and 'Chromosome' columns using grep command in the joined file teosinte_join_header1_c.txt

```
$ grep -E "(Chromosome|multiple)" teosinte_join_header1_c.txt > teosinte_snp_multiple.txt
```

#### Question 1
* Question 1 preparation: Sort the 3rd column 'Position' based on increasing numerical position values and add the header on the 1st row. Then, replace the missing data '?/?' with '?' using sed command.

```
$ head -n 1 teosinte_join_header1_c.txt > teosinte_join_header1_c_header1.txt
```
```
$ tail -n +2 teosinte_join_header1_c.txt | sort -k3,3n >teosinte_join_header1_c_content.txt
```
```
$ cat teosinte_join_header1_c_header1.txt teosinte_join_header1_c_content.txt | sed 's!?/?!?!g' > teosinte_join_header1_c_sort.txt
```
* Use awk to extract each chromosome from the 2nd column 'Chromosome'

```
$ awk '$2 ~ /Chromosome|1/ && $2 !~ /10/' teosinte_join_header1_c_sort.txt > teosinte_join_header1_c_sort_chr1.txt
```
```
$ awk '$2 ~ /Chromosome|2/' teosinte_join_header1_c_sort.txt > teosinte_join_header1_c_sort_chr2.txt
```
```
$ awk '$2 ~ /Chromosome|3/' teosinte_join_header1_c_sort.txt > teosinte_join_header1_c_sort_chr3.txt
```
```
$ awk '$2 ~ /Chromosome|4/' teosinte_join_header1_c_sort.txt > teosinte_join_header1_c_sort_chr4.txt
```
```
$ awk '$2 ~ /Chromosome|5/' teosinte_join_header1_c_sort.txt > teosinte_join_header1_c_sort_chr5.txt
```
```
$ awk '$2 ~ /Chromosome|6/' teosinte_join_header1_c_sort.txt > teosinte_join_header1_c_sort_chr6.txt
```
```
$ awk '$2 ~ /Chromosome|7/' teosinte_join_header1_c_sort.txt > teosinte_join_header1_c_sort_chr7.txt
```
```
$ awk '$2 ~ /Chromosome|8/' teosinte_join_header1_c_sort.txt > teosinte_join_header1_c_sort_chr8.txt
```
```
$ awk '$2 ~ /Chromosome|9/' teosinte_join_header1_c_sort.txt > teosinte_join_header1_c_sort_chr9.txt
```
```
$ awk '$2 ~ /Chromosome|10/' teosinte_join_header1_c_sort.txt > teosinte_join_header1_c_sort_chr10.txt
```

#### Question 2
* Question 2 preparation: Sort the 3rd column 'Position' based on decreasing numerical position values and add the header on the 1st row. Then, replace the missing data '?/?' with '-' using sed command.

```
$ tail -n +2 teosinte_join_header1_c.txt | sort -k3,3nr >teosinte_join_header1_c_content2.txt
```
```
$ cat teosinte_join_header1_c_header1.txt teosinte_join_header1_c_content2.txt | sed 's!?/?!-!g' > teosinte_join_header1_c_sort2.txt
```
* Use awk to extract each chromosome from the 2nd column 'Chromosome'

```
$ awk '$2 ~ /Chromosome|1/ && $2 !~ /10/' teosinte_join_header1_c_sort2.txt > teosinte_join_header1_c_sort2_chr1.txt
```
```
$ awk '$2 ~ /Chromosome|2/' teosinte_join_header1_c_sort2.txt > teosinte_join_header1_c_sort2_chr2.txt
```
```
$ awk '$2 ~ /Chromosome|3/' teosinte_join_header1_c_sort2.txt > teosinte_join_header1_c_sort2_chr3.txt
```
```
$ awk '$2 ~ /Chromosome|4/' teosinte_join_header1_c_sort2.txt > teosinte_join_header1_c_sort2_chr4.txt
```
```
$ awk '$2 ~ /Chromosome|5/' teosinte_join_header1_c_sort2.txt > teosinte_join_header1_c_sort2_chr5.txt
```
```
$ awk '$2 ~ /Chromosome|6/' teosinte_join_header1_c_sort2.txt > teosinte_join_header1_c_sort2_chr6.txt
```
```
$ awk '$2 ~ /Chromosome|7/' teosinte_join_header1_c_sort2.txt > teosinte_join_header1_c_sort2_chr7.txt
```
```
$ awk '$2 ~ /Chromosome|8/' teosinte_join_header1_c_sort2.txt > teosinte_join_header1_c_sort2_chr8.txt
```
```
$ awk '$2 ~ /Chromosome|9/' teosinte_join_header1_c_sort2.txt > teosinte_join_header1_c_sort2_chr9.txt
```
```
$ awk '$2 ~ /Chromosome|10/' teosinte_join_header1_c_sort2.txt > teosinte_join_header1_c_sort2_chr10.txt
```