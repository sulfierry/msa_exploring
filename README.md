In this section, the entire procedure is described, from obtaining the sequences, building the datasets, describing and the impacts of non-synonymous variations, applying multiple alignment, and structural modeling, culminating in virtual screening for repurposing new drugs that will be discussed in detail. 

![Alt text da image](https://github.com/gmmsb-lncc/CoV-2/blob/main/workflow.png)

# Sequence Acquisition

Currently, the largest database containing SARS-CoV-2 genomic sequences is **GISAID (Global Initiative on Sharing All Influenza Data)**, an international initiative where researchers from all over the world submit biologically obtained sequences following pre-established criteria.

From this database, 785,158 sequences were obtained containing the variants (Alpha(α), Beta(α), Delta(α), Gamma(α)) and 821,793 sequences containing the mutant trio (K417N, E484K, N501Y), totaling 1,606,951 sequences. These sets are crucial for identifying relevant mutations present in the receptor-binding domain (RBD). The data was collected between the dates 05/01/2020 and 06/01/2021.

It is of utmost importance to note that the announcement of the new Omicron (o) variant occurred on November 26, 2021, that is, after the data collection period. However, due to the severity of the current situation, exceptionally for this case, the information on this variant contained in the literature was considered, along with the information provided by the UK Health Security Agency (UKHSA) for structural modeling and conducting other analyses for this strain. The procedures carried out will be detailed below.

Following is an illustration of the ectodomain of the spike protein. It consists of the S1 and S2 domains. The S1 domain contains the Receptor Binding Domain (RBD) responsible for recognizing and binding to the host cell receptor. The S2 domain is responsible for fusion and contains the putative fusion peptide (FP, in turquoise) and the heptad repeat HR1 (orange) and HR2 (brown), TM is the transmembrane domain represented in purple.

![Alt text image](https://github.com/gmmsb-lncc/CoV-2/blob/main/spike_sub_units.png)

# Dataset Construction

A set composed of sequences identified in GISAID as associated with variants Alpha(α), Beta(α), Delta(α), Gamma(α) containing mutations (K417N, N439K, L452R, F456L, G476S, T478K, V483A, E484K, N501Y) was initially chosen, as these are the Variants of Concern (VoC).

The mutant trio set (K417N, E484K, N501Y) was selected because the combination of these three mutations is present in both the VoC and the Variants of Interest, and their presence results in a change in affinity with the extracellular receptor ACE2 (Angiotensin Converting Enzyme 2). This can influence the reduction of the neutralizing capacity of vaccines and/or convalescent plasma. Therefore, some sequences in the mutant trio may also be present in the variant set.

# Addressing the issue of downloading large numbers of sequences from GISAID

It's crucial to note that GISAID has a limit of 10,000 sequences for download at once; however, the file containing only the access codes (EPI_ISL) allows downloading about 270,000 access codes at once (each of the sequences has its EPI_ISL code).

It's clear that for a dataset containing 1,606,951 sequences, downloading every 10,000 sequences is an unfeasible task. Due to this characteristic of the database, the access codes to the sequences of the intersection between the previously mentioned sets and the reference file of Spike amino acid sequences used by GISAID were sought. The Python algorithm, whose function is to extract sequences that intersect between the studied sets and the Spike reference set used by GISAID (csv_extract_columns_find_intersec), is in the current directory.

# Identification of VNS through Multiple Sequence Alignment

A multiple sequence alignment (MSA) was performed using the MAFFT software (Multiple Alignment using Fast Fourier Transform) based on the previously mentioned sets, and the Spike sequence from Wuhan (NCBI YP009724390.1) was used as a reference. An illustrative visualization of the msa is shown below.

![Alt text image](https://github.com/gmmsb-lncc/CoV-2/blob/main/msa.png)

After processing the MSA, the variations between the amino acid residues for each of the subsets described here were quantified (fasta_MSA_count_mutations). For each reference residue contained in the study sequences, the physicochemical properties of the amino acids concerning the reference sequence Wuhan-Hu-1 (NCBI YP009724390.1) were annotated.

Subsequently, the most frequent residues in each set were identified, and from this, the consensus sequence was sought. The Python algorithm, whose function is the identification of the consensus sequences (consensus_msa), is present in the current directory.

For the visualization of the heatmap corresponding to the RBD and RBM regions, a function was built using the gmmsa package through the R language; the script (heatmap) is in the current directory. The heatmap figure is shown below. The image displays that: A) Correlation between the consensus sequences, where the variation in shade relative to the vertical reference indicates a change in the amino acid. B) Graphical representation of the consensus sequences, the gap indicates that the residue at this position underwent some variation, and the intensity of the blue color represents high prevalence of the amino acid. The less intense the blue, the lower the prevalence of the amino acid.

![Alt text image](https://github.com/gmmsb-lncc/CoV-2/blob/main/heatmap.png)
