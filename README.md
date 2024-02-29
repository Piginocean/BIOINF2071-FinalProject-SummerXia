# BIOINF2071-SummerXia
### Final Project for BIOINF2071
The prevalence and severity of metabolic dysfunction-associated steatotic liver disease (MASLD) demonstrate significant variation among patients, a portion of which can be attributed to genetic heterogeneity [1]. Among the genetic factors, PNPLA3 I148M variant (rs738409[G]) has emerged as a strong influencer. This variant has been robustly associated with MASLD, influencing hepatic steatosis, fibrosis, cirrhosis, and hepatocellular carcinoma [2]. Given this, stratifying MASLD patients by their PNPLA3 genotype offers a valuable strategy for understanding the genetic impacts on MASLD pathophysiology and drug response. Hence, this study proposes to leverage the PNPLA3-genotype stratification in MASLD patients to perform transcriptomic analysis, protein-protein interaction network construction, and network-based drug repurposing. This work is a follow-up study from the previous work that was published in our lab [1]. This final project for BIOINF2017 aims to complete Aim 1 and initiate explorations of Aim 2.
### Hypothesis:
MASLD patients carrying the PNPLA3 I148M (GG) variant exhibit distinct transcriptomic signatures and protein-protein interaction networks compared to those without the variant. These molecular differences may influence the patients' responses to therapies, suggesting that a genotype-specific therapeutic approach would be more effective for the treatment of PNPLA3-related MASLD.

### Aim 1: To identify the PNPLA3-genotype-dependent transcriptomic signatures of MASLD.
**Data Sources:** RNA-seq data were obtained from liver biopsy samples of patients undergoing bariatric surgery [3, 4]. A total of 182 patients were categorized based on predominant liver histology findings into normal (n=36), steatosis (n=46), lobular inflammation (n=50), or fibrosis (n=50) across different MASLD stages. The patient-gene expression matrix includes data for 182 patients and 18,307 genes per patient (BioProject ID: PRJNA512027).

***Sub aim 1.1: Extraction of the PNPLA3 genotype from RNA-sequencing data.***

**Method:** This step involves using BLAST to match the reference sequence of PNPLA3 rs738409 from Ensembl (150bp, ±75bp flanking the SNP) with query sequences from the SRA Experiment set (SRX). Additionally, the dataset will be analyzed using principal component analysis (PCA).

**Results:** Identification of PNPLA3 GG carriers among the patient categories showed the following distribution: Normal (0/36), Steatosis (5/46), Lobular Inflammation (3/50), and Fibrosis (5/50).

***Sub aim 1.2: Identification of differentially expressed genes (DEGs) and differentially enriched pathways specific to PNPLA3 I148M (GG) MASLD carriers from RNA-seq data.***

**Method:** The DEGs (FDR p-value < 0.001) will be identified from the gene expression data applying the standard LIMMA-VOOM [5, 6] pipeline for pairwise comparisons, PNPLA3 CC/CG vs GG groups in different disease stage. Differentially enriched pathways were identified by ranking the genes by t-statistic for each pairwise comparison and then performing GSEA [7] using the MSigDB v7.0 C2 KEGG pathways [8].

**Expected Results:** We anticipate identifying DEGs and pathways that are unique to PNPLA3 GG groups, containing the aggregated up- and down-regulated genes/pathways for each of these pairwise comparisons. The results will suggest the impact of the PNPLA3 I148M (GG) variant on the pathophysiology of MASLD. 

***Sub aim 1.3: Generation of a PNPLA3-specific protein-protein interaction (PPI) network associated with MASLD.***

