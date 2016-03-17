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

3.  From Kolja:
    *  [BasicAj](https://www.github.com/kkauder/BasicAj)
    *  [Event Structured Au](https://www.github.com/kkauder/eventStructuredAu)

4.  From Alex:
    *  code: /global/homes/a/aschmah/STAR/Analysis/Jet/StRoot/
    *  macro: /global/homes/a/aschmah/STAR/Analysis/Jet/JetAnalysis_Macro.cc
    *  [Related talk](http://www.star.bnl.gov/protected/heavy/aschmah/Presentations/aschmah_JetCorr_03_11_2016_V1.pdf)

### My code structure

1.  Install fastjet: 

    1.  `wget http://fastjet.fr/repo/fastjet-3.1.3.tar.gz`
    2.  `tar zxvf fastjet-3.1.3.tar.gz`
    3.  `cd fastjet-3.1.3`
    4.  `./configure --enable-shared --prefix=`pwd`/../fastjet3 CXXFLAGS="-O2 -m32 -g -Wall" CFLAGS="-O2 -m32 -g -Wall"`
    5.  `make && make install`

2.  Code implementation:
    
    1.  Select tracks: fastjet::PseudoJet is a template of TLorentzVector, which can used to store both jets and tracks information. A `std::vector< fastjet::PseudoJet> selectedTracks` is a good container for selected tracks.

    2.  Set jet definition, including both algorithm and size of R. (See more in [FastJet user manual](http://arxiv.org/pdf/1111.6097.pdf))
    `fastjet::JetDefinition jetDefinition(fastjet::antikt_algorithm,jet_R);`

    3.  Set area definition 
    `fastjet::AreaDefinition areaDefinition(fastjet::active_area_explicit_ghosts,fastjet::GhostedAreaSpec(ghost_maxrap,1,0.01));`

    4.  Start to fit:
    `fastjet::ClusterSequenceArea clust_seq_hard(selectedTracks,jetDefinition,areaDefinition);`

3.  How to  include `fastjet` when compile StRoot:
`cons EXTRA_CXXFLAGS="-Ifastjet3/include/"`

4.  QA plots: 

    1.  rho and jet area (See page#6 in [Alex's talk about h-Jet correlation Au-Au 200GeV](http://www.star.bnl.gov/protected/heavy/aschmah/Presentations/aschmah_JetCorr_03_11_2016_V1.pdf))
    2.  



> 1.  Select tracks
>
> > `TStarJetVectorContainer<TLorentzVector> *trackContainer;`
> > Save information of selected tracks (in TLorentzVector) to `trackContainer`. Need head file `#include <TStarJetVectorContainer.h>`. 
>
> 2.  Make input vector
>
>     1.  The input vector is defined as `std::vector<fastjet::PseudoJet> input_vector;`. 
>     2.  Then initialized the input vector by the container `input_vector = make_charged_input_vector(containerdata, pTmin, pTmax);`
>     3.  Two questions marked here: 1) how to load fastjet lib; 2) is `make_charged_input_vector` in STAR lib? Maybe `#include "jet.h"`
>
> 3.  Jet reconstruction
>
>     1.  Use data structure `FJWrapper akt_data;`. 
>     2.  Initialize FJWrapper: need `#include "fjwrapper.h"`
>
>     `akt_data.r = r;//double r = atof(gSystem->Getenv("RPARAM"));`
>     `akt_data.maxrap = max_rap-r;//Float_t max_rap = atoi(gSystem->Getenv("MAXRAP"));`
>     `akt_data.algor = fastjet::antikt_algorithm;`
>     `akt_data.input_particles = input_vector;`
>
>     3.  Start to reconstruct `akt_data.Run()`
>
> 4.  Find jets: `std::vector<fastjet::PseudoJet> jets = akt_data.inclusive_jets;`

### See also

*   [Alex's talk about h-Jet correlation Au-Au 200GeV](http://www.star.bnl.gov/protected/heavy/aschmah/Presentations/aschmah_JetCorr_03_11_2016_V1.pdf)
*   [Area-median background subtraction](http://indico.cern.ch/event/242816/contribution/9/attachments/413250/574181/2013-07-03-ParisJetWorkshop-gsoyez.pdf)
*   [FastJet user manual](http://arxiv.org/pdf/1111.6097.pdf)

