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

2. From  [StRoot StJetMaker](http://www.star.bnl.gov/cgi-bin/protected/cvsweb.cgi/StRoot/StJetMaker/)

3. From Kolja:
    *  [BasicAj](https://www.github.com/kkauder/BasicAj)
    *  [Event Structured Au](https://www.github.com/kkauder/eventStructuredAu)

### My code structure

1.  Select tracks

`TStarJetVectorContainer<TLorentzVector> *trackContainer;`
Save information of selected tracks (in TLorentzVector) to `trackContainer`. Need head file `#include <TStarJetVectorContainer.h>`. 

2.  Make input vector

    1.  The input vector is defined as `std::vector<fastjet::PseudoJet> input_vector;`. 
    2.  Then initialized the input vector by the container `input_vector = make_charged_input_vector(containerdata, pTmin, pTmax);`
    3.  Two questions marked here: 1) how to load fastjet lib; 2) is `make_charged_input_vector` in STAR lib? Maybe `#include "jet.h"`

3.  Jet reconstruction

    1.  Use data structure `FJWrapper akt_data;`. 
    2.  Initialize FJWrapper: need `#include "fjwrapper.h"`
    ```c+++
    akt_data.r = r;//double r = atof(gSystem->Getenv("RPARAM"));
    akt_data.maxrap = max_rap-r;//Float_t max_rap = atoi(gSystem->Getenv("MAXRAP"));
    akt_data.algor = fastjet::antikt_algorithm;
    akt_data.input_particles = input_vector;
    ```
    3.  Start to reconstruct `akt_data.Run()`

4.  Find jets: `std::vector<fastjet::PseudoJet> jets = akt_data.inclusive_jets;`