**Method:** The liver BioSnap network [9] which contains 3180 nodes, and 48,409 edges was retrieved for the overall liver background PPI. Then, the MASLD PPI background network was generated as follows: we selected the KEGG pathway [10, 11] map of MASLD, which represents a stage-dependent progression of MASLD (pathway id: hsa04932) in addition to 10 interrelated pathways: TNF-signaling (hsa04668), insulin signaling (hsa04910), type II diabetes mellitus (hsa04930), PI3K-Akt signaling (hsa04151), adipocytokine signaling (hsa04920), PPAR signaling (hsa03320), fatty acid biosynthesis (hsa00061), protein processing in the endoplasmic reticulum (hsa04141), oxidative phosphorylation (hsa00190), and apoptosis (hsa04210). This MASLD PPI network will be refined to include only nodes differentially expressed in the PNPLA3 CC/CG vs GG comparisons.

**Expected Results:** We expected to generate a PNPLA3-MASLD sub-network with key nodes and edges that are highly associated with PNPLA3 GG high-risk variant.

### Aim 2: Aim 2: To identify the drug candidates for PNPLA3-specific MASLD using Quantitative Systems Pharmacology (QSP) approach.
**Rationale:** The development of new drugs is a time-consuming and costly process. Using QSP approach to target MASLD could lower the costs and shorten the drug discovery process accelerating the translation of research findings into clinical applications. The Connectivity Map (CMap) database is a resource containing gene expression profiles of human cells treated with various small molecules [12]. CMap can be utilized to find drugs that can potentially reverse the identified transcriptomic signatures. Further, by calculating network proximity [13], we aim to prioritize these drugs based on their relevance to the disease-associated proteins and pathways, thereby making the drug selection process more efficient and targeted.

***Sub aim 2.1 Identification of potential therapeutic compounds in CMap that may reverse the gene expression changes associated with PNPLA3 genotype-specific transcriptomic signatures in MASLD.***

**Method:** The drug repurposing algorithm [14, 15] takes two inputs: 1) full DEG signatures, an ordered list of up- and down-regulated genes in a disease (results from Aim 1.2), and 2) the data from CMap, consisting of rank fold change (FC) of each gene after drug treatment. A Kolmogorov-Smirnov test (K-S test) of the gene expression ranks in the disease and drug signatures will be used to assign each drug a CMap score, which reflects the degree to which the drug “flips” the signature of disease.

**Expected Results:** We expect to identify the top five compounds as potential therapeutics for PNPLA3-related MASLD.

***Sub aim 2.2 Ranking of identified drug candidates by calculating their network proximity for drug repurposing in PNPLA3-specific MASLD.***

**Method:** The method evaluates the distance between the compound’s targets and a given disease module based on the premise that a compound is effective against disease if its target proteins are within or in the immediate vicinity of the disease module [13]. This approach provides an independent criterion for selecting from amongst CMap-extracted compounds, to enable further prioritization for experimental testing. PNPLA3-MASLD sub-network will be used as the disease module to determine the proximity of the compounds identified from CMap. For each compound/drug, we will extract the set of targets (T) from DrugBank v5.1.4 [16], and a distance (d) to the PNPLA3-MASLD subnetwork nodes(S) was calculated using the liver PPI network as the shortest distance between node t (belonging to T) and the closest node s (belonging to S) averaged over all nodes in T.

$$ d(S, T) = \frac{1}{\|T\|} \sum_{t \in T} \min_{s \in S}d(s, t) $$

A reference distance distribution was constructed, corresponding to the expected distance between two randomly selected groups of proteins of the same size and degree of distribution as the disease proteins and drug targets in the network. This bootstrapping procedure will be repeated 1000 times, and the mean (µ) and standard deviation (σ) of the reference distance distribution in conjunction with the distance (d) determined above were used to calculate a z-score (d– µ)/σ for each drug. A lower z-score means a drug’s target profile is closer to the disease module. This method is adopted from the previous publication [3].!

**Expected Results:** We anticipated identifying top five compounds as potential therapeutics against PNPLA3-related MASLD. 

