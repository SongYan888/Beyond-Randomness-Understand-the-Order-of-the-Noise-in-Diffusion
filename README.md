\<div align="center"\>

\<h1\>Beyond Randomness: Understand the Order of the Noise in Diffusion\</h1\>

\<a href='[https://arxiv.org/abs/2406.xxxxx](https://www.google.com/search?q=https://arxiv.org/abs/2406.xxxxx)'\>\<img src='[https://img.shields.io/badge/Paper-ArXiv-red](https://www.google.com/search?q=https://img.shields.io/badge/Paper-ArXiv-red)'\>\</a\>
\<a href='[https://huggingface.co/spaces](https://huggingface.co/spaces)'\>\<img src='[https://img.shields.io/badge/%F0%9F%A4%97%20Hugging%20Face-Demo-orange](https://www.google.com/search?q=https://img.shields.io/badge/%25F0%259F%25A4%2597%2520Hugging%2520Face-Demo-orange)'\>\</a\>
\<a href='\#'\>\<img src='[https://img.shields.io/badge/Project-Page-green](https://www.google.com/search?q=https://img.shields.io/badge/Project-Page-green)'\>\</a\>

**Yunwei Lan**¹,² $^*$ · **Tao Zhang**³ · **Wei Zhai**¹,² · **Zheng-Jun Zha**² · **Yunwei Lan**¹,²

¹Xi’an High-tech Research Institute · ²USTC · ³HUST

\<p align="center"\>
\<img src="assets/teaser\_fig1.png" width="95%" alt="Teaser Comparison"\>
\</p\>
[cite\_start]\<em\>Figure 1: Our method significantly improves semantic alignment and generation quality across SDXL, FLUX.1, Wan 2.1 (Video), and TRELLIS (3D) without any training. [cite: 1, 12-25]\</em\>

\</div\>

-----

## 📖 Abstract

In text-driven content generation (T2C) diffusion models, the initial noise is typically treated as a random element contributing solely to diversity. [cite\_start]**Contrary to this view, we reveal that initial noise contains strong, analyzable semantic patterns.** [cite: 48, 49]

We identify that random noise inherently encodes rich semantic information. If mismatched with the prompt, this "noise semantic" leads to artifacts and misalignment. To solve this, we propose a **Training-Free** and **Universal** two-step process:

1.  [cite\_start]**Semantic Erasure:** Diluting unwanted semantics via Noise Latent Normalization (NLN). [cite: 53, 216]
2.  [cite\_start]**Semantic Injection:** Injecting prompt-aligned semantics using the pre-trained model itself via Temporal Prediction Weighting (TPW). [cite: 53, 228]

[cite\_start]This method works on **any diffusion architecture** (DiT, UNet, Flow Matching) for Image, Video, and 3D generation. [cite: 45]

-----

## 🚀 Key Features

  * **⚡ Training-Free:** No fine-tuning or LoRAs required. [cite\_start]Works directly with off-the-shelf checkpoints. [cite: 44]
  * [cite\_start]**🌐 Universal Compatibility:** Tested successfully on **SDXL** (UNet), **FLUX.1** (DiT/Flow), **Wan 2.1** (Video), and **TRELLIS** (3D). [cite: 54]
  * **🛠️ Plug-and-Play:** A simple modification to the sampling procedure that can be integrated into existing pipelines (e.g., Diffusers, ComfyUI).
  * [cite\_start]**📈 Performance:** Consistently outperforms standard pipelines in semantic alignment (VQAScore, PickScore) and visual fidelity. [cite: 281]

-----

## 🧠 Methodology

Our approach leverages Information Theory and the equivalence between the denoising process and semantic injection.

### 1\. Semantic Erasure (NLN)

Random noise often carries "idiosyncratic" semantics that conflict with your prompt. [cite\_start]By averaging multiple independent noise samples and re-normalizing, we cancel out these specific directions while maintaining a valid Gaussian distribution. [cite: 218, 225]

$$\overline{s}_{t} = \frac{1}{\sqrt{n}} \sum_{i=1}^{n} z_{t}^{i}, \quad \text{where } z_{t}^{i} \sim \mathcal{N}(0, I)$$

### 2\. Semantic Injection (TPW)

Once the noise is "clean," we inject the *correct* semantics. [cite\_start]We use the model's own predictions at key timesteps to guide the initial noise toward the target manifold. [cite: 228, 407]

$$\epsilon_{adj} = \frac{z_{T} + \delta(t) \epsilon_{agg}}{\sqrt{1+\delta(t)^2}}$$

Where $\epsilon_{agg}$ is the weighted aggregation of predicted noise/velocity at early, middle, and late stages.

\<details\>
\<summary\>\<strong\>Click to see the Algorithm\</strong\>\</summary\>

[cite\_start]**Algorithm 1: Semantic Erasure and Injection** [cite: 718]

1.  **Stage 1: Semantic Erasure**
      * Sample $n$ independent noise vectors.
      * Compute purified noise $z_T$ via summation and normalization.
2.  **Stage 2: Semantic Injection**
      * Select $K$ key timesteps.
      * For each timestep, predict noise/velocity using the model and prompt.
      * Aggregate predictions with weights $w_k$.
3.  **Stage 3: Noise Adjustment**
      * Mix purified noise $z_T$ with aggregated semantics $\epsilon_{agg}$ controlled by $\delta$.
4.  **Stage 4: Final Generation**
      * Run standard sampling using the adjusted noise.

\</details\>

-----

## 🖼️ Gallery & Comparisons

### Text-to-Image (FLUX.1 & SDXL)

> **Prompt:** "A blue colored pizza."
> *Standard:* Generates a normal pizza or slight tint.
> [cite\_start]*Ours:* Generates a distinctly blue pizza with correct textures. [cite: 18, 24]

### Text-to-Video (Wan 2.1)

> **Prompt:** "A storefront with 'CVPR' written on it."
> *Standard:* Often fails text rendering or structural coherence.
> [cite\_start]*Ours:* Clear, legible text and stable architecture. [cite: 25, 35]

### Text-to-3D (TRELLIS)

> **Prompt:** "A claw hammer with a metallic grey hammer head..."
> *Standard:* Missing textures or incorrect geometry.
> [cite\_start]*Ours:* High fidelity geometry and material separation. [cite: 35, 43]

-----

## 💻 Usage

*(Pseudo-code based on the paper's logic)*

```python
coming soon
```

-----

## 📊 Quantitative Results

[cite\_start]Our method consistently achieves higher scores on standard benchmarks. [cite: 281]

| Benchmark | Model | Method | PickScore 🏆 | ImageReward 🖼️ |
| :--- | :--- | :--- | :--- | :--- |
| **Pick-a-Pic** | SDXL | Standard | 17.0490 | -1.9705 |
| | | **Ours** | **17.0520** | **-1.9662** |
| **DrawBench** | FLUX.1 | Standard | 17.2748 | -2.0509 |
| | | **Ours** | **17.2871** | **-2.0380** |

-----

## 📝 Citation

If you find this work useful, please cite our paper:

```bibtex
@article{lan2024beyond,
  title={Beyond Randomness: Understand the Order of the Noise in Diffusion},
  author={Lan, Yunwei and Zhang, Tao and Zhai, Wei and Zha, Zheng-Jun},
  journal={Xi’an High-tech Research Institute & USTC},
  year={2024}
}
```

-----

\<p align="center"\>
[cite\_start]\<i\>"Noise not only contains rich semantic information, but also allows for the erasure of unwanted semantics."\</i\> [cite: 52]
\</p\>
