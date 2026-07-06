---
layout: paper
title: How to Scale Mixture-of-Experts
subtitle: From μP to the Maximally Scale-Stable Parameterization
venue: ICML HiLD Workshop 2026
badge: Oral Presentation
arxiv: "2605.14200"
authors:
  - name: Leena Chennuru Vankadara
    mark: "∗"
  - name: Moritz Haas
    mark: "∗"
  - name: Luke Hayward
    mark: "†"
  - name: Sebastian Bordt
    mark: "†"
  - name: Alessandro Breccia
    mark: "†"
author_note: "∗Equal first author.  †Equal contributor."
links:
  - label: Paper
    url: https://arxiv.org/abs/2605.14200
    icon: paper
  - label: Code
    url: https://github.com/vankadara-lab/mssp-moe
    icon: code
notice: An interactive blog post walking through the theory and experiments behind this work is in the works.
---

<section class="section">
  <figure class="figure">
    <div class="figure__frame">
      <img src="{{ '/assets/projects/2026_scaling-moe/figure1.png' | prepend: site.baseurl }}" alt="Four panels comparing μP and MSSP across MoE co-scaling regimes. Left panels (SGD, Adam) show MSSP recovering monotonic improvement with scale where μP degrades; right panels show learning-rate transfer under MSSP for Regimes II and III.">
    </div>
    <figcaption><strong>Figure 1.</strong> μP does not reliably improve performance with scale in MoEs. MSSP recovers monotonic improvement and delivers LR transfer across MoE co-scaling regimes. <em>Left:</em> Across optimizers and regimes, MSSP (solid lines) outperforms μP (dashed lines) at large scale for MLP MoEs on TinyImageNet. <em>Right:</em> LR transfer in validation loss for GPT MoEs trained with Adam in MSSP for 2.5B tokens when co-scaling width and number of (active) experts, with fixed expert width (Regime II) or co-scaled expert width (Regime III).</figcaption>
  </figure>
</section>

<section class="section">
  <h2 class="eyebrow">Abstract</h2>
  <p class="lead">Recent frontier large language models predominantly rely on Mixture-of-Experts (MoE) architectures. Despite empirical progress, there is still no principled understanding of how hyperparameters should scale with network width <i>N</i>, expert width <i>N<sub>e</sub></i>, number of experts <i>M</i>, sparsity <i>K</i>, and depth <i>L</i> to ensure both stability and optimal performance at scale. We take a principled step toward resolving this gap by analyzing three different scaling regimes: (I) co-scaling <i>N</i>&nbsp;≍&nbsp;<i>N<sub>e</sub></i>, (II) co-scaling <i>N</i>&nbsp;≍&nbsp;<i>M</i>&nbsp;≍&nbsp;<i>K</i>, and (III) full proportional scaling of <i>N</i>, <i>N<sub>e</sub></i>, <i>M</i>, and <i>K</i>. For each regime, we develop a novel Dynamical Mean Field Theory (DMFT) description of the limiting training dynamics of MoEs that provides a formal foundation for our analysis. Within this framework, we derive the unique parameterization for SGD and Adam satisfying all maximal-update (μP) desiderata. We then show that the resulting μP prescription does not reliably induce monotonic improvement with scale or robust learning-rate transfer. We trace these pathologies to scale-dependent observables in the aggregation dynamics, which motivates a refined set of desiderata that we term <i>maximal scale stability</i>. Guided by this principle, we derive a Maximally Scale-Stable Parameterization (MSSP) for both SGD and Adam in all three scaling regimes, and characterize the corresponding limiting dynamics — qualitatively distinct from the μP limit — through a separate DMFT analysis. Experiments verify that MSSP robustly recovers learning rate transfer and monotonic improvement with scale across regimes. Combined with existing depth-scaling theory, these results provide a complete scaling prescription for MoE architectures as a function of width, depth, expert width, and number of experts.</p>
</section>

