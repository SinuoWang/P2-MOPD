<div align="center">

# P²-MOPD: Accelerating Multi-Teacher On-Policy Distillation via Parameter Priors

Sinuo Wang, Jiacheng Lu, Zeyu Zhang, Maojia Song, Rui Sun, Beidi Luan, Yutong Xie, Jing Li, Daxin Jiang, Qi Wu, Zuo Bai

[![arXiv](https://img.shields.io/badge/arXiv-2611.03953-b31b1b.svg)](https://arxiv.org/abs/2611.03953)

</div>

This repository hosts the official implementation of [P²-MOPD](https://arxiv.org/abs/2611.03953).

## Overview

Multi-teacher on-policy distillation (MOPD) consolidates domain experts into one student by letting each teacher supervise the student's own responses. Task arithmetic offers a training-free alternative, adding a weighted sum of the experts' task vectors to the base model.

**P²-MOPD treats the linear combination of student and expert parameters as a single differentiable model.** Every linear projection of the student is composed as

$$W_m = B_m + \sum_{k=1}^{K} \alpha_{k,m}\,\tau_{k,m},$$

where $\tau_{k,m}$ is the frozen task vector of expert $k$, $B_m$ are trainable student weights and $\alpha_{k,m}$ are learnable mixing coefficients. Both are optimized jointly under the standard MOPD objective. The experts therefore guide the student in two ways: their logits supervise its outputs, and their weights act as **parameter priors** that the student can move along through one scalar per module. Because the student weights stay trainable, the model is not confined to the span of the task vectors.

<p align="center">
  <img src="assets/P2-MOPD.png" width="95%" alt="Overview of P2-MOPD">
</p>

After training, $\alpha$ is folded into the weights, so the output is a standard checkpoint and inference cost is unchanged.

## Highlights

- **Faster.** P²-MOPD surpasses the final average recovery of MOPD from the same task-arithmetic start after half the training steps, at both 1.7B and 4B.
- **More balanced.** Worst-domain recovery rises from 47.2% / 32.0% to 89.4% / 88.0% (4B / 1.7B), and the across-domain std of recovery falls from 21.8 / 28.4 to 4.6 / 4.8.
- **Stable and interpretable.** The learned mixing coefficients are reproducible: the mean coefficient of each teacher agrees across three seeds to within 0.01, so the mixture is learned from the experts and the student rather than left to chance. They also consistently identify the teacher the student most needs (the medical coefficient rises from 0.25 to 0.63 / 0.65), without any validation data.

<p align="center">
  <img src="assets/fig_main_recovery.png" width="55%" alt="Average recovery over training">
</p>

## Citation

If you find this work useful, please cite:

```bibtex
@article{wang2026p2mopd,
  title   = {P$^2$-MOPD: Accelerating Multi-Teacher On-Policy Distillation via Parameter Priors},
  author  = {Wang, Sinuo and Lu, Jiacheng and Zhang, Zeyu and Song, Maojia and Sun, Rui and Luan, Beidi and Xie, Yutong and Li, Jing and Jiang, Daxin and Wu, Qi and Bai, Zuo},
  journal = {arXiv preprint arXiv:2611.03953},
  year    = {2026}
}
```

## Contact

For questions, please open an issue in this repository.
