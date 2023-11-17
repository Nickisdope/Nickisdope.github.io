---
layout: distill
title: Towards Interpretable Neural Implicit Scene Representations
description: Semester Thesis at Autumn Term 2023
tags: distill formatting
giscus_comments: false
date: 2023-11-16
featured: true

authors:
  - name: Xiang Liu
    url: ""
    affiliations:
      name: ETH Zurich

bibliography: 2018-12-22-distill.bib

# Optionally, you can add a table of contents to your post.
# NOTES:
#   - make sure that TOC names match the actual section names
#     for hyperlinks within the post to work correctly.
#   - we may want to automate TOC generation in the future using
#     jekyll-toc plugin (https://github.com/toshimaru/jekyll-toc).
# toc:
#   - name: Equations
#     # if a section has subsections, you can add them as follows:
#     # subsections:
#     #   - name: Example Child Subsection 1
#     #   - name: Example Child Subsection 2
#   - name: Citations
#   - name: Footnotes
#   - name: Code Blocks
#   - name: Interactive Plots
#   - name: Layouts
#   - name: Other Typography?

# Below is an example of injecting additional post-specific styles.
# If you use this post as a template, delete this _styles block.
# _styles: >
#   .fake-img {
#     background: #bbb;
#     border: 1px solid rgba(0, 0, 0, 0.1);
#     box-shadow: 0 0px 4px rgba(0, 0, 0, 0.1);
#     margin-bottom: 12px;
#   }
#   .fake-img p {
#     font-family: monospace;
#     color: white;
#     text-align: left;
#     margin: 12px 0;
#     text-align: center;
#     font-size: 16px;
#   }

---

## Abstract

Scene understanding is important for intelligent agents to plan context-sensitive actions. This paper seeks to find an interpretable neural implicit scene representation, where information from different scenes can be correlated. We separate the original NeRF<d-cite key="mildenhall2021nerf"></d-cite> architecture into a scene-specific hash grid encoding and a scene-agnostic rendering network. With this setting, multiple scenes can be trained simultaneously to obtain a general rendering network and a set of hash grid encodings containing geometric and appearance information for each scene individually. We can easily generalize to novel scenes by optimizing only the scene-specific hash grid encoding while keeping the parameters of the rendering network fixed, which makes our method more versatile. More importantly, we investigate the relation of feature vectors computed by hash grid encoding with its corresponding RGB value and manage to perform one naive scene editing experiment of moving objects between scenes by composing feature vectors.


***

<!-- ## Main Contribution
- We propose a protocol, accompanied by open-source code, for collecting high-quality 3D point-of-gaze data paired with near-eye IR images and world views in RGB-D format.
- Using the proposed protocol, we collect GT3D, a dataset with 18 participants, 527 3D point-of-gaze targets with a significant amount of depth variation, and 210,029 pairs of eye images.
- We propose a transformer-based gaze prediction model, 3DGazeFormer, that outperforms state-of-the-art gaze estimation methods on GT3D by a large margin and achieves a prediction error of 2.19 degrees.


*** -->

## Training Scheme
 
<img src="/assets/img/SP/training.png" width="700px">

**Mixed Batch Training and Novel Scene Generalization.** 

The top diagram shows the training scheme we proposed to enable joint training of multiple scenes in a single NeRF. Images taken at different scenes are first encoded by different hash encodings to feature vectors, then feature vectors from different scenes are concatenated and passed together into the shared rendering MLP. Then volume rendering is leveraged to generate images from multiple scenes. With this scheme, the network is jointly optimized for multiple scenes without overfitting to one particular scene.

The bottom diagram shows how we generalize to novel scenes by fixing the weights of pretrained rendering MLP and only optimizing the hash encoding of the scene. 

***

## Results
### Without Mixed Batch Training
<img src="/assets/img/SP/overfitting.png" width="700px">
The image above shows that the network overfits the hydrant scene without using our training scheme. As we can observe from the rendering results of apple_2 scene, the colors of the computer, table, and background are painted as hydrant_sf and crosswalk.


### With Mixed Batch Training

<img src="/assets/img/SP/mbt.png" width="700px">
After using our training scheme, the network optimized well for both scenes, as the original colors are preserved for both.

### Joint Training for 10 scenes

<img src="/assets/img/SP/10scenes.png" width="700px">
After solving the overfitting problem, we jointly trained our network for 10 scenes. In the figure shown above, we can observe that the network has the capacity to fit 10 scenes with their colors and structures well-preserved.

### Generalize to novel scene

<img src="/assets/img/SP/generalization.png" width="700px">

We fixed the weights of pretrained MLP on 10 scenes and then optimized only for the parameters of a new hash encoding on a novel scene. We can observe the rendering results on the first row, which are visually comparable to those optimizing for the whole network. Besides, we also ablate the pretraining phase by replacing the pre-trained MLP with an MLP with randomized weights and rendering the same set of images on a novel scene, which is shown on the bottom row. In this case, the results are over-smoothed and fail to catch the fine details of the novel scene, and only the high-level structure and color distribution are preserved. This shows the necessity of pretraining MLP to generalize to novel scenes.




