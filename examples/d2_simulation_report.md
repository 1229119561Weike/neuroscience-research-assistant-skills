# D2 Receptor Antagonist Simulation Report

**Study:** Dopaminergic Modulation of Cortico-Striatal Functional Connectivity
**Model:** Wilson-Cowan Coupled Network (3-node)
**Date:** 2024
**Reference:** Jocham G et al. (2011). *J Neurosci*, 31(40):14320–14330.

---

## 1. Background and Motivation

Dopamine D2 receptors are densely expressed in the striatum and prefrontal cortex, where they modulate GABAergic interneuron activity and regulate the signal-to-noise ratio of neural ensembles. Pharmacological blockade of D2 receptors (antagonism) reduces lateral inhibition, leading to a net disinhibition of principal neurons in the striatum and mPFC.

Jocham et al. (2011) demonstrated that individuals carrying the A1 allele of the DRD2/ANKK1 Taq1A polymorphism — associated with reduced striatal D2 receptor density — show altered reversal learning performance and corresponding differences in striatal and ventromedial prefrontal activation. Their findings provide a direct empirical basis for the perturbation parameters used in this simulation.

**Research Question:** How does D2 receptor antagonism reshape functional connectivity within the mPFC–Striatum–VTA circuit, and does the simulated effect recapitulate patterns reported in human neuroimaging?

---

## 2. Simulation Configuration

### 2.1 Neural Mass Model

| Parameter | Value | Description |
|-----------|-------|-------------|
| Model | Wilson-Cowan | Mean-field excitatory/inhibitory population |
| Nodes | 3 | mPFC, Striatum, VTA |
| Duration | 2,000 ms | Simulation length |
| dt | 0.1 ms | Integration timestep |
| Integrator | Heun Stochastic | 2nd-order stochastic integration |
| Noise (nsig) | 0.01 | Additive Gaussian noise |

### 2.2 Pharmacological Perturbation: D2 Antagonism

D2 receptor blockade was modeled by reducing inhibitory coupling weights in the mPFC and Striatum nodes:

| Parameter | Control | D2 Antagonist | Mechanism |
|-----------|---------|--------------|-----------|
| `c_ie` (mPFC, Striatum) | baseline | −25% | Reduced I→E lateral inhibition |
| `c_ii` (mPFC, Striatum) | baseline | −25% | Reduced I→I recurrent inhibition |
| VTA parameters | unchanged | unchanged | D2 effects minimal at source |

**Rationale:** D2 receptors are primarily expressed on GABAergic interneurons (indirect pathway). Blockade reduces their inhibitory output onto pyramidal/projection neurons, consistent with the disinhibition model in Jocham et al. (2011) and Frank et al. (2004).

---

## 3. Results

### 3.1 Mean Neural Activity

| Region | Control | D2 Antagonist | Δ Absolute | Δ Percent |
|--------|---------|--------------|-----------|----------|
| mPFC | 0.2234 | 0.5065 | +0.2831 | **+126.8%** |
| Striatum | 0.1014 | 0.2642 | +0.1628 | **+160.5%** |
| VTA | 0.2580 | 0.2767 | +0.0187 | +7.3% |

**Key finding:** mPFC and Striatum showed substantial activity increases under D2 antagonism, while VTA activity was minimally affected. This is consistent with a downstream disinhibition mechanism rather than altered dopamine release from VTA terminals.

### 3.2 Functional Connectivity Matrix

**Control condition (Pearson r):**

|  | mPFC | Striatum | VTA |
|--|------|---------|-----|
| **mPFC** | 1.000 | 0.202 | 0.098 |
| **Striatum** | 0.202 | 1.000 | −0.128 |
| **VTA** | 0.098 | −0.128 | 1.000 |

**D2 Antagonist condition (Pearson r):**

|  | mPFC | Striatum | VTA |
|--|------|---------|-----|
| **mPFC** | 1.000 | 0.685 | 0.146 |
| **Striatum** | 0.685 | 1.000 | 0.080 |
| **VTA** | 0.146 | 0.080 | 1.000 |

**Delta (D2 antagonist − control):**

|  | mPFC | Striatum | VTA |
|--|------|---------|-----|
| **mPFC** | 0.000 | **+0.484** | +0.047 |
| **Striatum** | **+0.484** | 0.000 | +0.208 |
| **VTA** | +0.047 | +0.208 | 0.000 |

