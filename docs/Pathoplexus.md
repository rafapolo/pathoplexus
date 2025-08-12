# Pathoplexus: Specialized Genomic Database for Human Viral Pathogens

Pathoplexus is a curated, specialized genomic database for human viral pathogens of public health importance. It supports comprehensive genomic, metadata, and bioinformatics resources, with a focus on open data, interoperability, and high-quality analytical pipelines.

## Dataset Types and Scope

### Supported Organisms / Pathogens

**Ebola viruses**
- Ebola Zaire (Zaire ebolavirus)
- Ebola Sudan (Sudan ebolavirus)

**West Nile Virus** (West Nile virus)

**Crimean-Congo Hemorrhagic Fever Virus (CCHF)** (Orthonairovirus haemorrhagiae)
- Multi-segment genome: L, M, S segments

**Mpox Virus** (Monkeypox virus)

**Respiratory Syncytial Viruses (RSV)**
- RSV-A (Human respiratory syncytial virus A)
- RSV-B (Human respiratory syncytial virus B)

**Human Metapneumovirus (HMPV)**

### Data Types

**Genomic Data**
- Complete viral genome sequences (FASTA format)
- Gene-level sequences for each pathogen
- Reference genomes with INSDC accessions
- Multi-segment genomes (e.g., CCHF: L, M, S segments)

**Metadata**
- Sample Metadata: Collection date, location, host information
- Clinical Metadata: Symptoms, vaccination status, prior infections
- Laboratory Metadata: Sequencing methods, QC metrics
- Epidemiological Data: Exposure details, specimen processing
- Submission Tracking: Submitter info, group affiliations

## Bioinformatics Tools and Models

### Core Analysis Pipeline

**Primary Tool: Nextclade**
- Sequence QC and validation
- Genome alignment against reference sequences
- Phylogenetic clade assignment and lineage classification
- Mutation detection (substitutions, insertions, deletions)
- Stop codon identification and frameshift detection

### Analysis Capabilities

**Quality Control Metrics:** Substitutions, insertions, deletions, frameshifts, missing nucleotides

**Phylogenetic Analysis:** Clade assignment with Nextstrain datasets

**Lineage Systems:** Hierarchical classification for each organism

**Geographic Mapping:** Spatial visualization of sequence data

**Comparative Analysis:** Cross-environment data integrity checks

### Open Models & Standards

**Open Source:** Built on Loculus

**Standard Formats:** FASTA, INSDC accessions

**Interoperability:** NCBI, ENA submission pipeline integration

**Open Data:** Configurable data use terms, publicly accessible sequences

## Technical Infrastructure

**Database:** PostgreSQL for metadata and sequence storage

**Search Engine:** LAPIS/SILO for high-performance sequence search

**Processing:** Snakemake workflows for QC and data processing

**APIs:** RESTful endpoints for programmatic access to data & metadata

Note: Pathoplexus is not a machine learning or AI-based platform. It is a specialized genomic repository leveraging established bioinformatics tools—primarily Nextclade and Nextstrain—for sequence analysis and phylogenetic classification of public health–relevant viral pathogens.