# Towards Multi-pose Guided Virtual Try-on Network


## Contribution

-  We introduce a novel task of virtual try-on conditioned
  on multiple poses, and collect a new dataset that covers
  different poses and various clothes.
- We propose a novel Multi-pose Guided Virtual Try-On
  Network (MG-VTON) that handles large pose varia-
  tions by disentangling the warping of clothes appear-
  ance and the pose manipulation in multiple stages.
  Specifically, we propose a pose-clothes guided human
  parsing network to first synthesize the human parsing
  with the desired clothes and pose, which effectively
  guides the virtual try-on to achieve reasonable results
  via the correct region parts.
-  We design a Warp-GAN that integrates human pars-
  ing with geometric matching to alleviate blurry issues
  caused by the misalignment among different poses.
-  A pose-guided refinement network is further proposed
  to adaptively controls the composition mask according
  to different poses, which learns to recover details and
  remove artifacts.
