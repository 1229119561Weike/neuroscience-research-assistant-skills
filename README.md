# Neuroscience Research Assistant Skill

> An AI-powered, end-to-end research assistant for neuroscientists — from literature discovery to computational simulation and interactive visualization.

[![Flowith OS](https://img.shields.io/badge/Flowith%20OS-Skill%20v1.0.0-blue)](https://flowith.io)
[![Python](https://img.shields.io/badge/Python-3.9%2B-green)](https://python.org)
[![License](https://img.shields.io/badge/License-MIT-yellow)](LICENSE)

---

## Overview

The **Neuroscience Research Assistant** is a Flowith OS Skill that integrates four tightly coupled research modules into a single, conversational workflow. Researchers can move fluidly from automated literature mining through imaging preprocessing, computational neural modeling, and publication-quality visualization — without leaving the AI assistant interface.

This skill was designed with inspiration from the [BioClaw](https://github.com/Runchuan-BU/BioClaw) project, adapting its philosophy of modular, reproducible neuroscience tooling for the Flowith OS skill protocol.

---

## Core Features

### 1. Literature Mining via PubMed
- Automated query construction and batch retrieval using the NCBI E-utilities API
- Structured abstract extraction: title, authors, year, journal, PMID
- NLP-based brain region co-occurrence analysis across retrieved abstracts
- Output: ranked literature tables, co-occurrence heatmaps, and structured Markdown reports

### 2. fMRI / EEG Preprocessing Pipelines
**fMRI (Nilearn)**
- Atlas-based ROI signal extraction (AAL-90, Schaefer-200, Destrieux)
- Bandpass filtering, confound regression, and nuisance removal
- Functional connectivity matrix computation (Pearson correlation, partial correlation, tangent)
- Connectome and glass-brain visualization

**EEG / MEG (MNE-Python)**
- Raw data loading from `.fif`, `.edf`, `.bdf`, `.set`, `.vhdr` formats
- Bandpass + notch filtering, bad channel interpolation, average re-referencing
- ICA-based artifact removal (EOG, ECG)
- Epoch segmentation, ERP component extraction, Morlet wavelet time-frequency analysis

### 3. TVB (The Virtual Brain) Computational Simulation
- Whole-brain network dynamics using structural connectivity (Hagmann 76-node connectome)
- Neural mass model selection: Wilson-Cowan, Kuramoto, Hindmarsh-Rose, Generic2dOscillator
- BOLD signal simulation (TR-matched monitors) and LFP proxy output
- Parameter sweep protocols: global coupling strength (`G`) vs. conduction speed
- Pharmacological perturbation modeling (e.g., D2 receptor antagonism)

### 4. Interactive 3D Visualization
- Static glass-brain overlays with statistical threshold mapping
- Inflated cortical surface rendering (fsaverage, interactive Plotly HTML)
- Circular functional connectivity diagrams (top 20% connections)
- Time-frequency heatmaps and ERP waveform plots
- Publication-ready figure export (300 dpi PNG + interactive HTML)

---

## Installation

### Prerequisites
- Python 3.9 or higher
- Flowith OS Beta (for Skill protocol integration)

### Python Dependencies

```bash
# Core neuroimaging
pip install nilearn nibabel scipy pandas matplotlib seaborn

# EEG/MEG analysis
pip install mne mne-bids mne-connectivity

# Computational modeling
pip install tvb-library tvb-data numpy

# Literature mining & NLP
pip install requests spacy pingouin plotly
python -m spacy download en_core_web_sm
```

Or install all at once:

```bash
pip install nilearn nibabel mne mne-bids mne-connectivity tvb-library \
            tvb-data requests spacy pingouin plotly pandas numpy \
            scipy matplotlib seaborn
python -m spacy download en_core_web_sm
```

---

## Usage

### Activating the Skill in Flowith OS

The skill activates automatically when you use any of the following trigger keywords in a Flowith OS conversation:

```
neuroscience | 神经科学 | fMRI | EEG | 脑成像 | 神经动力学
```

Upon activation, the assistant presents an interactive menu:

```
Welcome to the Neuroscience Research Assistant. Please select your research type:

  1. 🧠 Brain Imaging     — fMRI / structural MRI / DTI
  2. ⚡ Electrophysiology — EEG / MEG / LFP / spike sorting
  3. 🔬 Simulation        — Neural dynamics / brain network models (TVB)
  4. 📚 Literature Mining — PubMed search / brain region co-occurrence
  5. 🔄 Full Pipeline     — Multi-modal integrated workflow

Enter the corresponding number or describe your specific research need:
```

### Manual Skill Loading

Place `SKILL.md` in your Flowith OS skills directory:

```
~/.flowith/skills/neuroscience-research-assistant/SKILL.md
```

Then reference it in a conversation:

```
/skill neuroscience-research-assistant
```

### Module Navigation

| User Input | Activated Module |
|---|---|
| `1` / `fMRI` / `imaging` | §2A — Nilearn fMRI Pipeline |
| `2` / `EEG` / `electrophysiology` | §2B — MNE EEG Pipeline |
| `3` / `simulation` / `TVB` | §3 — TVB Computational Simulation |
| `4` / `literature` / `PubMed` | §1 — Literature Mining |
| `5` / `full pipeline` | §1 → §2 → §3 → §4 (sequential) |

---

## Application Example: Dopamine & Reward Learning

### Research Background

Dopaminergic modulation of reward-based learning is a canonical topic in computational psychiatry. The mesolimbic circuit — encompassing the **medial Prefrontal Cortex (mPFC)**, **Striatum**, and **Ventral Tegmental Area (VTA)** — is central to prediction error signaling and reinforcement learning. This example demonstrates how the skill supports a complete investigation of D2 receptor antagonism effects on this circuit.

### Step 1: Literature Trend Analysis

Using the PubMed mining module with the query:

```
dopamine reward learning fMRI[Title/Abstract] AND D2 receptor[Title/Abstract]
```

**Trend Findings (representative sample):**
- High-frequency brain regions: striatum, nucleus accumbens, mPFC, orbitofrontal cortex, VTA
- Top co-occurrence pair: **striatum ↔ mPFC** (consistent with cortico-striatal loop literature)
- Publication trend: steady increase in D2-related reward learning studies 2005–present, with a notable uptick in computational modeling papers post-2015
- Key reference surfaced: **Jocham et al. (2011)** — DRD2 polymorphism alters reversal learning and associated neural activity

### Step 2: D2 Receptor Antagonist Simulation

The TVB simulation module modeled D2 antagonism by reducing inhibitory coupling weights (`c_ie`, `c_ii`) by 25% in mPFC and Striatum nodes within a Wilson-Cowan coupled network, consistent with the GABAergic disinhibition mechanism described in Jocham et al. (2011).

**Simulation Configuration:**
- Model: Wilson-Cowan Coupled Network (3-node: mPFC, Striatum, VTA)
- Duration: 2,000 ms | dt: 0.1 ms
- Perturbation: 25% reduction in `c_ie` and `c_ii` (D2 blockade → reduced lateral inhibition)

**Results:**

| Region | Control Activity | D2 Antagonist Activity | Δ Change |
|--------|-----------------|----------------------|----------|
| mPFC | 0.223 | 0.506 | **+126.8%** |
| Striatum | 0.101 | 0.264 | **+160.5%** |
| VTA | 0.258 | 0.277 | +7.3% |

**Functional Connectivity (Pearson r):**

| Connection | Control | D2 Antagonist | Δ |
|-----------|---------|--------------|---|
| mPFC ↔ Striatum | 0.202 | 0.685 | **+0.484** |
| mPFC ↔ VTA | 0.098 | 0.146 | +0.047 |
| Striatum ↔ VTA | −0.128 | 0.080 | +0.208 |

**Interpretation:** D2 receptor antagonism produced a marked increase in cortico-striatal functional coupling (mPFC ↔ Striatum: r = 0.20 → 0.69), consistent with disinhibition of striatal projection neurons. The VTA showed minimal activity change, suggesting that the primary effect is on downstream cortico-striatal signal integration rather than dopamine release per se. These findings are broadly consistent with Jocham et al. (2011), who demonstrated that reduced D2 signaling enhances striatal activity during reversal learning.

**Glass Brain Visualization:**

![D2 Antagonist Glass Brain]
<img width="1830" height="629" alt="04_glass_brain_d2_antagonist" src="https://github.com/user-attachments/assets/82cef7fe-914e-41be-82a2-32bd11b88e30" />

*Figure: Simulated neural activity overlay on glass brain under D2 receptor antagonist condition. Warmer colors indicate elevated activity in mPFC and striatal regions.*

> Full simulation data: see [`examples/d2_simulation_report.md`](examples/d2_simulation_report.md) and `sim_results.json`.

**Reference:** Jocham G, Klein TA, Ullsperger M. (2011). Dopamine-mediated reinforcement learning signals in the striatum and ventromedial prefrontal cortex underlie value-based choices. *J Neurosci*, 31(40):14320–14330. [DOI: 10.1523/JNEUROSCI.2785-11.2011](https://doi.org/10.1523/JNEUROSCI.2785-11.2011)

---

## Output Directory Structure

The skill generates outputs organized as follows:

```
output/
├── literature/
│   ├── pubmed_results.csv
│   ├── brain_region_frequency.png
│   └── cooccurrence_heatmap.png
├── imaging/
│   ├── functional_connectivity.png
│   ├── conn_matrix.csv
│   ├── glass_brain_3d.png
│   └── surface_render.html
├── eeg/
│   ├── erp_waveform.png
│   ├── erp_topomap.png
│   └── tfr_power.png
├── simulation/
│   ├── tvb_simulation.png
│   └── parameter_sweep_results.csv
└── summary_report.md
```

---

## Key Python Libraries

| Module | Library | Install |
|--------|---------|---------|
| fMRI analysis | `nilearn` | `pip install nilearn` |
| EEG/MEG analysis | `mne` | `pip install mne` |
| Brain network simulation | `tvb-library` | `pip install tvb-library` |
| PubMed retrieval | `requests` | `pip install requests` |
| Statistical analysis | `pingouin` | `pip install pingouin` |
| Interactive visualization | `plotly` | `pip install plotly` |
| NLP / brain region NER | `spacy` | `pip install spacy` |
| Neuroimaging I/O | `nibabel` | `pip install nibabel` |

---

## Contributing

Contributions are welcome. Please open an issue to discuss proposed changes before submitting a pull request. When adding new pipeline modules, follow the structured output template conventions defined in `SKILL.md`.

---

## Citation

If you use this skill in academic work, please cite:

```bibtex
@software{neuroscience_skill_2024,
  title  = {Neuroscience Research Assistant Skill for Flowith OS},
  author = {flowithOS},
  year   = {2024},
  url    = {https://github.com/flowithOS/neuroscience-research-assistant}
}
```

---

## License

MIT License. See [LICENSE](LICENSE) for details.