<section class="section">
  <h2 class="eyebrow">Contributions</h2>
  <p class="lead">We study three co-scaling regimes for MoEs: <strong>Regime I</strong> — <i>N</i>&nbsp;≍&nbsp;<i>N<sub>e</sub></i>&nbsp;→&nbsp;∞ with <i>M</i>, <i>K</i> fixed; <strong>Regime II</strong> — <i>N</i>&nbsp;≍&nbsp;<i>M</i>&nbsp;≍&nbsp;<i>K</i>&nbsp;→&nbsp;∞ with <i>N<sub>e</sub></i> fixed, the fine-grained / bottleneck regime increasingly favored in modern MoE designs; and <strong>Regime III</strong> — joint scaling <i>N</i>&nbsp;≍&nbsp;<i>N<sub>e</sub></i>&nbsp;≍&nbsp;<i>M</i>&nbsp;≍&nbsp;<i>K</i>&nbsp;→&nbsp;∞.</p>

  <figure class="figure" style="margin-top: 1.9rem;">
    <div class="figure__frame">
      <img src="{{ '/assets/projects/2026_scaling-moe/figure2.png' | prepend: site.baseurl }}" alt="A 2×2 schematic: (a) the Transformer-MoE architecture, and the MoE block under (b) Regime I, (c) Regime II, and (d) Regime III, with active experts shown in orange and inactive experts in grey.">
    </div>
    <figcaption><strong>Figure 2.</strong> Transformer-MoE architecture and the MoE block under the three scaling regimes.</figcaption>
  </figure>

  <p class="lead" style="margin-top: 1.9rem; margin-bottom: 1.5rem;">Across these regimes, we make the following contributions:</p>
  <ul class="list">
    <li><strong>μP for MoEs across regimes and optimizers.</strong> In each scaling regime, we derive the parameterizations that satisfy the μP desiderata for SGD and Adam via signal propagation analyses which are formally justified by a novel Dynamical Mean Field Theory (DMFT) for each scaling regime.</li>
    <li><strong>Scale-dependence of μP in MoEs.</strong> Despite formally satisfying the μP desiderata, we find that μ-parameterizations do not reliably deliver learning-rate transfer or monotonic improvement with scale in MoEs (Fig. 1, 5 and 6). We trace this to scale-dependence in the training dynamics.</li>
    <li><strong>Maximally Scale-Stable Parameterization (MSSP).</strong> We propose <em>maximal scale stability</em> as a refined principle generalizing μP desiderata, and derive the Maximally Scale-Stable Parameterization (MSSP) for SGD and Adam in each regime. The required corrections to μP are structurally distinct across regimes: zero router initialization in Regime I, amplified expert-output initialization variance (1/<i>N<sub>e</sub></i> to <i>M</i>/<i>N<sub>e</sub></i>) in Regime II, and shared expert weights at initialization in Regime III.</li>
    <li><strong>DMFT under MSSP.</strong> We derive a self-consistent DMFT for the limiting training dynamics in each regime under MSSP, including a four-level conditional mean-field hierarchy in Regime III induced by the shared expert initialization, qualitatively distinct from the limiting dynamics under μP.</li>
    <li><strong>Empirical validation.</strong> We verify on MLP and Transformer MoEs that MSSP robustly recovers learning-rate transfer, and restores monotonic improvement with scale across all three regimes. We provide a complete scaling prescription for modern Transformer MoE architectures along width <i>N</i>, depth <i>L</i>, number of experts <i>M</i>, expert width <i>N<sub>e</sub></i> and number of active experts <i>K</i>.</li>
  </ul>
</section>

<section class="section">
  <h2 class="eyebrow">Cite</h2>
  <div class="codecard">
    <button class="copy" type="button" data-copy="#bibtex">Copy</button>
    <pre><code id="bibtex">@article{vankadara2026mssp,
  title   = {How to Scale Mixture-of-Experts: From {$\mu$}P to the
             Maximally Scale-Stable Parameterization},
  author  = {Chennuru Vankadara, Leena and Haas, Moritz and Hayward, Luke
             and Bordt, Sebastian and Breccia, Alessandro},
  journal = {arXiv preprint arXiv:2605.14200},
  year    = {2026}
}</code></pre>
  </div>
</section>
