---
title: Audio Synchronization
permalink: wiki/Audio_Synchronization/
layout: wiki
---

The goal of this component is to perform the synchronization of the all
audio files in an existing dataset.

### Input

Option 1:

Option 2:

### Output

On top of the txt files listed bellow, both options cut the audio files
in the dataset according to the segments and save them in
"sounds\_cut/".

Example: "sounds\_cut/1/10.0" has all cut audio files that exist in
cluster 1 starting on 10 seconds until the time stamp where the next
segments starts.

Option 1:

Option 2:

### File Examples

Input:

Output:

### Default dataset

For testing and validation purposes, a dataset with recordings crawled
from YouTube was used. Each song had one professional recording (BBC)
and a set of user-recordings.

Number of songs: 10 Number of samples: 90

<figure>
<img src="/wiki/dataset_info.png" title="Dataset information." width="500" alt="" /><figcaption>Dataset information.</figcaption>
</figure>
