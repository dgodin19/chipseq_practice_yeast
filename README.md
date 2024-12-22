This project analyzes yeast samples from the paper ["Yeast Mediator facilitates transcription initiation at most promoters via a Tail-independent mechanism"](https://www.cell.com/molecular-cell/fulltext/S1097-2765(22)00905-4?_returnURL=https%3A%2F%2Flinkinghub.elsevier.com%2Fretrieve%2Fpii%2FS1097276522009054%3Fshowall%3Dtrue). 

## Experimental Design

The experimental design can be inferred from the following diagram:

![Experimental Design](experimental_design.png)

The authors included two input controls and IP experiments for each experimental condition. The conditions were separated into **DMSO** and **3IAA**, with respective genotypes.

## Analysis Decisions

To enhance signal clarity for downstream analyses, replicate BAM files were pooled together for peak finding and motif discovery. While this approach may obscure some differences between replicates, it provides stronger overall signals for identifying binding sites.
