---
layout: post
title: Vertex Finder
---

## Motivation 
*   To find the best vertex finder for 1)pp and 2)heavy ion events
*   To understand performance of 1)KFVertex; 2)Minuit Vertex; 3)PPV 
*   To find a common VF for both pp and heavy ion 


## PPV
An algorithm for pp events. In the previous discussion, I know some features about PPV:

1.  It only fits on z direction and the x and y direction are same as beam line constraint. Although there is a [website](https://drupal.star.bnl.gov/STAR/blog/balewski/2009/may/20/upgrade-ppv-vertex-finder-3d-pp-500-beam-line-constrain-2009) which shows the intention to update PPV to 3D. It's still not applied yet.
2.  PPV is a 1-dimensional algorithm, which only gets the vertex position event-by-event in z, based on the ASSUMPTION that the fundamental width of the beam spot (~100 um) is much better than the resolution that can be obtained by the TPC in low multiplicity events.

### Improvement of PPV
Dmitri is doing an improvement of PPV. The idea is to find the first PV with Minuit to minimize the chi2 and find the iptimal position for a common vertex of all tracks pointing to the initial seed. [Source code](https://github.com/star-bnl/star-vertex/compare/ds-ppv-fitter)

1.  How to install
    1.  `git clone --recursive https://github.com/star-bnl/star-vertex.git`
    2.  `cd star-vertex/`
    3.  `git checkout ds-ppv-fitter`
    4.  `mkdir build && cd build`
    5.  `cmake -D CMAKE_INSTALL_PREFIX=./ ../`
    6.  `make install`
    7.  Then you will find libStGenericVertexMaker.so  libStSecondaryVertexMaker.so and libStZdcVertexMaker.so. Include them when calling bfc. 

2.  Another ToolKit for vertex find in : https://github.com/star-bnl/star-vertex-eval