### Reference
1. Mantovani A. Not all NAFLD patients are the same: We need to find a personalized therapeutic approach. Dig Liver Dis. 2019;51(1):176-177. doi:10.1016/j.dld.2018.10.009
2. Trépo E, Romeo S, Zucman-Rossi J, Nahon P. PNPLA3 gene in liver diseases. J Hepatol. 2016;65(2):399-412. doi:10.1016/j.jhep.2016.03.011
3. Lefever, Daniel E et al. “A Quantitative Systems Pharmacology Platform Reveals NAFLD Pathophysiological States and Targeting Strategies.” Metabolites vol. 12,6 528. 7 Jun. 2022, doi:10.3390/metabo12060528 
4. Gerhard, Glenn S et al. “Transcriptomic Profiling of Obesity-Related Nonalcoholic Steatohepatitis Reveals a Core Set of Fibrosis-Specific Genes.” Journal of the Endocrine Society vol. 2,7 710-726. 5 Jun. 2018, doi:10.1210/js.2018-00122
5. Ritchie, M.E.; Phipson, B.; Wu, D.; Hu, Y.; Law, C.W.; Shi, W.; Smyth, G.K. limma powers differential expression analyses for RNA-sequencing and microarray studies. Nucleic Acids Res. 2015, 43, e47. 
6. Law, C.W.; Chen, Y.; Shi,W.; Smyth, G.K. Voom: Precision weights unlock linear model analysis tools for RNA-seq read counts. Genome Biol. 2014, 15, R29.
7. Subramanian, A.; Tamayo, P.; Mootha, V.K.; Mukherjee, S.; Ebert, B.L.; Gillette, M.A.; Paulovich, A.; Pomeroy, S.L.; Golub, T.R.; Lander, E.S.; et al. Gene set enrichment analysis: A knowledge-based approach for interpreting genome-wide expression profiles. Proc. Natl. Acad. Sci. USA 2005, 102, 15545–15550.
8. Liberzon, A.; Subramanian, A.; Pinchback, R.; Thorvaldsdóttir, H.; Tamayo, P.; Mesirov, J.P. Molecular signatures database (MSigDB) 3.0. Bioinformatics 2011, 27, 1739–1740.
9. Marinka Zitnik, R.S.; Maheshwari, S.; Leskovec, J. BioSNAP Datasets: Stanford Biomedical Network Dataset Collection. 2018. Available online: https://snap.stanford.edu/biodata/ 
10. Kanehisa, M.; Furumichi, M.; Tanabe, M.; Sato, Y.; Morishima, K. KEGG: New perspectives on genomes, pathways, diseases and drugs. Nucleic Acids Res. 2017, 45, D353–D361.
11. Kanehisa, M.; Goto, S. KEGG: Kyoto Encyclopedia of Genes and Genomes. Nucleic Acids Res. 2000, 28, 27–30.
12. Subramanian, A.; Narayan, R.; Corsello, S.M.; Peck, D.D.; Natoli, T.E.; Lu, X.; Gould, J.; Davis, J.F.; Tubelli, A.A.; Asiedu, J.K.; et al. A next generation connectivity map: L1000 platform and the first 1,000,000 profiles. Cell 2017, 171, 1437–1452.
13. Guney, E.; Menche, J.; Vidal, M.; Barabasi, A.L. Network-based in silico drug efficacy screening. Nat. Commun. 2016, 7, 10331.
14. Sirota M, et al. Discovery and preclinical validation of drug indications using compendia of public gene expression data. Sci. Transl. Med 3, 96ra77 (2011).
15. Chen B, et al. Computational Discovery of Niclosamide Ethanolamine, a Repurposed Drug Candidate That Reduces Growth of Hepatocellular Carcinoma Cells In Vitro and in Mice by Inhibiting Cell Division Cycle 37 Signaling. Gastroenterology 152, 2022–2036 (2017). 
16. Wishart, D.S.; Feunang, Y.D.; Guo, A.C.; Lo, E.J.; Marcu, A.; Grant, J.R.; Sajed, T.; Johnson, D.; Li, C.; Sayeeda, Z.; et al. DrugBank 5.0: A Major Update to the DrugBank Database for 2018. Nucleic Acids Res. 2018, 46, D1074–D1082.
