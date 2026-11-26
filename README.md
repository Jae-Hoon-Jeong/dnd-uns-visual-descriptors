# DND-UNS: Visual Descriptors

Visual descriptor generation materials for *Distribution-guided Nuclei Discovery for Annotation-free Nuclei Segmentation*.

## Models and generation settings

| Model | Identifier / interface |
|---|---|
| ChatGPT-4o | ChatGPT web interface, October 2025, as reported in the manuscript, which exposes no sampling parameter |
| Mistral | `mistralai/Mistral-7B-Instruct-v0.3` |
| Qwen | `Qwen/Qwen2.5-7B-Instruct` |

| Setting | Open models |
|---|---|
| Sampling | Enabled |
| Temperature | 1.0 |
| Top-p | 1.0 |
| Maximum new tokens for the short-phrase response | 64 |
| Queries per repetition | 40 |
| Repetitions per model and dataset | 5 |
| Sampling seeds | Included in the response files |

## Instructions

```
Please describe the typical visual characteristics of nuclei observed in an H&E stained <tissue> tissue slide at 40x magnification. Focus on providing concise English phrases that capture the nuclei's geometric shape, staining color, and relative size.
```

Open-model answer-format instruction:

```
Answer with exactly two short noun phrases, one per line, nothing else. Each phrase must end with the word 'nuclei' and combine shape, colour and size, in the form:
<size> <shape> <colour> nuclei
<size> <shape> <colour> nuclei
```

The tissue terms are `colon` (MoNuSeg and CoNSeP) and `breast` (TNBC).

## Final prompts

The two-descriptor prompts are:

| Model | Prompt |
|---|---|
| GPT-4o | nuclei. small elongated dark blue nuclei. small oval purple nuclei. |
| Mistral | nuclei. large oval blue nuclei. small round dark purple nuclei. |
| Qwen | nuclei. small round to oval blue nuclei. moderate oval light purple nuclei. |

The complete grid prompts for the repeated comparisons are provided below.

| Dataset | Final prompts | Recorded responses and seeds |
|---|---|---|
| MoNuSeg | [Prompts](prompts/MoNuSeg.json) | [Responses](responses/MoNuSeg/) |
| CoNSeP | [Prompts](prompts/CoNSeP.json) | [Responses](responses/CoNSeP/) |
| TNBC | [Prompts](prompts/TNBC.json) | [Responses](responses/TNBC/) |

The GPT colon grid prompt is in [GPT_colon.json](prompts/GPT_colon.json) and is reused for the TNBC comparison.

## Prompt composition

Count occurrences of shape, staining color, and relative size terms across responses, at most once per response. Rank terms by frequency with alphabetical tie-breaking. For the two-descriptor configuration, pair the top two terms of each attribute by rank. For the repeated PLA comparisons, combine the three most frequent shape terms with the two most frequent color terms to obtain six descriptors. Match longer color phrases before shorter ones and exclude cytoplasm-related spans when counting nuclear colors. Each final prompt begins with `nuclei.`.

## Contents

```
prompts/     the final GLIP prompt of every dataset and model
responses/   the recorded responses with the sampling seed of each one,
             30 files of 40 responses, grouped by dataset
```
