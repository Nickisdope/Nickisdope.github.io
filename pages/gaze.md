---
layout: distill
title: Towards real-world 3D gaze tracking
description: In submission to ETRA 2024
tags: distill formatting
giscus_comments: false
date: 2023-11-16
featured: true

authors:
  - name: Xiang Liu
    url: ""
    affiliations:
      name: ETH Zurich
  - name: Mathis Lindner
    url: ""
    affiliations:
      name: ETH Zurich
  - name: Nikola Popovic
    url: ""
    affiliations:
      name: ETH Zurich
  - name: Xi Wang
    url: ""
    affiliations:
      name: ETH Zurich
  - name: Danda Pani Paudel
    url: ""
    affiliations:
      name: ETH Zurich
  - name: Luc Van Gool
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

Studying where individuals direct their gaze is a valuable signal for understanding actions and activities, particularly when captured
by eye trackers mounted on the head during everyday tasks. However, gaze estimation in 3D is notably challenging, especially in
estimating depth, where two gaze rays create an acute triangle, leading to an inherently ill-posed problem. We tackle this task by
introducing a comprehensive 3D gaze tracking dataset GT3D and a robust method 3DGazeFormer for 3D gaze estimation using infrared
images captured close to the eyes. We propose a novel system for 3D gaze collection by precisely aligning between a depth camera
and a mobile RGB camera. Our system enables the rapid collection of high-quality gaze data with a wide range of depth variations,
thereby overcoming the constraints of static 3D target locations. We present GT3D, the largest publicly available dataset of its kind by
scale, paired with scene content and calibrated camera parameters. 3DGazeFormer builds on existing methods by integrating temporal
dynamics and outperforms existing state-of-the-art methods. Our model achieves a prediction error of 2.19 degrees of visual angle. Our dataset and model will be made publicly available.

<style>
  .icon {
    font-size: 24px; /* You can adjust the size as needed */
  }
</style>

<!-- PDF Link. -->
<span class="link-block">
<a href=""
    class="external-link button is-normal is-rounded is-dark">
    <span class="icon">
        <i class="fas fa-file-pdf"></i>
    </span>
    <span>Paper (under review)</span>
</a>
</span>
<!-- <span class="link-block">
<a href=""
    class="external-link button is-normal is-rounded is-dark">
    <span class="icon">
        <i class="ai ai-arxiv"></i>
    </span>
    <span>arXiv</span>
</a>
</span> -->
<!-- Video Link. -->
<!-- <span class="link-block">
<a href=""
    class="external-link button is-normal is-rounded is-dark">
    <span class="icon">
        <i class="fab fa-youtube"></i>
    </span>
    <span>Video</span>
</a>
</span> -->
<!-- Code Link. -->
<span class="link-block">
<a href=""
    class="external-link button is-normal is-rounded is-dark">
    <span class="icon">
        <i class="fab fa-github"></i>
    </span>
    <span>Code (preparing)</span>
    </a>
</span>
<!-- Dataset Link. -->
<span class="link-block">
<a href=""
    class="external-link button is-normal is-rounded is-dark">
    <span class="icon">
        <i class="far fa-images"></i>
    </span>
    <span>Data (preparing)</span>
    </a>


***

<!-- ## Main Contribution
- We propose a protocol, accompanied by open-source code, for collecting high-quality 3D point-of-gaze data paired with near-eye IR images and world views in RGB-D format.
- Using the proposed protocol, we collect GT3D, a dataset with 18 participants, 527 3D point-of-gaze targets with a significant amount of depth variation, and 210,029 pairs of eye images.
- We propose a transformer-based gaze prediction model, 3DGazeFormer, that outperforms state-of-the-art gaze estimation methods on GT3D by a large margin and achieves a prediction error of 2.19 degrees.


*** -->

## Collection Protocal
 
<img src="/assets/img/ETRA2024/hardware_setup.png" width="700px">

**Hardware setup for recording.** The diagram shows how different devices are placed during the recording. The participant wears the pupil invisible glasses which have two off-axis cameras capturing eye movement and one scene camera captures the frontal view. The depth camera is attached to a tripod to ensure stability throughout the recording and capture the depth map of the frontal view, which is later used to compute the ground-truth 3D point-of-gaze using the laser as a target. 

