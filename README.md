# DualSeqNet: AI-Driven Protein Sequence & Metadata Generation project

## Project Overview
Welcome to the DualSeqNet project! This project aims to address a critical bottleneck in bioinformatics: the time-consuming process of annotating novel or synthetic protein sequences. Traditional sequence similarity search tools like BLAST and DIAMOND, while powerful for identifying homologous proteins and their associated metadata, can be computationally intensive and slow when dealing with massive datasets of newly generated sequences.

My goal is to develop an AI-driven pipeline that can generate biologically plausible protein sequences and then rapidly assign them emulated BLAST-like metadata (organism, protein name, description, and key similarity metrics) without performing a full sequence alignment search. This "virtual BLAST" will provide quick, insightful annotations, enabling faster exploratory analysis and high-throughput annotation for synthetic biology and protein engineering.

## The Task
The task is to build a robust and scalable AI pipeline that performs two primary functions:

* Protein Sequence Generation: Generate novel protein sequences that are biologically plausible and resemble sequences found in nature.

* AI-Driven Metadata Emulation: Given a protein sequence (either generated or real), predict its most likely associated metadata, emulating the output of a BLASTp or DIAMOND search against a comprehensive protein database. This prediction must be significantly faster than running an actual alignment tool.

## Project Goals
The successful solution will be able to:

* Generate diverse and plausible protein sequences: These sequences should exhibit characteristics similar to natural proteins, potentially with a degree of novelty.

* Accurately predict key metadata:

    * The most probable organism the protein would be found in (e.g., Homo sapiens, Escherichia coli K-12).

    * A plausible protein name and description (e.g., "Hypothetical uncharacterized protein", "Putative DNA binding protein").

    * A synthesized accession ID that follows a plausible format (e.g., SYNPROT_000001).

* Emulated BLAST-like metrics: Approximate E-value, Bit Score, and Percentage Identity, reflecting how a real BLAST/DIAMOND search might score the sequence against a large database.

* Achieve high inference speed: The metadata prediction step should be orders of magnitude faster than running a full BLASTp or DIAMOND search.

* Demonstrate scalability: The architecture should be designed to handle the generation and annotation of massive amounts of protein sequences.

## Proposed Workflow (Your Guiding Strategy)
We envision a multi-stage pipeline, with distinct components for sequence generation and metadata prediction.

### Stage 1: Protein Sequence Generation
* Objective: Create novel protein sequences that are biologically realistic.

* Approach: Leverage pre-trained Protein Language Models (PLMs).

    * Recommended Models: Consider models like ProtGPT2 (for its autoregressive generation capabilities) or fine-tuning other large PLMs (e.g., from the ESM family) if they offer controllable generation features.
    
    * Controllability: Explore methods to guide generation (e.g., conditioning on desired protein families, lengths, or functional properties) to ensure the generated sequences are relevant to "current knowledge."
    
    * Output: A large collection of generated protein sequences in FASTA format.

### Stage 2: Ground Truth Generation (for Training/Validation Data)
* Objective: Create a high-quality dataset of (protein sequence embeddings, associated metadata) pairs to train and validate the metadata prediction model. This is a one-time (or periodic) computationally intensive step, not part of the final rapid inference.

* Approach:

    * Select a comprehensive reference database: Use a large, well-annotated protein database like UniProtKB (Swiss-Prot for quality, TrEMBL for scale).
    
    * Generate Protein Embeddings: For each sequence in your reference database, generate its embedding using a powerful PLM encoder (e.g., ESM-2 embeddings are highly recommended for their rich biological information capture).
    
    * Run Sequence Alignment for Ground Truth: For a representative subset of sequences (or the entire database if resources allow), perform a similarity search using DIAMOND (highly recommended over BLASTp for its speed and comparable sensitivity) against the same reference database.
    
    * Extract Ground Truth Metadata: From the DIAMOND output for each query sequence's best hit, extract:
    
        * Best hit organism (scientific name, and ideally NCBI Taxonomy ID).
    
        * Best hit protein name and description.
    
        * Best hit accession ID.
    
    * Key numerical metrics: E-value, Bit Score, Percentage Identity, Query Coverage.
    
    * Store Data: Store these (embedding, metadata) pairs in an efficient tabular format (e.g., Parquet, HDF5) suitable for machine learning training.

