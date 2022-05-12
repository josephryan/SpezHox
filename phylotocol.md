# Echinometra Hox analysis 
 Principal Investigator: Joseph Ryan, Remi Ketchum, Adam Reitzel  
 Draft or Version Number: v.0.1  
 Date: 28 September 2021  

## SUMMARY OF CHANGES FROM PREVIOUS VERSION
first version
  
## 1 INTRODUCTION: BACKGROUND INFORMATION AND SCIENTIFIC RATIONALE  

  The objective is to accurately classify the Hox and ParaHox genes from Echinometra sp. EZ and compare to an array of other echinoderms.  
  To do this we will conduct phylogenetic analyses on Hox genes from the following echinoderms and outgroups:

  *Echinometra sp. EZ*
  *Strongylocentrotus purpuratus*
  *Lytechinus variegatus*
  *Patiria miniata*
  *Parastichopus parvimensis*
  *Holothuria glaberrima*
  *Homo sapiens*
  *Drosophila melanogaster*
  *Branchiostoma floridae*
  *Tribolium castaneum*
  *Capitella teleta*
  *Crassostrea gigas*
  
## 2 STUDY DESIGN AND ENDPOINTS  

#### 2.1 Build dataset and alignment.

We will use the curated HomeoDB (Zhong et al, 2008) to assemble HOXL subclass homeodomains from the following species: *Homo sapiens*, *Branchiostoma floridae*, 
  *Drosophila melanogaster*, and *Tribolium castaneum*. We will include the spiralia Hox/ParaHox homeodomains *Capitella teleta* and 
  *Crassostrea gigas* (as classified in Paps et al, 2015 and Zwarycz et al, 2015). We will use hd60.hmm hidden Markov model from Zwarycz et al (2015) 
  to search transcriptomes from the following:  *Echinometra sp. EZ*, *Strongylocentrotus purpuratus*, *Lytechinus variegatus*, *Patiria miniata*, 
  *Parastichopus parvimensis*, and *Holothuria glaberrima*. NOTE: we will remove the three Hox3-related genes of 
  *Drosophila melanogaster* (zen1, zen2, and bicoid) as they are known to be highly derived relative to the ancestral Hox3 sequence as was done in Ryan et al (2007).

This script runs hmmsearch, stockholm2fasta, and some custom code to remove indels and fill end gaps.
```
hmm2aln.pl --hmm=hd60.hmm --name=HD --fasta_dir=02-RENAMED_DATA --threads=40 --nofillcnf=nofill.hox.conf > cnid_hox_plus.fa
```
We will remove from the resulting alignment any sequences with more than 5 gaps (8.3%)

#### 2.2 Run initial ML tree

```
iqtree-omp -s [infile.mafft-gb] -nt AUTO -m LG -pre [output prefix] > iq.out 2> iq.err
```

#### 2.3 Prune non-Hox/ParaHox genes

This script takes a prefix of a subset of (our ingroup) taxa and will return an alignment of only those genes within the clade descended from the most recent common ancestor of all genes with the specified prefix. We will use "Human" as 
```
make_subalignment --tree=<newick_treefile> --aln=<phylip_alignment> --root=<root_taxa> --pre=<prefix>
```

### 2.4 Here we removed three S. purpuratus sequences (XP_011675355, XP_011682234, and XP_011682243), which were copies of three other sequences (NP_999816, NP_999774, and XP_793141 respectively) 

### 2.5 Run final ML tree
```
iqtree -m [best-fit_model]+G4 -s [alignment_file] -pre [prefix] -bb 1000
```

## 3 WORK COMPLETED SO FAR WITH DATES  

28 September 2021 - No work has been started

## 4 LITERATURE REFERENCED  

  Paps J, Xu F, Zhang G, Holland PW. Reinforcing the egg-timer: Recruitment of novel Lophotrochozoa homeobox genes to early and late development in the Pacific oyster. Genome Biology and Evolution. 2015 https://doi.org/10.1093/gbe/evv018

  Ryan JF, Mazza ME, Pang K, Matus DQ, Baxevanis AD, Martindale MQ, Finnerty JR. Pre-bilaterian origins of the Hox cluster and the Hox code: evidence from the sea anemone, Nematostella vectensis. PloS One. 2007 https://doi.org/10.1371/journal.pone.0000153

  Zhong YF, Butts T, Holland PW. HomeoDB: a database of homeobox gene diversity. Evolution & Development. 2008 https://doi.org/10.1111/j.1525-142X.2008.00266.x

  Zwarycz AS, Nossa CW, Putnam NH, Ryan JF. Timing and scope of genomic expansion within Annelida: evidence from homeoboxes in the genome of the earthworm Eisenia fetida. Genome Biology and Evolution. 2015 https://doi.org/10.1093/gbe/evv243
  
## APPENDIX

Version&nbsp; &nbsp; &nbsp; &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; &nbsp;Date&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; Significant Revisions  
1.1  May 12, 2022  added 2.4
1.2  
1.3  
1.4  
