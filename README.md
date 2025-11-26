<div align="center">

<h1>Beyond Randomness: Understand the Order of the Noise in Diffusion</h1>

<a href='https://arxiv.org/abs/2511.07756'><img src='https://img.shields.io/badge/Paper-ArXiv-B31B1B.svg'></a>
<a href='https://huggingface.co/spaces'><img src='https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Demo-FFD21E'></a>
<a href='#'><img src='https://img.shields.io/badge/Project-Page-20B2AA'></a>
<a href='#'><img src='https://img.shields.io/badge/License-MIT-blue'></a>

**Song Yan**$^{2,1}$, **Min Li**$^{*,1}$, **Bi Xinliang**$^{1}$, **Jian Yang**$^{2}$, **Yusen Zhang**$^{1}$, **Guanye Xiong**$^{1}$,<br> **Yunwei Lan**$^{1,2}$, **Tao Zhang**$^{3}$, **Wei Zhai**$^{*,2}$, **Zheng-Jun Zha**$^{2}$

<small>
$^1$ Xi'an High-tech Research Institute &nbsp;&nbsp; $^2$ USTC &nbsp;&nbsp; $^3$ HUST
</small>

<br><br>

<img src="assets/teaser_fig1.png" width="100%" alt="Teaser Comparison">

<p align="center">
  <em><strong>Figure 1:</strong> Our method is Training-Free, Universal, and Plug-and-Play. It significantly improves alignment and quality across SDXL, FLUX.1, Wan 2.1 (Video), and TRELLIS (3D).</em>
</p>

</div>

---

## 🔥 News
- **[2024.11.26]** Code released! We support **SDXL**, **FLUX.1**, **Wan 2.1**, and **TRELLIS**.
- **[2024.06.XX]** Paper submitted to ArXiv.

---

## 📖 Abstract

In text-driven content generation (T2C) diffusion models, the initial noise is typically characterized as a random element contributing solely to diversity. **Contrary to this view, this paper reveals that beneath the random surface of noise lies strong analyzable patterns**.

We identify that random noise inherently encodes rich semantic information. If mismatched with the prompt, this "noise semantic" leads to artifacts. We propose a simple, **Training-Free**, and **Universal** two-step process:

1.  **Semantic Erasure:** Diluting unwanted semantics via *Noise Latent Normalization (NLN)*.
2.  **Semantic Injection:** Injecting prompt-aligned semantics using the pre-trained model itself via *Temporal Prediction Weighting (TPW)*.

---

## 🧠 Methodology

Our method is grounded in the theoretical equivalence between the **denoising process** and **semantic injection**.

### 1. Semantic Erasure (NLN)
Random noise often carries "idiosyncratic" semantics. We propose **Noise Latent Normalization (NLN)** to aggregate $n$ i.i.d. noise samples and re-standardize them. [cite_start]This cancels out directions orthogonal to the summation axis (the "private" semantics) while preserving the Gaussian distribution [cite: 218-227].

$$
\overline{s}_{t} = \frac{1}{\sqrt{n}} \sum_{i=1}^{n} z_{t}^{i}, \quad \text{where } z_{t}^{i} \sim \mathcal{N}(0, I)
$$

### 2. Semantic Injection (TPW)
We utilize the pre-trained model to inject helpful semantics via **Temporal Prediction Weighting (TPW)**. This leverages the phase-dependent nature of diffusion: early steps for layout, middle for structure, and late for details.

$$
\epsilon_{agg} = \frac{\sum_{k=1}^{K} w_{k} \epsilon_{\theta}(z_{T}, t_{k}, y)}{\sqrt{\sum_{k=1}^{K} w_{k}^{2}}}
$$

### 3. Final Adjustment
We blend the purified noise with the aggregated semantics:

$$
\epsilon_{adj} = \frac{z_{T} + \delta(t) \epsilon_{agg}}{\sqrt{1+\delta(t)^2}}
$$

---

## 🖼️ Gallery

[cite_start]Our method works across **Image**, **Video**, and **3D** generation tasks [cite: 1, 271-278].

<table align="center">
  <tr>
    <th align="center">Model</th>
    <th align="center">Standard</th>
    <th align="center">Ours</th>
    <th align="center">Prompt</th>
  </tr>
  <tr>
    <td align="center"><strong>FLUX.1</strong><br>(Image)</td>
    <td align="center"><img src="assets/flux_std.png" width="200px"></td>
    <td align="center"><img src="assets/flux_ours.png" width="200px"></td>
    <td align="center">"A blue colored pizza."<br></td>
  </tr>
  <tr>
    <td align="center"><strong>Wan 2.1</strong><br>(Video)</td>
    <td align="center"><img src="assets/wan_std.png" width="200px"></td>
    <td align="center"><img src="assets/wan_ours.png" width="200px"></td>
    <td align="center">"A storefront with 'CVPR' written on it."<br></td>
  </tr>
  <tr>
    <td align="center"><strong>TRELLIS</strong><br>(3D)</td>
    <td align="center"><img src="assets/trellis_std.png" width="200px"></td>
    <td align="center"><img src="assets/trellis_ours.png" width="200px"></td>
    <td align="center">"A claw hammer with a metallic grey hammer head..."<br></td>
  </tr>
</table>

---

## 💻 Usage

Our method involves no training. You only need to modify the inference pipeline.

### Requirements
```bash
pip install torch diffusers transformers
