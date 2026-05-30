# Qwen3 Proxy Label Fine-Tuning for Candidate Matching

This project fine-tunes a small Qwen language model to classify candidate profiles into recruiting match labels:

- `low_match`
- `medium_match`
- `high_match`

The notebook is designed to run in Google Colab with a GPU runtime. It uses supervised fine-tuning with QLoRA, making it practical to train an adapter even with limited GPU memory.

## Project Goal

The goal is to train an LLM that can read basic candidate information, such as job title and location, and predict a proxy match label for recruiting or talent screening tasks.

This project is part of my learning path in applied AI, data science, and LLM fine-tuning.

## Notebook

Main notebook:

```text
.ipynb_checkpoints/Qwen3_Proxy_Labels_Colab.ipynb
```

The notebook includes:

1. Installing required libraries
2. Uploading the Excel dataset in Colab
3. Building train, validation, and test JSONL datasets
4. Fine-tuning `Qwen/Qwen3Guard-Gen-0.6B` with QLoRA
5. Optional troubleshooting for `torchao`
6. Testing the fine-tuned model on a sample candidate

## Model and Fine-Tuning Method

Base model:

```text
Qwen/Qwen3Guard-Gen-0.6B
```

Fine-tuning approach:

- Supervised Fine-Tuning using TRL `SFTTrainer`
- LoRA adapters using PEFT
- 4-bit quantization using BitsAndBytes
- GPU training in Google Colab

This approach trains only a small adapter instead of updating the full model, which reduces memory usage and makes the workflow more suitable for Colab.

## Dataset Format

The notebook expects an Excel file named:

```text
Potential_Talents_Proxy_Labels.xlsx
```

Required columns:

| Column | Description |
| --- | --- |
| `id` | Candidate identifier |
| `title` | Candidate job title or profile title |
| `location` | Candidate location |
| `screening_score` | Numeric screening score |
| `match_label` | Target label: `low_match`, `medium_match`, or `high_match` |

The notebook converts each row into a chat-style supervised fine-tuning example with:

- A system instruction
- A user prompt containing candidate information
- An assistant response containing the target label

## Label Mapping

The proxy labels are based on screening score ranges:

| Score Range | Label |
| --- | --- |
| `0-30` | `low_match` |
| `31-70` | `medium_match` |
| `71-100` | `high_match` |

## Requirements

The notebook installs or uses the following main libraries:

```text
transformers>=4.57.0
trl>=0.24.0
peft>=0.17.0
accelerate>=1.10.0
datasets>=4.0.0
bitsandbytes>=0.47.0
openpyxl>=3.1.0
```

Recommended environment:

- Google Colab
- GPU runtime
- Python 3

## How to Run

1. Open the notebook in Google Colab.
2. Switch the runtime to GPU:

```text
Runtime -> Change runtime type -> GPU
```

3. Run the install cell if the required packages are missing.
4. Upload `Potential_Talents_Proxy_Labels.xlsx` when prompted.
5. Run the dataset preparation cell.
6. Run the fine-tuning cell.
7. Test the trained adapter using the inference cell.

## Output

The notebook saves the final LoRA adapter to:

```text
/content/qwen3_proxy_labels/final_adapter
```

The saved adapter can be reused later with the same base model and tokenizer for candidate classification.

## Example Prediction Workflow

The testing cell scores each possible label:

```text
low_match
medium_match
high_match
```

The model then selects the label with the highest log-probability score.

This avoids free-text generation and keeps the output restricted to the allowed classification labels.

## Notes

- If Colab runs out of memory, reduce `MAX_LENGTH`.
- If training is still too heavy, reduce batch size or number of epochs.
- Keep private candidate data out of public GitHub repositories.
- If you publish this repository, consider adding a small sample dataset instead of the full private Excel file.

## Author

Ibrahim Chaoudi  
Data Scientist

- GitHub: [Ch-Ibrahim](https://github.com/Ch-Ibrahim)
- LinkedIn: [ibrahimchaoudi](https://www.linkedin.com/in/ibrahimchaoudi/)
- X: [@i_chaoudi_](https://x.com/i_chaoudi_)
- Email: [brahiml_chaoudi@yahoo.com](mailto:brahiml_chaoudi@yahoo.com)

## License

This project is for learning, experimentation, and portfolio development. Add a license file before using it in production or sharing it as an open-source project.
