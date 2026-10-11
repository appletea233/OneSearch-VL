<div align="center">

<img src="images/logo.png" alt="OneSearch-VL" width="320">

<h2>Unified Multimodal Deep Research Agent for Image and Video</h2>

<p><b>Unified Tool Set &nbsp;·&nbsp; VGEG-Centered Data Engine &nbsp;·&nbsp; Rubric Reward &nbsp;·&nbsp; Multi-Image / Video Benchmarks</b></p>

[![Paper](https://img.shields.io/badge/paper-A42C25?style=for-the-badge&logo=arxiv&logoColor=white)](https://arxiv.org/abs/2610.12419)
[![Discussion](https://img.shields.io/badge/discussion-A42C25?style=for-the-badge&logo=arxiv&logoColor=white&color=blue)](https://github.com/appletea233/OneSearch-VL)
[![GitHub](https://img.shields.io/badge/OneSearch_VL-000000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/appletea233/OneSearch-VL)
[![Hugging Face Collection](https://img.shields.io/badge/OneSearch_VL_Collection-fcd022?style=for-the-badge&logo=huggingface&logoColor=000)](https://huggingface.co/OneSearch-VL)
[![Twitter](https://img.shields.io/badge/Twitter-%23000000.svg?style=for-the-badge&logo=twitter&logoColor=white)](https://github.com/appletea233/OneSearch-VL)

[![Awesome](https://awesome.re/badge.svg)](https://github.com/appletea233/OneSearch-VL)
[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![Python](https://img.shields.io/badge/Python-3.11%2B-blue.svg)](SFT/README.md#1-install)
[![Last commit](https://img.shields.io/github/last-commit/appletea233/OneSearch-VL?color=green)](https://github.com/appletea233/OneSearch-VL/commits/main)

</div>

## 📑 Table of Contents

- [📖 Introduction](#introduction)
- [🗺️ Overview](#overview)
- [🍭 Method Overview](#method-overview)
- [📊 OneSearch Benchmarks](#benchmarks)
- [🏆 Main Results](#main-results)
- [🔎 Case Study](#case-study)
- [🤗 Models & Datasets](#models-and-datasets)
- [📁 Repository Layout](#repository-layout)
- [🛠️ Getting Started](#getting-started)
- [🏋️ Agentic SFT](#agentic-sft)
- [🚀 Agentic RL](#agentic-rl)
- [🔬 Inference & Evaluation](#inference-and-evaluation)
- [🙌 Acknowledgements](#acknowledgements)
- [📄 Citation](#citation)
- [⚖️ License](#license)

---

<a id="introduction"></a>

## 📖 Introduction

**OneSearch-VL** is a unified multimodal deep research agent for **single images,
multi-image collections, and videos**. It locates visual clues, retrieves external
knowledge, and combines evidence into supported answers.

At its core is the **Visually Grounded Evidence Graph (VGEG)**, which connects
visual anchors, source-supported facts, and answer-producing operations. This
shared structure supports three parts of our approach:

- **Data construction:** a VGEG-centered data engine generates and verifies
  research questions and expert tool-use trajectories.
- **Evidence supervision:** the **Evidence-aware Visual-Grounded Rubric reward
  (EVGR)** supervises evidence traceability and visual grounding during RL.
- **Evaluation:** **OneSearch-MI-Bench** and **OneSearch-Video-Bench** evaluate
  how agents retrieve and compose evidence across six research operations.

---

<a id="overview"></a>

## 🗺️ Overview

Starting from **Qwen3-VL-8B**, OneSearch-VL learns a shared research policy through
joint SFT and RL across all three visual input types. The agent combines visual
inspection, image and text search, and video frame selection in a common tool
environment. VGEGs provide supervision and evaluation references; the agent
receives the visual input, question, and tools and gathers its own evidence.

This repository provides [SFT](SFT/README.md), [RL](RL/README.md), and
[inference and evaluation](onesearch_vl/README.md) code. Model weights and data
are available under [OneSearch-VL on Hugging Face](https://huggingface.co/OneSearch-VL).

---

<a id="method-overview"></a>

## 🍭 Method Overview

### VGEG-Centered Multimodal Data Engine

Our data engine discovers visual anchors, connects them to verified web facts,
and constructs compositional questions with explicit evidence dependencies.
Expert trajectories are filtered for answer correctness and process quality.
Together with adapted single-image data from
[OpenSearch-VL](https://github.com/shawn0728/OpenSearch-VL), this produces
**OneSearch-VL-SFT-110K** and **OneSearch-VL-RL-10K** for unified training.

![Overview of the VGEG-centered multimodal data engine](images/data_pipeline.png)

*VGEG-centered data construction, from visual sources to verified research tasks and expert trajectories.*

### Evidence-aware Visual-Grounded Rubric Reward (EVGR)

EVGR uses VGEG-derived rubrics to evaluate whether a rollout obtains the required
facts (**evidence traceability**) and uses the correct images, regions, or video
frames (**visual grounding**). Combined with answer and query rewards, it
encourages the agent to ground its answers in the evidence it actually gathers.

![Overview of the Evidence-aware Visual-Grounded Rubric reward](images/evgr.png)

*EVGR supervises evidence traceability and visual grounding during reinforcement learning.*

---

<a id="benchmarks"></a>

## 📊 OneSearch Benchmarks

[**OneSearch-MI-Bench**](https://huggingface.co/datasets/OneSearch-VL/OneSearch-MI-Bench)
contains **301 multi-image questions**, and
[**OneSearch-Video-Bench**](https://huggingface.co/datasets/OneSearch-VL/OneSearch-Video-Bench)
contains **307 video questions**. Both organize evaluation around six research
operations: single-anchor lookup, multi-hop retrieval, knowledge-conditioned
count, multi-anchor join, arithmetic, and comparison.

![Operation-oriented design of the OneSearch benchmarks](images/benchmarks.png)

*The OneSearch benchmarks cover six operations for retrieving and composing visual and web evidence.*

Multi-image tasks use unordered image sets with stable visual references; video
tasks retain temporal structure. The dataset cards provide media downloads and
preparation instructions.

---

<a id="main-results"></a>

## 🏆 Main Results

- **Single-image research:** OneSearch-VL-8B achieves **58.3 average accuracy**
  across seven benchmarks, improving over OpenSearch-VL-8B by **1.7 points**.
- **Multi-image and video research:** compared with **Qwen3-VL-8B with tools**,
  it improves accuracy by **20.2 points** on OneSearch-MI-Bench, **17.6 points**
  on OneSearch-Video-Bench, and **27.0 points** on VideoDR.
- **Ablations:** joint SFT achieves the best average in the data-mixture ablation. Full EVGR adds
  **3.8 points** over answer and query rewards on the paper's six-benchmark
  ablation average.

All gains are in percentage points. See the [paper](https://arxiv.org/abs/2610.12419) for detailed comparisons.

---

<a id="case-study"></a>

## 🔎 Case Study

In this multi-image example, the agent identifies **Ford** and **Brembo** from
separate visual anchors, retrieves their founding years, and answers that the
manufacturers were founded **58 years** apart.

![Multi-image arithmetic: grounding Ford and Brembo before comparing founding years](images/case_mi_arithmetic.png)

*Multi-image arithmetic: visual grounding, external fact retrieval, and answer composition.*

---

<a id="models-and-datasets"></a>

## 🤗 Models & Datasets

- [**OneSearch-VL-8B**](https://huggingface.co/OneSearch-VL/OneSearch-VL-8B): our
  unified agent for single-image, multi-image, and video deep research.
- [**OneSearch-VL-SFT-110K**](https://huggingface.co/datasets/OneSearch-VL/OneSearch-VL-SFT-110K):
  approximately 110K expert trajectories for joint supervised fine-tuning.
- [**OneSearch-VL-RL-10K**](https://huggingface.co/datasets/OneSearch-VL/OneSearch-VL-RL-10K):
  approximately 10K tasks for reinforcement learning.

For setup, follow the [SFT data preparation guide](SFT/README.md#2-prepare-data)
and [RL registration guide](RL/README.md#3-register-rl-10k). Benchmark downloads
are linked in [OneSearch Benchmarks](#benchmarks).

---

<a id="repository-layout"></a>

## 📁 Repository Layout

```text
OneSearch-VL/
├── images/                         # README assets and project logo
├── SFT/                            # Joint agentic supervised fine-tuning
│   ├── examples/onesearch/          # Qwen3-VL-8B SFT-110K recipe
│   ├── data/dataset_info.json       # Three modality dataset registrations
│   ├── scripts/relocate_media.py    # Resolve downloaded media paths
│   ├── run_train.sh
│   └── README.md
├── RL/                             # Asynchronous agentic reinforcement learning
│   ├── rllm/                       # rLLM, verl, rollout workflow, and launchers
│   ├── Megatron-LM/                 # Policy training backend
│   ├── mbridge/                     # Hugging Face ↔ Megatron bridge
│   ├── register_dataset.py
│   └── README.md
├── onesearch_vl/                    # Shared visual tools, inference, and evaluation
│   ├── onesearch_infer/             # Prompts, tools, model backends, and agent loop
│   ├── run_infer.py                 # Unified modality entry point
│   ├── run_eval.sh                  # Benchmark evaluation
│   └── README.md
├── scripts/                         # Source verification and release export
├── THIRD_PARTY_NOTICES.md
├── LICENSE
└── README.md
```

---

<a id="getting-started"></a>

## 🛠️ Getting Started

```bash
git clone https://github.com/appletea233/OneSearch-VL.git
cd OneSearch-VL
```

Run the commands below from the **repository root**. Paths such as `/data/...`
and `/models/...` are placeholders for your local datasets and checkpoints.
Use a separate environment for each component; installation commands and
backend requirements are documented in the linked guides.

| Component | Requirements | Installation |
|---|---|---|
| SFT | Python 3.11+, compatible PyTorch / CUDA / DeepSpeed | [SFT setup](SFT/README.md#1-install) |
| RL | SGLang 0.5.5 and PyTorch 2.8.0 via the bundled extra; compatible Transformer Engine | [RL setup](RL/README.md#1-install) |
| Inference | Inference Python dependencies; `ffmpeg` for video; GPU for local model inference | [Inference setup](onesearch_vl/README.md#1-install) |

### Configure External Services

Copy the templates for the components you use, then fill in your model,
retrieval, judge, and media-publication settings:

```bash
cp RL/.env.example RL/.env
cp onesearch_vl/.env.example onesearch_vl/.env
```

RL launchers load `RL/.env`; inference and evaluation shell wrappers load
`onesearch_vl/.env`. Both accept `ENV_FILE` to select a different file. When
running inference Python entry points directly, export the settings first:

```bash
set -a
source onesearch_vl/.env
set +a
```

See the [service configuration guide](onesearch_vl/README.md#2-configure-services)
for the required providers and credentials. Local `.env` files are ignored by Git.

---

<a id="agentic-sft"></a>

## 🏋️ Agentic SFT

Download [SFT-110K](https://huggingface.co/datasets/OneSearch-VL/OneSearch-VL-SFT-110K),
extract its media archives into their modality directories, then prepare the
registered training files and launch:

```bash
python SFT/scripts/relocate_media.py \
  --base-dir /data/OneSearch-VL-SFT-110K \
  --output-dir SFT/data

bash SFT/run_train.sh \
  model_name_or_path=/models/Qwen3-VL-8B-Instruct \
  output_dir=/checkpoints/onesearch-sft
```

The recipe uses learning rate `1e-5`, eight epochs, and DeepSpeed ZeRO-2.
The vision tower and multimodal projector remain frozen.

> Dataset layout, installation, training parameters, and multi-node setup:
> [SFT/README.md](SFT/README.md).

---

<a id="agentic-rl"></a>

## 🚀 Agentic RL

Download the prepared [RL-10K bundle](https://huggingface.co/datasets/OneSearch-VL/OneSearch-VL-RL-10K),
register the train split, and launch from your SFT checkpoint:

```bash
DATA_ROOT=/data/OneSearch-VL-RL-10K \
  bash RL/rllm/vision_deepresearch_async_workflow/data_prepare/register_rl_dataset.sh \
  --media-root /data/OneSearch-VL-RL-10K

# Set MODEL_PATH, OUTPUT_DIR, FORMAL_EVGR_DATA_ROOT, JUDGE_API_KEY,
# SEED_API_BASE, and SEED_API_KEY in RL/.env.
# For the path above, set FORMAL_EVGR_DATA_ROOT to the same data directory.
bash RL/rllm/vision_deepresearch_async_workflow/run/qwen3-vl-8b-single-node.sh
```

The released launcher uses the RLOO advantage estimator, eight responses per prompt, prompt batch `256`,
minibatch `64`, and `201` training steps. Single-node and multi-node launchers default
to 1 × 8 and 4 × 8 GPUs respectively. The released recipe is train-only.

> Installation, dataset registration, Ray setup, rewards, and multi-node launch:
> [RL/README.md](RL/README.md).

---

<a id="inference-and-evaluation"></a>

## 🔬 Inference & Evaluation

Use the published [OneSearch-VL-8B checkpoint](https://huggingface.co/OneSearch-VL/OneSearch-VL-8B)
with the local `8b` backend. Install the inference dependencies and export your
service settings as described in [Getting Started](#getting-started).
For the OneSearch benchmarks, run the dataset's `prepare.py` first to generate
its `runtime/` inputs.

```bash
# Single image: supply a supported parquet benchmark.
python onesearch_vl/run_infer.py --modality single_image \
  --model 8b --checkpoint OneSearch-VL/OneSearch-VL-8B \
  --data-path /data/single_image.parquet --output-dir ./outputs/single_image

# OneSearch-MI: preserve the image-index mapping in the prepared manifest.
python onesearch_vl/run_infer.py --modality multi_image \
  --model 8b --checkpoint OneSearch-VL/OneSearch-VL-8B \
  --csv /data/OneSearch-MI-Bench/runtime/mi.csv \
  --manifest /data/OneSearch-MI-Bench/runtime/mi_manifest.jsonl \
  --output-dir ./outputs/onesearch_mi

# OneSearch-Video: use the prepared video and original-resolution frame manifest.
python onesearch_vl/run_infer.py --modality video \
  --model 8b --checkpoint OneSearch-VL/OneSearch-VL-8B \
  --csv /data/OneSearch-Video-Bench/runtime/video.csv \
  --manifest /data/OneSearch-Video-Bench/runtime/video_manifest.jsonl \
  --output-dir ./outputs/onesearch_video
```

For an HTTP model server, use `--model vllm` and configure `VLLM_API_BASE` and
`VLLM_MODEL` instead of passing a local checkpoint. The public Claude Messages
API is also supported; see [model backends](onesearch_vl/README.md#model-backends).

Evaluate the saved trajectories with the corresponding benchmark inputs:

```bash
bash onesearch_vl/run_eval.sh image \
  --traj_dir ./outputs/onesearch_mi \
  --csv /data/OneSearch-MI-Bench/runtime/mi.csv

bash onesearch_vl/run_eval.sh video \
  --traj_dir ./outputs/onesearch_video \
  --csv /data/OneSearch-Video-Bench/runtime/video.csv
```

Single-image and multi-image evaluation default to the `onesearch` protocol;
OneSearch-Video and VideoDR default to `videodr`. Both use `JUDGE_MODEL`
(default: `gpt-4o`).

> Input schemas, video preparation, model backends, and judge protocols:
> [onesearch_vl/README.md](onesearch_vl/README.md).

---

<a id="acknowledgements"></a>

## 🙌 Acknowledgements

This project builds on [OpenSearch-VL](https://github.com/shawn0728/OpenSearch-VL),
[LLaMA-Factory](https://github.com/hiyouga/LLaMA-Factory),
[rLLM](https://github.com/rllm-org/rllm), [verl](https://github.com/volcengine/verl),
[Megatron-LM](https://github.com/NVIDIA/Megatron-LM),
[mbridge](https://github.com/ISEEKYAN/mbridge),
[sglang](https://github.com/sgl-project/sglang), and
[Qwen-VL](https://github.com/QwenLM/Qwen3-VL). The bundled frameworks include
project modifications used by the training and inference workflows.

We also thank the authors of [Search-R1](https://github.com/PeterGriffinJin/Search-R1)
and [Vision-DeepResearch](https://github.com/Alibaba-NLP/Vision-DeepResearch)
whose ideas inspired our multi-turn search-augmented RL formulation.

---

<a id="citation"></a>

## 📄 Citation

**[OneSearch-VL: Unified Multimodal Deep Research Agent for Image and Video](https://arxiv.org/abs/2610.12419).**

```bibtex
@article{li2026onesearch,
  title={OneSearch-VL: Unified Multimodal Deep Research Agent for Image and Video},
  author={Li, Hongyu and Zhang, Manyuan and Feng, Kaituo and Chen, Shu and Zheng, Dian and Li, Hao and Yu, Hao and Chen, Zhangquan and Guo, Zoey and Zhang, Ray and others},
  journal={arXiv preprint arXiv:2610.12419},
  year={2026}
}
```

---

<a id="license"></a>

## ⚖️ License

This repository is released under the [Apache License 2.0](LICENSE).
See [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md) for bundled components,
which retain their licenses and copyright notices.
