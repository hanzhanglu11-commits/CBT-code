# CBT-Code

This repository contains the datasets and Colab notebooks used to evaluate Base, fine-tuned (FT), retrieval-augmented generation (RAG), and combined fine-tuned with retrieval-augmented generation (FT+RAG) model configurations for CBT-oriented dialogue generation.

## Files

| File                                                        | Description                                                                                                                                                                                             |
| ----------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `00_prepare_model_assets.ipynb`                             | Downloads and prepares the base Qwen2.5-32B-Instruct model, the CBT LoRA adapter, and other model assets required by the evaluation notebooks.                                                          |
| `01_single_turn_base_ft_evaluation.ipynb`                   | Generates and evaluates Base and FT responses for 100 single-turn CBT test cases. It includes BLEU, ROUGE, LLM-as-a-Judge, response-length, and KL-divergence analyses.                                 |
| `02_build_rag_index.ipynb`                                  | Loads the CBT retrieval corpus, generates corpus embeddings, and prepares the dense retrieval and reranking pipeline used by the RAG configurations.                                                    |
| `03_automated_multiturn_base_ft_evaluation.ipynb`           | Generates and evaluates automated multi-turn dialogues for the Base and FT configurations across 12 predefined scenarios. It includes dialogue-level LLM-as-a-Judge and pairwise evaluation.            |
| `04_adversarial_safety_base_ft_evaluation.ipynb`            | Evaluates Base and FT responses to 40 adversarial clinical-safety prompts using automated moderation and an eight-dimension clinical-safety rubric.                                                     |
| `05_ft_rag_evaluation.ipynb`                                | Generates and evaluates FT+RAG responses in the single-turn, automated multi-turn, and adversarial safety experiments.                                                                                  |
| `06_rag_only_evaluation.ipynb`                              | Generates and evaluates RAG-only responses in the single-turn, automated multi-turn, and adversarial safety experiments.                                                                                |
| `07_researcher_interactive_multiturn.ipynb`                 | Contains the researcher-interactive multi-turn experiment across five scenarios and the evaluation of complete dialogues from the four model configurations.                                            |
| `08_cbt_chat_ui.ipynb`                                      | Implements an interactive CBT dialogue interface and provides functions for exporting multi-turn dialogues for inspection.                                                                              |
| `09_recompute_single_turn_rag_judge_temperature_0.ipynb`    | Recomputes the single-turn LLM-as-a-Judge scores for the **RAG-only model** over the fixed set of 100 responses using Claude Sonnet 4.5 with `temperature=0`.                                           |
| `10_recompute_single_turn_ft_rag_judge_temperature_0.ipynb` | Recomputes the single-turn LLM-as-a-Judge scores for the **FT+RAG model** over the fixed set of 100 responses using Claude Sonnet 4.5 with `temperature=0`.                                             |
| `11_recompute_single_turn_lexical_metrics.ipynb`            | Recomputes BLEU, ROUGE-1, ROUGE-2, and ROUGE-L for all four model configurations. This notebook adds the previously missing RAG lexical-overlap results and does not perform LLM-as-a-Judge evaluation. |
| `cbt_synthetic_multiturn_dataset_2000.json`                 | Contains the 2,000 synthetic multi-turn CBT-oriented dialogues used for CBT-specific fine-tuning.                                                                                                       |




## Authoritative Results and Result Provenance

The outputs stored in some earlier notebooks were generated during preliminary or exploratory evaluation runs. Therefore, they may not exactly match the final values reported in the dissertation.

For the final single-turn results, the following notebooks should be treated as the authoritative sources:

* The final **RAG-only LLM-as-a-Judge scores** are determined by `09_recompute_single_turn_rag_judge_temperature_0.ipynb`.
* The final **FT+RAG LLM-as-a-Judge scores** are determined by `10_recompute_single_turn_ft_rag_judge_temperature_0.ipynb`.
* The final **BLEU and ROUGE scores for all four model configurations** are determined by `11_recompute_single_turn_lexical_metrics.ipynb`.
* The final **Base and FT LLM-as-a-Judge scores** are taken from `01_single_turn_base_ft_evaluation.ipynb`.

The RAG-only and FT+RAG judge scores were recomputed over the same fixed set of 100 single-turn responses using Claude Sonnet 4.5, the same evaluation prompt and scoring rubric, and `temperature=0`.

If any cached output in an earlier notebook differs from the values produced by the corresponding recomputation notebook, the recomputed values take precedence. The dissertation tables and reported conclusions use these authoritative recomputed results.

## Recomputed Single-Turn LLM-as-a-Judge Results

The RAG-only and FT+RAG single-turn scores reported in the dissertation use the controlled recomputations in notebooks `09` and `10`. These values supersede the preliminary cached judge outputs that may appear in the earlier evaluation notebooks.

| Evaluation dimension     |   RAG | FT+RAG |
| ------------------------ | ----: | -----: |
| CBT accuracy             | 7.800 |  7.910 |
| Empathy                  | 7.930 |  8.380 |
| Relevance                | 9.450 |  9.400 |
| Clinical appropriateness | 8.870 |  9.390 |
| Overall                  | 8.512 |  8.770 |

The RAG values are produced by `09_recompute_single_turn_rag_judge_temperature_0.ipynb`, while the FT+RAG values are produced by `10_recompute_single_turn_ft_rag_judge_temperature_0.ipynb`.