### Stage 3: AI-Driven Metadata Prediction (The Core ML Challenge)
* Objective: Train a neural network that can rapidly predict the desired metadata from a protein sequence's embedding, bypassing the need for computationally expensive sequence alignment.

* Approach:

  * Model Architecture: Design a neural network that takes protein embeddings as input.
  
  * For Organism Prediction: A multi-class or multi-label classification head (e.g., fully connected layers with softmax/sigmoid activation) predicting the most likely organism(s) or taxonomic IDs. Consider hierarchical prediction for scalability.
  
  * For Protein Name/Description: A text generation model (e.g., a small Transformer decoder or an attention-based encoder-decoder) that generates a descriptive string.
  
  * For Accession ID: A component that synthesizes a unique, plausible ID (e.g., based on a predefined schema). Do not try to predict existing accession IDs.
  
  * For Emulated BLAST Metrics: Regression heads (e.g., fully connected layers) predicting numerical values for E-value (consider log-transforming), Bit Score, and Percentage Identity.
  
  * Training: Train this neural network using the ground truth dataset generated in Stage 2.

  * Validation & Testing:
      
      * Validate the model's performance on a held-out set of generated sequences (running DIAMOND on them to get ground truth for comparison).
      
      * Test the model on a separate held-out set of real protein sequences to ensure it generalizes well to known data.
  
  * Inference: Once trained, the inference process will involve:
  
      * Generating the embedding for an input protein sequence (fast).
  
      * Feeding the embedding into the trained metadata prediction NN (extremely fast).
      
      * Outputting the predicted metadata.

## Datasets
* Protein Sequences & Metadata: The primary source for training data will be UniProtKB. You will need to download and parse relevant sections (e.g., Swiss-Prot for high-quality annotations, TrEMBL for breadth).

* UniProt Downloads

* Protein Language Models: Access pre-trained models via the Hugging Face Transformers library.

* Hugging Face Models (search for "protein")

## Technical Considerations & Hints
* Protein Language Models (PLMs):

- ESM-2: Excellent for generating embeddings that capture rich biological information. These embeddings are crucial inputs for your metadata prediction NN.

- ProtGPT2 / ProGen: Strong candidates for generating novel protein sequences.

* Fine-tuning: Consider fine-tuning your chosen PLM on a specific subset of UniProt data to bias generation towards certain protein types or to improve embedding quality for your specific task.

## Libraries:

1) Python: The standard for this project.

2) Hugging Face Transformers: For easy access to PLMs.

3) PyTorch / TensorFlow / Keras: For building and training your neural networks.

4) Biopython: For parsing FASTA files and general sequence manipulation.

5) Pandas / NumPy: For data handling and numerical operations.

6) DIAMOND: Install and use the command-line tool for ground truth generation.

## Tips
* Scalability: Think about efficient data loading, batch processing, and potential parallelization for both ground truth generation and metadata inference. Cloud computing resources can be very beneficial for large-scale training data generation.

* Text Generation for Descriptions: Generating coherent and biologically accurate protein descriptions from embeddings is a challenging NLP task. Start simple and iterate. You might need to experiment with different decoder architectures or even fine-tune a small general-purpose LLM on protein descriptions.

* Taxonomy: Use NCBI Taxonomy IDs for organisms to ensure consistency and hierarchical understanding.

* Evaluation: Carefully define how you will evaluate the "plausibility" and "accuracy" of your generated metadata, especially for the text-based descriptions and numerical BLAST metrics.

## Getting Started
Clone this repository:

```
git clone https://github.com/MartinAmiens/DualSeqNet.git
cd DualSeqNet
```

Set up your environment: Create a virtual environment and install necessary libraries.

Start coding! Begin by exploring existing PLMs and planning your data collection strategy.

## Contribution & Community
This is an open project, and I encourage collaboration and diverse approaches. Feel free to open issues for questions, discuss ideas, and submit your solutions!

Good luck, and happy coding!
