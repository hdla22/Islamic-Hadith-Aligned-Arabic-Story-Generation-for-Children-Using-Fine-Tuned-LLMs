# Islamic Hadith-Aligned Arabic Story Generation for Children Using Fine-Tuned LLMs

This repository contains the code and resources for our research project on building an **end-to-end Arabic storytelling system for children** that generates culturally grounded, Islamic, and Hadith-aligned narratives using fine-tuned large language models.

## Overview

We fine-tune three modern instruction-tuned Arabic-capable LLMs — **Qwen2.5-7B**, **Llama-3-8B**, and **ALLaM-7B** — using QLoRA on a structured Modern Standard Arabic children's story dataset. Each generated story is then paired with an authentic Hadith through a multi-stage retrieval pipeline that combines dense semantic search, cross-encoder reranking, and Arabic-aware deduplication. The final system integrates story generation, Hadith alignment, image illustration, and Arabic narration into a unified multimodal framework.

## System Components

The pipeline consists of four main stages:

1. **Story Generation** — Three Arabic LLMs fine-tuned via QLoRA produce instruction-aligned, age-appropriate stories that conclude with an explicit moral.
2. **Hadith Retrieval** — A multi-stage pipeline (BGE-M3 dense retrieval → BGE-reranker cross-encoder → Arabic-aware deduplication) retrieves the most contextually appropriate Hadith for each story.
3. **Human Evaluation** — Domain experts (Islamic studies and Arabic linguistics) rate stories on five criteria (Fluency, Coherence, Following Instructions, Consistency, Variety) and rate the relevance of the top-3 retrieved Hadiths.
4. **Multimodal Output** — Comic-style illustrations, automatic Arabic narration, and background ambience are generated for each story.

## Repository Contents

### Notebooks

| File | Description |
|------|-------------|
| `multimodal_ALLaM.ipynb` | End-to-end multimodal generation notebook: story generation with ALLaM + Hadith retrieval + image synthesis + Arabic TTS narration. |
| `retriever_per_model.ipynb` | Standalone Hadith retrieval pipeline (BGE-M3 + cross-encoder + deduplication). Runs over pre-generated stories from all three models and outputs the top-10 Hadith candidates per story. |

### Datasets

| File | Description |
|------|-------------|
| `Formatted-MSA-Dataset.csv` | The Modern Standard Arabic children's story corpus (996 instruction–story pairs), used to fine-tune the three LLMs. |
| `hadith_dataset.zip` | The authenticated Hadith corpus (34,433 narrations) drawn from the six canonical Sunni collections (Sahih al-Bukhari, Sahih Muslim, Sunan an-Nasa'i, Sunan Abi Dawud, Sunan Ibn Majah, Jami' al-Tirmidhi), along with its pre-computed BGE-M3 embeddings. |

### Generated Outputs

| File | Description |
|------|-------------|
| `Generators.zip` | Per-model generation scripts used to produce the evaluation stories. |
| `Generator_results.zip` | The generated stories from each fine-tuned model (Qwen, Llama, ALLaM), saved as CSV files. Each row contains the input attributes, generated story, extracted moral, and retrieval queries. |
| `Retriever_results.zip` | The top-10 retrieved Hadiths for each generated story (per model), under two conditions: with and without an LLM-generated story summary in the query. |

## Fine-Tuned Model Adapters

Each adapter was trained on the same MSA children's story corpus using identical QLoRA hyperparameters (4-bit NF4 quantization, LoRA rank 32, learning rate 4×10⁻⁵, 2 epochs). The adapters attach to the q/k/v/o attention projections of the base models, which are available on HuggingFace:
- `humain-ai/ALLaM-7B-Instruct-preview`
- `Qwen/Qwen2.5-7B-Instruct`
- `meta-llama/Meta-Llama-3-8B-Instruct`

## Citation

If you use this work, please cite our paper:

```bibtex
@misc{alshutayri2026islamic,
  title  = {Islamic Hadith-Aligned Arabic Story Generation for Children Using Fine-Tuned LLMs},
  author = {Alshutayri, Areej and Almehmadi, Aseel and Miran, Rawnaa and Abdulhadi, Hadeel and Almaashi, Fatima},
  year   = {2026},
}
```
