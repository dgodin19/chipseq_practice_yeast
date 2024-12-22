This project analyzes yeast samples from the paper ["Yeast Mediator facilitates transcription initiation at most promoters via a Tail-independent mechanism"](https://www.cell.com/molecular-cell/fulltext/S1097-2765(22)00905-4?_returnURL=https%3A%2F%2Flinkinghub.elsevier.com%2Fretrieve%2Fpii%2FS1097276522009054%3Fshowall%3Dtrue). 

The general workflow for this project was to: 
- Obtain reads from the SRA. 
- Align reads to SacCer3 genome using bowtie (ebwa). Reference genome and the corresponding gff was obtained using the ncbi datasets command line tool.
- Call peaks using macs2. 
- Visualize peaks and signals using IGV.
- Find motifs with the MEME suite.


## Experimental Design

The experimental design can be inferred from the following diagram:

![Experimental Design](experimental_design.png)

The authors included two input controls and IP experiments for each experimental condition. The conditions were separated into **DMSO** and **3IAA**, with respective genotypes.

## Analysis Decisions

To enhance signal clarity for downstream analyses, replicate BAM files were pooled together for peak finding and motif discovery. While this approach may obscure some differences between replicates, it provides stronger overall signals for identifying binding sites.

After peaks were found, peaks were refined through: 

`cat output.bed | awk ' $5> 10 { print $0 }' > highscore.bed`

`sdust reference.fa > lowcomplexity.bed`

`bedtools intersect -v -a highscore.bed -b lowcomplexity.bed > highcomplexity.bed`

Credit goes to the Biostar handbook for suggesting these steps. 

## Peaks

![3IAA_1](results/bed_file_mods/3IAA_1.png)
![3IAA_2](results/bed_file_mods/3IAA_2.png)
![3IAA_3](results/bed_file_mods/3IAA_3.png)
![DMSO_1](results/bed_file_mods/DMSO_1.png)
![DMSO_2](results/bed_file_mods/DMSO_2.png)
![DMSO_3](results/bed_file_mods/DMSO_3.png)

To annotate the peaks, I had tried following what the authors did. This involved obtaining the gff file with transcription start sites as obtained in [Park et al. (2014)](https://pubmed.ncbi.nlm.nih.gov/24413663/). These attempts can be found in results/bed_file_mods/{condition}_annotated_peaks.bed . Alternatively, the annotated 

All steps from here followed [Ming Tang's guide to ChIP-Seq analysis](https://divingintogeneticsandgenomics.com/publication/2017-08-01-biostarhandbook/). 

## Heatmaps

Heatmaps were created to compare expression between DMSO and 3IAA conditions. 
![3IAA_1_vs_DMSO_1](results/peaks/3IAA_1_vs_DMSO_1.png)
![3IAA_2_vs_DMSO_2](results/peaks/3IAA_2_vs_DMSO_2.png)
![3IAA_3_vs_DMSO_3](results/peaks/3IAA_3_vs_DMSO_3.png)

## Metaprofile

![3IAA_1_vs_DMSO_1_signal](results/peaks/3IAA_1_vs_DMSO_1_signal.png)
![3IAA_2_vs_DMSO_2_signal](results/peaks/3IAA_2_vs_DMSO_2_signal.png)
![3IAA_3_vs_DMSO_3_signal](results/peaks/3IAA_3_vs_DMSO_3_signal.png)

The DMSO conditions appeared to have stronger signals throughout, except in the 3rd genomic condition which was: 
MAT{alpha} Dade2::hisG his3D200 leu2D0 lys2D0 met15D0 trp1D63 ura3D0 RPB3-3xFlag::NATMX pGPD1-OSTIR::HIS3 SUA7-13x Myc::HPH MED14-3xV5 IAA7::KanMX

This last set of genetic modifications appeared to neutralize most differences between conditions. This was consistent with what the authors found in figure 5b, with the DMSO condition having a stronger signal in most cases. 

## Motifs
The following tables can be found in results/motifs/{condition}/summary.tsv. 

3IAA_1

 Motif Analysis Results

| MOTIF_INDEX | MOTIF_SOURCE                          | MOTIF_ID         | ALT_ID    | CONSENSUS         | WIDTH | SITES | E-VALUE     | E-VALUE_SOURCE | MOST_SIMILAR_MOTIF_SOURCE                     | MOST_SIMILAR_MOTIF   | URL                                                     |
|-------------|---------------------------------------|------------------|-----------|-------------------|-------|-------|-------------|----------------|-----------------------------------------------|-----------------------|---------------------------------------------------------|
| 1           | MEME                                  | CMCSGGTTCGAHYCC  | MEME-1    | CMCSGGTTCGAHYCC   | 15    | 94    | 2.9e-206    | MEME           | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA0349.1 (OPI1)       | [Link](http://jaspar2022.genereg.net/matrix/MA0349.1)   |
| 2           | MEME                                  | KTGGYBYAGTGGKWA  | MEME-2    | KTGGYBYAGTGGKWA   | 15    | 139   | 5.7e-186    | MEME           |                                               |                       |                                                         |
| 3           | MEME                                  | CACCCACACCCACA   | MEME-3    | CACCCACACCCACA    | 14    | 19    | 2.1e-055    | MEME           | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA1107.2 (KLF9)       | [Link](http://jaspar2022.genereg.net/matrix/MA1107.2)   |
| 4           | db/motif_databases/MOUSE/uniprobe_mouse.meme | UP00053_2       | Rxra_secondary | KCRCRWAGKTYRNDHN | 16    | 465   | 3.9e-004    | CENTRIMO      |                                               |                       | [Link](http://thebrain.bwh.harvard.edu/uniprobe/details2.php?id=00053) |
| 5           | STREME                                | 1-GAATCGAACCSGKG | STREME-1  | GAATCGAACCSGKG    | 14    | 103   | 1.1e-002    | STREME        | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA0349.1 (OPI1)       | [Link](http://jaspar2022.genereg.net/matrix/MA0349.1)   |
| 6           | STREME                                | 2-CATCGTK        | STREME-2  | CATCGTK           | 7     | 53    | 1.7e-002    | STREME        |                                               |                       |                                                         |
| 7           | STREME                                | 3-WACCACTAVRCCAMN | STREME-3 | WACCACTAVRCCAMN   | 15    | 105   | 2.3e-002    | STREME        |                                               |                       |                                                         |

---

 Software and Command Details

 MEME-ChIP

- **Version**: 5.5.7 (released on Wed Jun 19 13:59:04 2024 -0700)
- **Documentation**: The format of this file is described at [MEME Suite Documentation](https://meme-suite.org/meme//doc/meme-chip-output-format.html).

 Command Used

```bash
meme-chip -oc . -time 240 -ccut 100 -dna -order 2 -minw 6 -maxw 15 \
-db db/motif_databases/MOUSE/uniprobe_mouse.meme \
-db db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme \
-meme-mod zoops -meme-nmotifs 3 -meme-searchsize 100000 \
-streme-pvt 0.05 -streme-align center -streme-totallength 4000000 \
-centrimo-score 5.0 -centrimo-ethresh 10.0 3IAA_1_sequences.fa

3IAA_2

 Motif Analysis Results

| MOTIF_INDEX | MOTIF_SOURCE                          | MOTIF_ID          | ALT_ID            | CONSENSUS           | WIDTH | SITES | E-VALUE     | E-VALUE_SOURCE | MOST_SIMILAR_MOTIF_SOURCE                     | MOST_SIMILAR_MOTIF   | URL                                                     |
|-------------|---------------------------------------|-------------------|-------------------|---------------------|-------|-------|-------------|----------------|-----------------------------------------------|-----------------------|---------------------------------------------------------|
| 1           | MEME                                  | CVBSGGTTCGATYCC   | MEME-1            | CVBSGGTTCGATYCC     | 15    | 175   | 2.4e-417    | MEME           | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA0349.1 (OPI1)       | [Link](http://jaspar2022.genereg.net/matrix/MA0349.1)   |
| 2           | MEME                                  | YTAMCCACTRVRCCA   | MEME-2            | YTAMCCACTRVRCCA     | 15    | 233   | 9.3e-406    | MEME           | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA1532.2 (NR1D2)      | [Link](http://jaspar2022.genereg.net/matrix/MA1532.2)   |
| 3           | MEME                                  | GCGYMWGACTNDWAA   | MEME-3            | GCGYMWGACTNDWAA     | 15    | 119   | 1.2e-150    | MEME           |                                               |                       |                                                         |
| 4           | STREME                                | 4-AAGGCGYMWGACTN  | STREME-4          | AAGGCGYMWGACTN      | 14    | 93    | 3.0e-010    | STREME         |                                               |                       |                                                         |
| 5           | STREME                                | 1-ACCACTAVRCYA    | STREME-1          | ACCACTAVRCYA        | 12    | 145   | 8.8e-004    | STREME         |                                               |                       |                                                         |
| 6           | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA1426.1       | MYB124             | NNACGCGCCN         | 10    | 322   | 9.1e-004    | CENTRIMO       |                                               |                       | [Link](http://jaspar2022.genereg.net/matrix/MA1426.1)   |
| 7           | STREME                                | 3-CCAACTDGGCYAH   | STREME-3          | CCAACTDGGCYAH       | 13    | 109   | 2.0e-003    | STREME         |                                               |                       |                                                         |
| 8           | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA1414.1       | E2FA               | WVGCGCCAHN         | 10    | 210   | 3.1e-003    | CENTRIMO       |                                               |                       | [Link](http://jaspar2022.genereg.net/matrix/MA1414.1)   |
| 9           | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA1496.1       | HOXA4              | RTMATTAV           | 8     | 391   | 4.7e-003    | CENTRIMO       |                                               |                       | [Link](http://jaspar2022.genereg.net/matrix/MA1496.1)   |
| 10          | STREME                                | 2-GRATCGAACCSSKGA | STREME-2          | GRATCGAACCSSKGA     | 15    | 174   | 5.1e-003    | STREME         | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA0349.1 (OPI1)       | [Link](http://jaspar2022.genereg.net/matrix/MA0349.1)   |
| 11          | db/motif_databases/MOUSE/uniprobe_mouse.meme | UP00003_2      | E2F3_secondary    | NNYWYGGCGCCAMDVBN   | 17    | 140   | 5.3e-003    | CENTRIMO       |                                               |                       | [Link](http://thebrain.bwh.harvard.edu/uniprobe/details2.php?id=00003) |
| 12          | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA1497.1       | HOXA6              | BGYMATTANN         | 10    | 400   | 6.1e-003    | CENTRIMO       |                                               |                       | [Link](http://jaspar2022.genereg.net/matrix/MA1497.1)   |
| 13          | db/motif_databases/MOUSE/uniprobe_mouse.meme | UP00001_2      | E2F2_secondary    | NNNWYGGCGCCAANNKB   | 17    | 159   | 7.7e-003    | CENTRIMO       |                                               |                       | [Link](http://thebrain.bwh.harvard.edu/uniprobe/details2.php?id=00001) |
| 14          | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA1500.1       | HOXB6              | BTAATKRC           | 8     | 393   | 8.3e-003    | CENTRIMO       |                                               |                       | [Link](http://jaspar2022.genereg.net/matrix/MA1500.1)   |
| 15          | db/motif_databases/MOUSE/uniprobe_mouse.meme | UP00089_1      | Tcf1_primary      | NHTHRGTTAACTAADNN   | 17    | 218   | 8.7e-003    | CENTRIMO       |                                               |                       | [Link](http://thebrain.bwh.harvard.edu/uniprobe/details2.php?id=00089) |
| 16          | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA0232.1       | lbl                | TAATKA             | 6     | 398   | 1.1e-002    | CENTRIMO       |                                               |                       | [Link](http://jaspar2022.genereg.net/matrix/MA0232.1)   |
| 17          | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA1051.1       | RAP2-3             | GCGCCGCC           | 8     | 149   | 1.2e-002    | CENTRIMO       |                                               |                       | [Link](http://jaspar2022.genereg.net/matrix/MA1051.1)   |
| 18          | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA1831.1       | Zm00001d031796     | SGACGGCGACGV       | 12    | 131   | 2.4e-002    | CENTRIMO       |                                               |                       | [Link](http://jaspar2022.genereg.net/matrix/MA1831.1)   |
| 19          | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA0213.1       | brk                | SYGGCGCY           | 8     | 137   | 4.4e-002    | CENTRIMO       |                                               |                       | [Link](http://jaspar2022.genereg.net/matrix/MA0213.1)   |
| 20          | db/motif_databases/MOUSE/uniprobe_mouse.meme | UP00103_1      | Jundm2_primary    | NSGATGACGTCAYCRH    | 16    | 69    | 4.7e-002    | CENTRIMO       |                                               |                       | [Link](http://thebrain.bwh.harvard.edu/uniprobe/details2.php?id=00103) |

---

 Software and Command Details

 MEME-ChIP

- **Version**: 5.5.7 (released on Wed Jun 19 13:59:04 2024 -0700)
- **Documentation**: The format of this file is described at [MEME Suite Documentation](https://meme-suite.org/meme//doc/meme-chip-output-format.html).

 Command Used

```bash
meme-chip -oc . -time 240 -ccut 100 -dna -order 2 -minw 6 -maxw 15 \
-db db/motif_databases/MOUSE/uniprobe_mouse.meme \
-db db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme \
-meme-mod zoops -meme-nmotifs 3 -meme-searchsize 100000 \
-streme-pvt 0.05 -streme-align center -streme-totallength 4000000 \
-centrimo-score 5.0 -centrimo-ethresh 10.0 3IAA_2_sequences.fa


3IAA_3

 Motif Analysis Results

| MOTIF_INDEX | MOTIF_SOURCE                          | MOTIF_ID          | ALT_ID            | CONSENSUS           | WIDTH | SITES | E-VALUE     | E-VALUE_SOURCE | MOST_SIMILAR_MOTIF_SOURCE                     | MOST_SIMILAR_MOTIF   | URL                                                     |
|-------------|---------------------------------------|-------------------|-------------------|---------------------|-------|-------|-------------|----------------|-----------------------------------------------|-----------------------|---------------------------------------------------------|
| 1           | MEME                                  | WGAAAAARAAAARAA   | MEME-1            | WGAAAAARAAAARAA     | 15    | 469   | 1.2e-127    | MEME           | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA1274.1 (DOF3.6)     | [Link](http://jaspar2022.genereg.net/matrix/MA1274.1)   |
| 2           | MEME                                  | CTTTGACCCAGGTAG   | MEME-2            | CTTTGACCCAGGTAG     | 15    | 21    | 1.2e-026    | MEME           |                                               |                       |                                                         |
| 3           | MEME                                  | CCGTATAATCAACGG   | MEME-3            | CCGTATAATCAACGG     | 15    | 23    | 5.0e-020    | MEME           |                                               |                       |                                                         |
| 4           | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA0062.3       | GABPA              | NNCACTTCCTGTNN      | 14    | 1009  | 2.8e-003    | CENTRIMO       |                                               |                       | [Link](http://jaspar2022.genereg.net/matrix/MA0062.3)   |
| 5           | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA1063.1       | TCP19              | TGGGSCCCAC          | 10    | 628   | 1.0e-002    | CENTRIMO       |                                               |                       | [Link](http://jaspar2022.genereg.net/matrix/MA1063.1)   |
| 6           | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA0305.1       | GCR2               | GCTTCCH             | 7     | 989   | 3.6e-002    | CENTRIMO       |                                               |                       | [Link](http://jaspar2022.genereg.net/matrix/MA0305.1)   |
| 7           | STREME                                | 1-CTCATCKCWT      | STREME-1          | CTCATCKCWT          | 10    | 190   | 5.0e-002    | STREME         | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA0351.1 (DOT6)       | [Link](http://jaspar2022.genereg.net/matrix/MA0351.1)   |

---

 Software and Command Details

 MEME-ChIP

- **Version**: 5.5.7 (released on Wed Jun 19 13:59:04 2024 -0700)
- **Documentation**: The format of this file is described at [MEME Suite Documentation](https://meme-suite.org/meme//doc/meme-chip-output-format.html).

 Command Used

```bash
meme-chip -oc . -time 240 -ccut 100 -dna -order 2 -minw 6 -maxw 15 \
-db db/motif_databases/MOUSE/uniprobe_mouse.meme \
-db db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme \
-meme-mod zoops -meme-nmotifs 3 -meme-searchsize 100000 \
-streme-pvt 0.05 -streme-align center -streme-totallength 4000000 \
-centrimo-score 5.0 -centrimo-ethresh 10.0 3IAA_3_sequences.fa

DMSO_1

 Motif Analysis Results

| MOTIF_INDEX | MOTIF_SOURCE                          | MOTIF_ID          | ALT_ID             | CONSENSUS           | WIDTH | SITES | E-VALUE     | E-VALUE_SOURCE | MOST_SIMILAR_MOTIF_SOURCE                     | MOST_SIMILAR_MOTIF            | URL                                                     |
|-------------|---------------------------------------|-------------------|--------------------|---------------------|-------|-------|-------------|----------------|-----------------------------------------------|-------------------------------|---------------------------------------------------------|
| 1           | MEME                                  | TTTTYTTYYTYTTTT   | MEME-1             | TTTTYTTYYTYTTTT     | 15    | 587   | 3.0e-083    | MEME           | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA1268.1 (CDF5)                | [Link](http://jaspar2022.genereg.net/matrix/MA1268.1)   |
| 2           | STREME                                | 1-GAAAAAAAA       | STREME-1           | GAAAAAAAA           | 9     | 125   | 3.3e-002    | STREME         | db/motif_databases/MOUSE/uniprobe_mouse.meme   | UP00028_2 (Tcfap2e_secondary) | [Link](http://thebrain.bwh.harvard.edu/uniprobe/details2.php?id=00028) |
| 3           | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA1829.1       | Zm00001d035604     | NNTACCTAACHNN       | 13    | 663   | 4.9e-002    | CENTRIMO       |                                               |                               | [Link](http://jaspar2022.genereg.net/matrix/MA1829.1)   |

---

 Software and Command Details

 MEME-ChIP

- **Version**: 5.5.7 (released on Wed Jun 19 13:59:04 2024 -0700)
- **Documentation**: The format of this file is described at [MEME Suite Documentation](https://meme-suite.org/meme//doc/meme-chip-output-format.html).

 Command Used

```bash
meme-chip -oc . -time 240 -ccut 100 -dna -order 2 -minw 6 -maxw 15 \
-db db/motif_databases/MOUSE/uniprobe_mouse.meme \
-db db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme \
-meme-mod zoops -meme-nmotifs 3 -meme-searchsize 100000 \
-streme-pvt 0.05 -streme-align center -streme-totallength 4000000 \
-centrimo-score 5.0 -centrimo-ethresh 10.0 DMSO_1_sequences.fa

DMSO_2

 Motif Analysis Results

| MOTIF_INDEX | MOTIF_SOURCE                          | MOTIF_ID          | ALT_ID             | CONSENSUS           | WIDTH | SITES | E-VALUE     | E-VALUE_SOURCE | MOST_SIMILAR_MOTIF_SOURCE                     | MOST_SIMILAR_MOTIF            | URL                                                     |
|-------------|---------------------------------------|-------------------|--------------------|---------------------|-------|-------|-------------|----------------|-----------------------------------------------|-------------------------------|---------------------------------------------------------|
| 1           | MEME                                  | TTTTYTTSYTYTTYT   | MEME-1             | TTTTYTTSYTYTTYT     | 15    | 522   | 3.1e-087    | MEME           | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA1268.1 (CDF5)                | [Link](http://jaspar2022.genereg.net/matrix/MA1268.1)   |
| 2           | MEME                                  | TCCTACCTACCTGGG   | MEME-2             | TCCTACCTACCTGGG     | 15    | 16    | 3.4e-015    | MEME           | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA1038.1 (MYB3)                | [Link](http://jaspar2022.genereg.net/matrix/MA1038.1)   |
| 3           | MEME                                  | AAGACATCCTATC     | MEME-3             | AAGACATCCTATC       | 13    | 16    | 4.4e-007    | MEME           |                                               |                               |                                                         |

---

 Software and Command Details

 MEME-ChIP

- **Version**: 5.5.7 (released on Wed Jun 19 13:59:04 2024 -0700)
- **Documentation**: The format of this file is described at [MEME Suite Documentation](https://meme-suite.org/meme//doc/meme-chip-output-format.html).

 Command Used

```bash
meme-chip -oc . -time 240 -ccut 100 -dna -order 2 -minw 6 -maxw 15 \
-db db/motif_databases/MOUSE/uniprobe_mouse.meme \
-db db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme \
-meme-mod zoops -meme-nmotifs 3 -meme-searchsize 100000 \
-streme-pvt 0.05 -streme-align center -streme-totallength 4000000 \
-centrimo-score 5.0 -centrimo-ethresh 10.0 DMSO_2_sequences.fa

DMSO_3

 Motif Analysis Results

| MOTIF_INDEX | MOTIF_SOURCE                          | MOTIF_ID          | ALT_ID             | CONSENSUS               | WIDTH | SITES | E-VALUE     | E-VALUE_SOURCE | MOST_SIMILAR_MOTIF_SOURCE                     | MOST_SIMILAR_MOTIF            | URL                                                     |
|-------------|---------------------------------------|-------------------|--------------------|-------------------------|-------|-------|-------------|----------------|-----------------------------------------------|-------------------------------|---------------------------------------------------------|
| 1           | MEME                                  | AAAARRAARAARARA   | MEME-1             | AAAARRAARAARARA         | 15    | 706   | 4.2e-088    | MEME           | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA1274.1 (DOF3.6)               | [Link](http://jaspar2022.genereg.net/matrix/MA1274.1)   |
| 2           | MEME                                  | GGAAKTGARAGGGAG   | MEME-2             | GGAAKTGARAGGGAG         | 15    | 48    | 4.8e-021    | MEME           | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA0205.2 (Trl)                  | [Link](http://jaspar2022.genereg.net/matrix/MA0205.2)   |
| 3           | MEME                                  | GGATGTCTTTGACCC   | MEME-3             | GGATGTCTTTGACCC         | 15    | 25    | 1.6e-014    | MEME           | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA1147.1 (NR4A2::RXRA)          | [Link](http://jaspar2022.genereg.net/matrix/MA1147.1)   |
| 4           | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA1428.1 | TCP8                | BGGSCCCAC               | 9     | 864   | 1.5e-003    | CENTRIMO       |                                               |                               | [Link](http://jaspar2022.genereg.net/matrix/MA1428.1)   |
| 5           | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA1919.1 | Tbox-a             | NNNNNNRGGTGTGAANDNNNNN  | 22    | 812   | 5.9e-003    | CENTRIMO       |                                               |                               | [Link](http://jaspar2022.genereg.net/matrix/MA1919.1)   |
| 6           | STREME                                | 1-GCGATGA         | STREME-1           | GCGATGA                 | 7     | 443   | 8.1e-003    | STREME         | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA0350.1 (TOD6)                 | [Link](http://jaspar2022.genereg.net/matrix/MA0350.1)   |
| 7           | STREME                                | 2-CCCTTTT         | STREME-2           | CCCTTTT                 | 7     | 816   | 9.5e-003    | STREME         | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA0657.1 (KLF13)                | [Link](http://jaspar2022.genereg.net/matrix/MA0657.1)   |
| 8           | db/motif_databases/MOUSE/uniprobe_mouse.meme | UP00388_1 | Six6_2267.4         | DATRGGGTATCAHNWHT        | 17    | 225   | 1.5e-002    | CENTRIMO       |                                               |                               | [Link](http://thebrain.bwh.harvard.edu/uniprobe/details2.php?id=00388) |
| 9           | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA1116.1 | RBPJ               | BVTGGGAANN              | 10    | 508   | 2.3e-002    | CENTRIMO       |                                               |                               | [Link](http://jaspar2022.genereg.net/matrix/MA1116.1)   |
| 10          | STREME                                | 3-ATATATAAA       | STREME-3           | ATATATAAA               | 9     | 318   | 3.9e-002    | STREME         | db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme | MA0379.1 (MOT2)                 | [Link](http://jaspar2022.genereg.net/matrix/MA0379.1)   |

---

 Software and Command Details

 MEME-ChIP

- **Version**: 5.5.7 (released on Wed Jun 19 13:59:04 2024 -0700)
- **Documentation**: The format of this file is described at [MEME Suite Documentation](https://meme-suite.org/meme//doc/meme-chip-output-format.html).

 Command Used

```bash
meme-chip -oc . -time 240 -ccut 100 -dna -order 2 -minw 6 -maxw 15 \
-db db/motif_databases/MOUSE/uniprobe_mouse.meme \
-db db/motif_databases/JASPAR/JASPAR2022_CORE_non-redundant_v2.meme \
-meme-mod zoops -meme-nmotifs 3 -meme-searchsize 100000 \
-streme-pvt 0.05 -streme-align center -streme-totallength 4000000 \
-centrimo-score 5.0 -centrimo-ethresh 10.0 DMSO_3_sequences.fa