### 3.3 Summary of Key Connectivity Changes

| Connection | Change | Interpretation |
|-----------|--------|---------------|
| mPFC ↔ Striatum | +0.484 (r: 0.20 → 0.69) | Strong cortico-striatal coupling enhancement |
| Striatum ↔ VTA | +0.208 (r: −0.13 → 0.08) | Sign reversal; increased integration |
| mPFC ↔ VTA | +0.047 (r: 0.10 → 0.15) | Modest frontotegmental increase |

---

## 4. Visualization Reference

The following plots were generated from this simulation:

| File | Description |
|------|-------------|
| `vis_plots/01_timeseries_comparison.png` | Neural activity time series: control vs. D2 antagonist |
| `vis_plots/02_fc_matrix_comparison.png` | Side-by-side functional connectivity matrices |
| `vis_plots/03_mean_activity_bar.png` | Bar chart of mean activity per region |
| `vis_plots/04_glass_brain_d2_antagonist.png` | Glass brain overlay: D2 antagonist condition |
| `vis_plots/05_power_spectral_density.png` | Power spectral density comparison |

All visualization files are located in:
```
Folders/temp/neuroscience/vis_plots/
```

---

## 5. Interpretation and Neuroscientific Significance

### 5.1 Cortico-Striatal Hyper-Coupling

The most prominent effect of D2 antagonism in the simulation was the dramatic increase in mPFC–Striatum functional connectivity (r = 0.20 → 0.69). This hyper-coupling reflects the loss of segregating inhibition — under normal D2 tone, GABAergic interneurons maintain a degree of signal independence between these regions. With D2 blockade, this gating mechanism is impaired, resulting in synchronized activity fluctuations.

This computational result has direct parallels to human neuroimaging findings. Jocham et al. (2011) reported elevated striatal and vmPFC activation during reversal learning in individuals with reduced D2 receptor availability, consistent with the elevated mean activity levels observed here.

### 5.2 Implications for Reward-Based Learning

The cortico-striatal loop is the neural substrate of model-based and model-free reinforcement learning. Enhanced coupling under D2 antagonism could reflect:

1. **Impaired credit assignment:** Loss of precision in prediction error signals propagated from striatum to mPFC
2. **Perseveration:** Hyperactive cortico-striatal feedback may bias the system toward previously rewarded actions (consistent with reversal learning deficits)
3. **Noise amplification vs. signal enhancement:** The disinhibition increases both signal and noise; the net behavioral effect depends on the signal-to-noise ratio context

### 5.3 Limitations

- The 3-node model is a substantial simplification of the full mesolimbic/mesocortical circuit; the basal ganglia direct/indirect pathways are collapsed into a single "Striatum" node
- D2 antagonism was modeled as uniform parameter reduction; in vivo, D2 receptors show regional heterogeneity
- The simulation does not incorporate synaptic plasticity or learning dynamics

---

## 6. Data File

The raw simulation data is stored in:
```
Folders/temp/neuroscience/sim_results.json
```

**JSON schema:**
- `simulation_meta`: model parameters and reference
- `fc_matrices.control / d2_antagonist / delta`: 3×3 Pearson correlation matrices
- `mean_activity.control / d2_antagonist / delta / pct_change`: per-region activity statistics
- `output_files`: paths to all generated visualization files

---

## 7. References

1. Jocham G, Klein TA, Ullsperger M. (2011). Dopamine-mediated reinforcement learning signals in the striatum and ventromedial prefrontal cortex underlie value-based choices. *Journal of Neuroscience*, 31(40):14320–14330. https://doi.org/10.1523/JNEUROSCI.2785-11.2011

2. Frank MJ, Seeberger LC, O'Reilly RC. (2004). By carrot or by stick: cognitive reinforcement learning in Parkinsonism. *Science*, 306(5703):1940–1943.

3. Sanz Leon P, et al. (2013). The Virtual Brain: a simulator of primate brain network dynamics. *Frontiers in Neuroinformatics*, 7:10.

4. Wilson HR, Cowan JD. (1972). Excitatory and inhibitory interactions in localized populations of model neurons. *Biophysical Journal*, 12(1):1–24.
