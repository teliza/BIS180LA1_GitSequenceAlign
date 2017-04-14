Assignment 1
==============

# Template for Unix, git, and Sequence Alignment Assignments (20 pts possible)

__Full Name:__ Eliza Tsang

__Student ID:__ 999606858

*_Any code should be formatted as such_*

## Markdown

Paste the bio of your the student you interviewed here:
# Untitled
Natalie

__Biotechnlogy__

*Third-Year*

Likes:
* Cook
* Play video games
* Pet cats



## Unix

Paste the `date` command that you used for exercise one here:
*date* shows something like:
Thu Apr 13 16:36:13 PDT 2017


*date "+%Y-%m-%d %H:%M:%S"* results in something similar to this:
2017-04-13 16:36:19
## Git

Paste the URL for the shared github repository that you created with your lab partners here.  Use proper markdown formatting:

https://github.com/teliza/HelloTester2


## Sequence Alignment

*_Please answer the following questions Be clear and concise with any written answers._*

__1.__ Paste in the markdown table from the lab manual that includes for each genome:

* Size of the file
* Number of chromosomes
* Size of the genome in bp
* Number of protein-coding genes
* Average protein length

For _ONE_ of the files, provide the code that you used to answer these questions.

|Info/Organism   | A.thaliana    |C.elegans     |D.melanogaster     |
|:---------|:---:|:---:|:---:|
|Size of the file|141189371|117223923 |204986307|
|Number of chromosomes|7|7|15|
|Size of the genome in bp|119667750 |100286401|168736537|
|Number of protein-coding genes|35386|26769|30307|
|Average protein length|409|445|659|

In regards to A.Thaliana:

__To find the size of the file:__

*wc /home/ubuntu/Data/Species/A.thaliana/A.thaliana.fa*
and
*wc /home/ubuntu/Data/Species/A.thaliana/TAIR10_pep_20101214_updated*
I added the numbers from both results.

__To find the number of chromosomes:__

*grep -c ">" /home/ubuntu/Data/Species/A.thaliana/A.thaliana.fa*
Each chromosome name is preceded by ">" so we just need to look and count those.

__To find the size of the genome in bp:__

*grep -v ">" /home/ubuntu/Data/Species/A.thaliana/A.thaliana.fa | wc -m*

 *grep -v ">" /home/ubuntu/Data/Species/A.thaliana/A.thaliana.fa | wc -l*

The first command lists all the lines without the chromosome name and pipes the output into a count for all characters. The second command does the same but the output is piped into a count for lines.
I subtract the second value from the first. By doing that, I eliminate the invisible newlines.

__To find the number of protein coding genes:__

*grep ">" /home/ubuntu/Data/Species/A.thaliana/TAIR10_pep_20101214_updated*
Similar to the chromosomes in this file, the proteins' names have a > before each name.

__To find the average protein length:__
First I find the number of amino acids total by using this command:

*grep -v ">" /home/ubuntu/Data/Species/A.thaliana/TAIR10_pep_20101214_updated | wc -m*
14681363

*grep -v ">" /home/ubuntu/Data/Species/A.thaliana/TAIR10_pep_20101214_updated | wc -l*
198508

Again, I subtract the second number from the first to be sure that there are no newlines.
Then I divide the total number of amino acids by the number of proteins to calculate th average.

__2.__ How do you know that when you use `shuffleseq` the sequences have the same exact composition?

When we use the *diff* command on the shuffled sequences, it returns with no information, which is important to note too. It signifies that there are no differences to report so we know that the sequences have the same composition.

__3.__ Create a text based "histogram" as described in the lab manual
that shows the distribution of scores when aligning shuffled sequences.
Why is the shape of the curve not quite normal?

With such a high N (sample size) at random shuffling, I would expect the curve to be normal (bell shaped). However, the composition is already pre-determined by the protein sequence given by C.elegans, so the starting composition is not completely random. From the statistics, it probably looks like the graph is skewed right.

__4.__ What would be the effect of doing 1,000,000 shuffled sequences?

Statistically, I wouldn't expect much of a change. However, performing the sequences themselves would take much more time and processing power.

__5.__ Briefly discuss the relationship of sequence length and score.

The more matches in a sequence, the higher the score. Shorter sequences are easier to match but many matches (especially continuous ones) in a long sequence yield better score values.

__6.__ Starting with the C. elegans B0213.10 protein, find the best
match in the A. thaliana and D. melanogaster proteomes with `water`.
Record their alignment scores, percent identities, and protein names
here.  (Use a markdown table!)

|Info/Organism| A.thaliana| D.melanogaster|
|:----|:---:|:---:|
|Protein|  AT1G01600.1| FBpp0082086 |
|Score| 98.0| 98.0|
|Percent identity| 69/256 (27.0%) Similarity: 121/256 (47.3%)| 48/215 (22.3%) Similarity: 88/215 (40.9%)|

__7.__ What is the expected (average) score of each pairwise alignment
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

Both the scores for the random sequence alignments look much lower than the score for the real alignment.

__8.__ How many Z-scores away from the mean is the best alignment? **(omitted)**

__9.__ Briefly discuss the statistical and biological significance of your results.

Because the scores differ quite a lot between the random sequence alignment and the real pairwise scores, it seems like the real values are significant.

__10.__ Approximately how long would it take to compare two proteomes using `water`?

For the total time taken for both pairwise comparisons, the system took .37 second. The user display showed 223.27 seconds. **The total elapsed time was about 4 minutes and 43 seconds.**
