---
layout: blog
title: "Introducing top-b"
date: 2025-11-02
author: "Deepon Halder, Raj Dabre, Ratish Pudupully, Anoop Kunchukuttan"
excerpt: "A new adaptive sampling method for language models that uses relative probability bands for dynamic, context-sensitive token selection."
tags: ["language models", "sampling methods", "top-b", "NLP", "generative AI"]
hidden: true  
---

_This project is being done at AI4Bharat, IIT Madras. The work is still in progress and in barebones state at the moment. We are releasing a part of the work that has been done for the community._

## 0. Introduction

Existing sampling strategies in language models — such as **Top-k** ([Fan et al., 2018](https://arxiv.org/abs/1805.04833)), **Top-p** ([Holtzman et al., 2020](https://arxiv.org/abs/1904.09751)), and **Min-p** ([Minh et al., 2024](https://arxiv.org/abs/2407.19605)) — rely on *static* or *absolute* thresholds to determine which tokens to consider at each generation step. While these methods work well in many contexts, they lack *adaptive responsiveness* to local probability distributions, leading to suboptimal diversity or stability depending on the prompt or task.  
**Top-b sampling** introduces a *relative probability band* centered around the most likely token, enabling dynamic, context-sensitive control over token selection.

---

## 1. Motivation

Language models output a probability distribution $P(t_i \mid C)$ over the vocabulary $V$, conditioned on context $C$. Traditional sampling methods filter this distribution using fixed criteria:

- **Top-k:** Retain only the $k$ most probable tokens ([Fan et al., 2018](https://arxiv.org/abs/1805.04833)).
- **Top-p:** Retain the smallest set of tokens $S \subset V$ such that:
  $$\sum_{t_i \in S} P(t_i \mid C) \geq p$$
  ([Holtzman et al., 2020](https://arxiv.org/abs/1904.09751))
- **Min-p:** Discard all tokens below a fixed probability threshold ([Minh et al., 2024](https://arxiv.org/abs/2407.19605)).

Each of these imposes a static boundary — either in *rank space* (Top-k) or *probability space* (Top-p, Min-p). However, the *shape* of the output distribution varies widely across contexts. A single fixed threshold cannot adapt to these changes.

---

## 2. The Core Idea

Top-b sampling defines a **relative band** around the top token probability.

Let $t_1$ be the most probable token with probability:

$$P_{\max} = P(t_1 \mid C)$$

Then define a *bandwidth ratio* $b \in (0, 1]$, and include all tokens whose probability falls within this band:

$$S_b = \{t_i \in V \mid P(t_i \mid C) \geq b \cdot P_{\max}\}$$

where $$b=1-adaptive\_bandwidth$$

Sampling then proceeds by renormalizing the probabilities within $S_b$:

$$P'(t_i \mid C) = \begin{cases}
\dfrac{P(t_i \mid C)}{\sum_{t_j \in S_b} P(t_j \mid C)} & \text{if } t_i \in S_b \\
0 & \text{otherwise}
\end{cases}$$

This creates an **adaptive dynamic threshold** — its width depends on the distribution’s concentration.

---

## 3. Adaptive Bandwidth

Let the probability distribution over tokens be denoted by $\mathbf{p} = \{p_i\}$, and let the Shannon entropy, $H(\mathbf{p})$, be given by:

$$H(\mathbf{p}) = -\sum_i p_i \log p_i$$

We define the adaptive bandwidth as:

$$\mathrm{adaptive\_bandwidth} = \mathrm{base\_bandwidth} \times \left( 1 + \frac{H(\mathbf{p})}{H_{\max}} \right )$$

where:
- $\text{base\_bandwidth}$ is the initial (static) bandwidth value (e.g., 0.2)
- $H(\mathbf{p})$ is the computed entropy of the current probability distribution
- $H_{\max}$ is the maximum possible entropy for the distribution (e.g., $\log(\text{vocab\_size})$ if the distribution were uniform)

---

### _Lemma 1: Maximum Entropy for Uniform Distribution_ 
The maximum entropy possible in any case is:

$$H_{\max} = \log n$$

where n is the total number of tokens, or the vocabulary.\\
**_Proof :_**
If the entropy is maximum, it means that all the probabilities will be equal, aka it will be a uniform distribution.

$$
\forall i \in \{1, \ldots, n\}, \quad P_i = \frac{1}{n}
$$

Hence using the formula of entropy, we can say: 

$$H_{\max} = -\sum_{i=1}^n \frac{1}{n} \log \frac{1}{n}$$

$$= -\sum_{i=1}^n \frac{1}{n} (-\log n)$$

$$= -(-\log n) \sum_{i=1}^n \frac{1}{n}$$

$$= \log n$$

---

## Code for Top-b

Here’s a quick PyTorch snippet for how top-b works 

<div class="code-block-wrapper">
<button class="copy-button">Copy</button>
<pre><code class="language-python">def apply_top_b(probs, base_bandwidth=0.5):
    entropy = -(probs * torch.log(probs + 1e-10)).sum()
    vocab_size = probs.shape[0]
    H_max = torch.log(torch.tensor(vocab_size, dtype=torch.float))
    adaptive_bandwidth = base_bandwidth * (1 + entropy / H_max)
    p_top = probs.max()
    t = p_top * (1 - adaptive_bandwidth)
    mask = (probs >= t).float()
    return mask
</code></pre>
</div>

## Results
### Experiment 1 : GPQA
The GPQA dataset ([Rein et al., 2023](https://arxiv.org/abs/2311.12022)) consists primarily of multiple-choice questions spanning a wide variety of physics, chemistry and many science topics.

All the experiments were run on 5-shot setting. The experiments were run on a set of 8xH100 GPUs for around a day. Hyperparameters are attached below.

| Parameter      | Value                |
| -------------- | -------------------- |
| Model          | google/gemma-3-4b-it |
| Max New Tokens | 512                  |
| Seed           | 42                   |
| Dtype          | torch.bfloat16       |
| Split          | gpqa_main            |

The results were as follows:

| Method      | Accuracy   | Avg_Entropy |
| ----------- | ---------- | ----------- |
| **Top-b**   | 0.2285     | **0.1579**  |
| Eta         | 0.2099     | 0.1592      |
| Min-p       | 0.2199     | 0.1714      |
| Epsilon     | **0.2389** | 0.1712      |
| Top-p       | 0.2132     | 0.1764      |
| Temperature | 0.2121     | 0.1774      |
| Top-k       | 0.2333     | 0.1815      |


If we see the figure below, we see the variance is the least for top-b. We will implore if that is the case in most cases. We also achieve the lowest entropy with top-b.

<figure style="max-width:450px; margin:auto;">
  <img src="/assets/top-b/gpqa_acc.png" alt="GPQA Scores" style="width:100%; height:auto;">
  <figcaption style="text-align:center; font-size:0.95em;">Figure: GPQA accuracy scores(best case) for different decoding strategies with variance</figcaption>
</figure>

We ran all the experiments over the temperatures, **0.7, 1.0, 2.0, 2.5**, and over 5 base values of bandwidths, **0.1, 0.2, 0.3, 0.4, 0.5**. We noticed the following result over all temperatures and bandwidths. We currently can't infer any conclusion from this.

<figure style="max-width:450px; margin:auto;">
  <img src="/assets/top-b/gpqa_topb_var.png" alt="GPQA Top-b Variance" style="width:100%; height:auto;">
  <figcaption style="text-align:center; font-size:0.95em;">Figure: Top-b accuracy variance on GPQA dataset</figcaption>
</figure>


We also did an analysis of how the entropy maps in top-b v/s top-p. Figure below:

<figure style="max-width:450px; margin:auto;">
  <img src="/assets/top-b/entropy_anal_gpqa.png" alt="GPQA Entropy Analysis" style="width:100%; height:auto;">
  <figcaption style="text-align:center; font-size:0.95em;">Figure: Entropy comparison of top-b vs top-p decoding strategies on the GPQA dataset</figcaption>
</figure>

We see the patterns are pretty similar in both the cases, but in the second half of the plot, top-b makes the entropy to close to 0, while top-p still has a lot of confusion, aka high entropy. The average entropy is lower for top-b as well. We feel like top-b makes it sure that the number of points, where entropy can be high, are as low as possible - hence allowing the model to give answers with the least amount of variance.


---



## Citation

If you reference this work, please cite it as:

<div class="code-block-wrapper">
<!-- <button class="copy-button">Copy</button> -->
<pre><code>@misc{halder2025topb,
  author       = {Deepon Halder, Raj Dabre, Ratish Pudupully, Anoop Kunchukuttan},
  title        = {Introducing top-b: Adaptive Relative Band Sampling for Language Models},
  year         = {2025},
  howpublished = {\url{https://deeponh.github.io/blog/introducing-top-b}},
}
</code></pre>
</div>
---

## References

- Fan, A., Lewis, M., & Dauphin, Y. (2018). [*Hierarchical Neural Story Generation*](https://arxiv.org/abs/1805.04833). *Proceedings of ACL 2018*.
- Holtzman, A., Buys, J., Du, L., Forbes, M., & Choi, Y. (2020). [*The Curious Case of Neural Text Degeneration*](https://arxiv.org/abs/1904.09751). *ICLR 2020*.
- Minh, T. et al. (2024). [*Turning Up the Heat: Min-p Sampling for Reliable Language Model Decoding*](https://arxiv.org/abs/2407.19605). *arXiv preprint arXiv:2407.19605*.
- Rein, D., Muennighoff, N., Schaeffer, R., Dey, M., & Xie, E. (2023). [*GPQA: A Graduate-Level Google-Proof Q&A Benchmark*](https://arxiv.org/abs/2311.12022). *arXiv preprint arXiv:2311.12022*.