***

## Data Samples

Size of dataset | 45GB
Number of eye-image pairs | 210,029
Number of scene images | 32,154
Number of 3D point-of-gaze targets | 527
Number of recordings | 26
Number of participants | 18



<img src="/assets/img/ETRA2024/eye_images.png" width="700px">



**Eye and scene image examples.** The top two rows are example images of the left and right eyes with different appearances and gaze direction, and the bottom is their corresponding scene image denoting their current field of view.

***

## Model Architecture

<img src="/assets/img/ETRA2024/GazeFormer.png" width="700px">

**Architecture of our model.** The model takes in a pair of eye image sequences as input and regresses the sequence gaze angles. Eye images are encoded by a ResNet, and then features from two eyes are concatenated and fed into the transformer, whose output is then projected to angle space by an MLP.

***

## Results
<img src="/assets/img/ETRA2024/within_part_hist.png" width="700px">

**Within participant gaze angular error distribution.** Our method largely concentrates on the low error zone.

<img src="/assets/img/ETRA2024/cross_part_hist.png" width="700px">

**Cross participant gaze angular error distribution.** Our method has by far the fewest predictions with substantially high angular error ($$ > 7 ^{\circ}$$).

<!-- ## Citations

Citations are then used in the article body with the `<d-cite>` tag.
The key attribute is a reference to the id provided in the bibliography.
The key attribute can take multiple ids, separated by commas.

The citation is presented inline like this: <d-cite key="gregor2015draw"></d-cite> (a number that displays more information on hover).
If you have an appendix, a bibliography is automatically created and populated in it.

Distill chose a numerical inline citation style to improve readability of citation dense articles and because many of the benefits of longer citations are obviated by displaying more information on hover.
However, we consider it good style to mention author last names if you discuss something at length and it fits into the flow well — the authors are human and it’s nice for them to have the community associate them with their work.

***

## Footnotes

Just wrap the text you would like to show up in a footnote in a `<d-footnote>` tag.
The number of the footnote will be automatically generated.<d-footnote>This will become a hoverable footnote.</d-footnote>

***

## Code Blocks

Syntax highlighting is provided within `<d-code>` tags.
An example of inline code snippets: `<d-code language="html">let x = 10;</d-code>`.
For larger blocks of code, add a `block` attribute:

<d-code block language="javascript">
  var x = 25;
  function(x) {
    return x * x;
  }
</d-code>

**Note:** `<d-code>` blocks do not look good in the dark mode.
You can always use the default code-highlight using the `highlight` liquid tag:

{% highlight javascript %}
var x = 25;
function(x) {
  return x * x;
}
{% endhighlight %}

***

## Interactive Plots

You can add interative plots using plotly + iframes :framed_picture:

<div class="l-page">
  <iframe src="{{ '/assets/plotly/demo.html' | relative_url }}" frameborder='0' scrolling='no' height="500px" width="100%" style="border: 1px dashed grey;"></iframe>
</div>

The plot must be generated separately and saved into an HTML file.
To generate the plot that you see above, you can use the following code snippet:

{% highlight python %}
import pandas as pd
import plotly.express as px
df = pd.read_csv(
  'https://raw.githubusercontent.com/plotly/datasets/master/earthquakes-23k.csv'
)
fig = px.density_mapbox(
  df,
  lat='Latitude',
  lon='Longitude',
  z='Magnitude',
  radius=10,
  center=dict(lat=0, lon=180),
  zoom=0,
  mapbox_style="stamen-terrain",
)
fig.show()
fig.write_html('assets/plotly/demo.html')
{% endhighlight %}

***

## Details boxes

Details boxes are collapsible boxes which hide additional information from the user. They can be added with the `details` liquid tag:

{% details Click here to know more %}
Additional details, where math $$ 2x - 1 $$ and `code` is rendered correctly.
{% enddetails %}

***

## Layouts

The main text column is referred to as the body.
It is the assumed layout of any direct descendants of the `d-article` element.

<div class="fake-img l-body">
  <p>.l-body</p>
</div>

For images you want to display a little larger, try `.l-page`:

<div class="fake-img l-page">
  <p>.l-page</p>
</div>

All of these have an outset variant if you want to poke out from the body text a little bit.
For instance:

<div class="fake-img l-body-outset">
  <p>.l-body-outset</p>
</div>

<div class="fake-img l-page-outset">
  <p>.l-page-outset</p>
</div>

Occasionally you’ll want to use the full browser width.
For this, use `.l-screen`.
You can also inset the element a little from the edge of the browser by using the inset variant.

<div class="fake-img l-screen">
  <p>.l-screen</p>
</div>
<div class="fake-img l-screen-inset">
  <p>.l-screen-inset</p>
</div>

The final layout is for marginalia, asides, and footnotes.
It does not interrupt the normal flow of `.l-body` sized text except on mobile screen sizes.

<div class="fake-img l-gutter">
  <p>.l-gutter</p>
</div>

***

## Other Typography?

Emphasis, aka italics, with *asterisks* (`*asterisks*`) or _underscores_ (`_underscores_`).

Strong emphasis, aka bold, with **asterisks** or __underscores__.

Combined emphasis with **asterisks and _underscores_**.

Strikethrough uses two tildes. ~~Scratch this.~~

1. First ordered list item
2. Another item
⋅⋅* Unordered sub-list.
1. Actual numbers don't matter, just that it's a number
⋅⋅1. Ordered sub-list
4. And another item.

⋅⋅⋅You can have properly indented paragraphs within list items. Notice the blank line above, and the leading spaces (at least one, but we'll use three here to also align the raw Markdown).

⋅⋅⋅To have a line break without a paragraph, you will need to use two trailing spaces.⋅⋅
⋅⋅⋅Note that this line is separate, but within the same paragraph.⋅⋅
⋅⋅⋅(This is contrary to the typical GFM line break behaviour, where trailing spaces are not required.)

* Unordered list can use asterisks
- Or minuses
+ Or pluses

[I'm an inline-style link](https://www.google.com)

[I'm an inline-style link with title](https://www.google.com "Google's Homepage")

[I'm a reference-style link][Arbitrary case-insensitive reference text]

[I'm a relative reference to a repository file](../blob/master/LICENSE)

[You can use numbers for reference-style link definitions][1]

Or leave it empty and use the [link text itself].

URLs and URLs in angle brackets will automatically get turned into links.
http://www.example.com or <http://www.example.com> and sometimes
example.com (but not on Github, for example).

Some text to show that the reference links can follow later.

[arbitrary case-insensitive reference text]: https://www.mozilla.org
[1]: http://slashdot.org
[link text itself]: http://www.reddit.com

Here's our logo (hover to see the title text):

Inline-style:
![alt text](https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 1")

Reference-style:
![alt text][logo]

[logo]: https://github.com/adam-p/markdown-here/raw/master/src/common/images/icon48.png "Logo Title Text 2"

Inline `code` has `back-ticks around` it.

```javascript
var s = "JavaScript syntax highlighting";
alert(s);
```

```python
s = "Python syntax highlighting"
print s
```

```
No language indicated, so no syntax highlighting.
But let's throw in a <b>tag</b>.
```

Colons can be used to align columns.

| Tables        | Are           | Cool  |
| ------------- |:-------------:| -----:|
| col 3 is      | right-aligned | $1600 |
| col 2 is      | centered      |   $12 |
| zebra stripes | are neat      |    $1 |

There must be at least 3 dashes separating each header cell.
The outer pipes (|) are optional, and you don't need to make the
raw Markdown line up prettily. You can also use inline Markdown.

Markdown | Less | Pretty
--- | --- | ---
*Still* | `renders` | **nicely**
1 | 2 | 3

> Blockquotes are very handy in email to emulate reply text.
> This line is part of the same quote.

Quote break.

> This is a very long line that will still be quoted properly when it wraps. Oh boy let's keep writing to make sure this is long enough to actually wrap for everyone. Oh, you can *put* **Markdown** into a blockquote.


Here's a line for us to start with.

This line is separated from the one above by two newlines, so it will be a *separate paragraph*.

This line is also a separate paragraph, but...
This line is only separated by a single newline, so it's a separate line in the *same paragraph*. -->
