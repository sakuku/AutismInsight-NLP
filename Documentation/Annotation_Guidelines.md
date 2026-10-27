# AutismInsight NLP — Research Topic Annotation Guidelines

## Purpose

Create consistent, human-reviewed **primary topic labels** for PubMed abstracts about autism spectrum disorder (ASD). These labels will support the TF-IDF baseline and transformer topic classifier. **PubMed search-query categories are candidate labels, not verified ground truth.**

## Dataset and files

* Data/autisminsight\_label\_review.csv: master review sheet containing 956 unique articles.
* Data/annotation\_member\_1.csv, annotation\_member\_2.csv, annotation\_member\_3.csv: proposed reviewer assignments (319, 319, and 318 rows).
* Data/Raw/autisminsight\_raw\_1000.csv: original search results, including overlapping category assignments.

Each reviewer should inspect Title and Abstract, use Candidate\_Categories only as a suggestion, and fill in Primary\_Category, Review\_Status, and Reviewer\_Notes.

## Topic labels

|Label|Use when the paper's **main research question** concerns…|Examples|
|-|-|-|
|**Genetics**|Genes, DNA variants, inheritance, genomic sequencing, or genetic risk in ASD.|Rare variants, GWAS, gene-expression genetics.|
|**Neuroscience**|Brain structure/function, neural circuits, neurodevelopment, or neural signaling in ASD.|fMRI, connectivity, synaptic function.|
|**Immunology**|Immune pathways, inflammatory responses, autoimmunity, or immune-related mechanisms in ASD.|Cytokines, maternal immune activation.|
|**Metabolism**|Metabolic pathways, mitochondria, energy use, or biochemical metabolism in ASD.|Mitochondrial dysfunction, metabolic profiling.|
|**Interventions**|Testing or evaluating treatments, therapies, educational/behavioral programs, or other interventions for ASD.|Behavioral intervention trials, treatment outcomes.|

## Annotation rules

1. **Read the title and abstract in full.** Do not assign a label from keywords alone.
2. Choose **one primary category** based on the central aim, methods, and outcomes. A paper may mention multiple topics; select the one it primarily investigates.
3. For a study evaluating a treatment's effectiveness, choose **Interventions** even if the treatment targets a biological pathway, unless the central research question is mechanistic rather than treatment-focused.
4. For mixed-topic studies, prioritize the principal research question, not the number of mentions. Explain difficult decisions in Reviewer\_Notes.
5. If the article is not substantially about ASD or none of the five categories fits, set Primary\_Category to **Exclude**, Review\_Status to **Excluded**, and explain why.
6. If you cannot confidently choose, leave Primary\_Category blank, set Review\_Status to **Needs Discussion**, and describe the ambiguity.
7. When a decision is clear, set Review\_Status to **Reviewed**.
8. Do not infer a paper's findings or evidence quality from the topic label. **Finding/evidence and uncertainty classification is a separate task requiring separate labels.**

## Example decisions

* “Rare SCN2A variants associated with ASD” → **Genetics** (variant association is central).
* “Altered functional connectivity in autistic adolescents” → **Neuroscience**.
* “Inflammatory cytokine profiles in children with ASD” → **Immunology**.
* “Mitochondrial respiration in ASD-derived cells” → **Metabolism**.
* “Randomized trial of a parent-mediated communication program” → **Interventions**.
* “Vitamin D, inflammation, and ASD” → **Needs Discussion** until the main research question is clear; do not label by the first matching term.

## Quality assurance and agreement

* **Before bulk annotation:** the team should jointly label a small calibration set (for example, 15–20 abstracts), discuss disagreements, and refine these guidelines.
* **During annotation:** independently double-label a representative subset (for example, 10–15% of records), without seeing the other reviewer's choices.
* Calculate agreement (e.g., Cohen's kappa for pairs), discuss disagreements, and record adjudicated final labels.
* Keep a decision log of ambiguous cases and any guideline revisions.
* Review class balance and excluded records before training.

## Preparing the modeling dataset

* Combine reviewer files by PMID only after review is complete; check for duplicates and missing labels.
* Include only adjudicated Reviewed rows with one of the five topic labels in the primary topic classification dataset.
* Create reproducible **stratified train/validation/test splits** after labels are finalized. Use the **same splits** for baseline and transformer comparisons; avoid data leakage.
* Report the number of articles per class, exclusions, annotation agreement, and any unresolved cases.

## Limitations

These categories simplify overlapping scientific fields. A single primary label does not capture all dimensions of an article. Search retrieval can bias class coverage, and human annotation is imperfect. Models are for **research organization**, not diagnosis, treatment selection, or clinical decision-making.

## Team decision needed

The three annotation CSVs represent a **proposed** workload split, not a finalized assignment. Confirm reviewer responsibilities, double-labeling, and adjudication procedures with teammates before beginning bulk annotation.

