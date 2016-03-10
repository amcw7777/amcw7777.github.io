---
layout: post
title: D0(Charm) correlation
---

## Jet Reconstruction

### Motivation
The statistic of AuAu2014 is not enough for D-hadron correlation. The next trial is D-jet correlation. The jet reconstructed in the back of D is likely to be a c-jet. The tool in STAR right now is call 'fastJet'. The motivation of the study in the section is:

*   Understand the algorithm, how to get jet from tracks.
*   Implant the code to MuDst data format.
*   Find best solution for c-jet. Potentially need simulation or TMVA.

### Code source (reference)

1.  From Jan's email: the code is located in PDSF:
   *   analysis: /global/homes/r/rusnak/jet_analysis/STARJet/analysis
   *   macros for running code: /global/homes/r/rusnak/jet_analysis/STARJet/macros
   *   scripts for submitting jobs: /global/homes/r/rusnak/jet_analysis/STARJet/submit_scripts

2.  From  [StRoot StJetMaker](http://www.star.bnl.gov/cgi-bin/protected/cvsweb.cgi/StRoot/StJetMaker/)

