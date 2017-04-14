|Info/Organism   | A.thaliana    |C.elegans     |D.melanogaster     |
|:---------|:---:|:---:|:---:|
|Size of the file|141189371|117223923 |204986307|
|Number of chromosomes|7|7|15|
|Size of the genome in bp|119667750 |100286401|168736537|
|Number of protein-coding genes|35386|26769|30307|
|Average protein length|409|445|659|

Copying the protein sequence file to at1.fa
*cp TAIR10_pep_20101214_updated > at1.fa*
*cp TAIR10_pep_20101214_updated at1.fa*

### 1k.water stats
  __Average: 31.8395__
  *~/Lab1 (master) $ grep Score 1k.water | awk '{sum+=$3} END {print sum/NR}'*

  __Total values: 16826__
  *~/Lab1 (master) $ grep Score 1k.water | wc -l* OR
  *~/Lab1 (master) $ grep Score 1k.water | cut -d " " -f 3 | wc -l ce1.shuffle.1000.fa*
  16826 ce1.shuffle.1000.fa


  __Median: 31__
  *~/Lab1 (master) $ grep Score 1k.water | cut -d " " -f 3 | sort -n | tail -8413 | head -2*
      31.0
      31.0
*~/Lab1 (master) $ grep Score 1k.water | cut -d " " -f 3 | sort -n | head -8413 | tail -2*
    31.0
    31.0

__Min: 19.0__
__Max: 67.0__
Use the command sort (default is least to greatest, so I saw the greatest value on the bottom. To find the smallest I imputed the command "head")
*~/Lab1 (master) $ grep Score 1k.water | cut -d " " -f 3 | sort -n| head*

__Text Histogram__
*~/Lab1 (master) $ grep Score 1k.water | cut -d " " -f 3 | sort -n| uniq -c*
      3 19.0
     17 20.0
     55 21.0
    111 22.0
    215 23.0
    402 24.0
    611 25.0
    844 26.0
    979 27.0
   1165 28.0
   1378 29.0
   1392 30.0
   1457 31.0
   1431 32.0
   1225 33.0
   1095 34.0
    925 35.0
    805 36.0
    635 37.0
    482 38.0
    367 39.0
    301 40.0
    229 41.0
    162 42.0
    131 43.0
     90 44.0
     90 45.0
     51 46.0
     54 47.0
     31 48.0
     26 49.0
     18 50.0
     13 51.0
     10 52.0
      6 53.0
      7 54.0
      2 55.0
      3 56.0
      1 57.0
      3 58.0
      1 59.0
      1 60.0
      1 61.0
      1 67.0
The number of the left is the amount and the number on the right is the value

### Questions

1. Is the shape of the curve normal? Do you expect it to be normal?
With such a high N (sample size) at random shuffling, I would expect the curve to be normal (bell shaped). However, the composition is already pre-determined by the protein sequence given by C.elegans, so the starting composition is not completely random. From the statistics, it probably looks like the graph is skewed right.

2. Do you expect all protein comparisons to have the same distribution?
 No.

3. How would protein composition and length affect the scores?
Some amino acid changes are more common than others so the more common and "acceptable" changes have lower penalties. For example, a switch between Alanine and Valine has a higher score than changing Tryptophan to anything else. Longer proteins are usually more difficult to match so complete matches for long sequences have higher total scores.

4. How would the scoring matrix and gap penalites affect the scores?
Gaps reduce the score and the longer the gap, the lower the score because of accumulating penalties.

5. How might real sequences be different from random?
The order matters in protein synthesis, and certain amino acids are more likely to appear alongside other amino acids in real sequences.

### Alignment Time
*~/Lab1 (master) $ time water ce1_B0213.10.fa at1.fa -gapopen 10 -gapextend 5 -outfile ce1_B0213.10_at1.water*

Smith-Waterman local alignment of sequences  between protein of ce1 and at1
86.46user
0.20system
1:26.69elapsed 99%CPU (0avgtext+0avgdata 34708maxresident)k
39272inputs+35400outputs (0major+5890minor)pagefaults 0swaps

*~/Lab1 (master) $ time water ce1_B0213.10.fa dm1.fa -gapopen 10 -gapextend 5 -outfile ce1_B0213.10_dm1.water*

Smith-Waterman local alignment of sequences  between protein of ce1 and dm1

136.81user
0.17system
2:17.07elapsed 99%CPU (0avgtext+0avgdata 102380maxresident)k
0inputs+30136outputs (0major+18183minor)pagefaults 0swaps



*~/Lab1 (master) $ grep -v ">" /home/ubuntu/Lab1/at1.fa | wc -m*
14681363
*~/Lab1 (master) $ grep -v ">" /home/ubuntu/Lab1/at1.fa | wc -l*
198508
Subtract the numbers: 14,482,855 AA in at1.fa

*~/Lab1 (master) $ grep -v ">" /home/ubuntu/Lab1/ce1_B0213.10.fa | wc -m*
508
*~/Lab1 (master) $ grep -v ">" /home/ubuntu/Lab1/ce1_B0213.10.fa | wc -l*
9
Subtract: 499 AA in ce1_B0213.10.fa

Add both totals: 499+ 14482855= 14,483,354
Divide by time computer took: (.2 seconds)
  = 72,416,770 per second

*~/Lab1 (master) $ grep -v ">" /home/ubuntu/Lab1/dm1.fa | wc -m*
20395691
*~/Lab1 (master) $ grep -v ">" /home/ubuntu/Lab1/dm1.fa | wc -l*
414578
Subtract: 19,981,113 in dm.fa

1. How many amino acids can I align per second?
72,416,770 per second
2. How many amino acids do I need to align to do this experiment?
334,464,467
3. How long would it take to compare two proteomes?
4.619 seconds


What is the expected (average) score of each pairwise alignment
at random? (Perform some shuffled alignments to get an idea of what the
random expectation is).
I shuffled the whole proteomes then looked for the protein of interest and copied all of those into a separate file.

*shuffleseq at1.fa -outseq at1.shuffle.fa -shuffle 100*

*grep -A 10 "AT1G01600.1" at1.shuffle.fa | cat > at1.shuffleprotein.fa*

Then I used water to compare the random alignments.
*water ce1_B0213.10.fa at1.shuffleprotein.fa -gapopen 10 -gapextend 5 -outfile cel_B0213.10_at1_shuffleprotein.water*

Then I check the scores and find the average. I compare the average of the random alignments to the value I found in the real pairwise alignment.

*grep Score ce1_B0213.10_at1_shuffleprotein.water | awk '{sum+=$3} END {print sum/NR}'*
36.7

*grep Score ce1_B0213.10_dm1_shuffleprotein.water | awk '{sum+=$3} END {print sum/NR}'*
35.41
