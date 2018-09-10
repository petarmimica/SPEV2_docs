# `SPEV` documentation

Welcome to the main page of the `SPEV` documentation. This document contains an introduction and links to other, more detailed documentation pages.

Petar Mimica, August 2018

## Introduction

`SPEV` is a radiation transfer code that is capable of processing the results of various hydrodynamic simulation codes and compute time- and frequency-dependent emission (images, spectra, light curves). For a technical introduction you can read the paper by [Mimica _et al._ (2016)][4c31a2f3]. A more detailed description can be found starting on page 32 of the master's thesis by Carmen Aloy ([Aloy C. 2016][2e979a74]).

In essence, to successfully get `SPEV` to produce a synthetic observation, the following three steps need to be executed:

1. **[Preprocess][0528faf2]** one or (typically) more snapshots of a hydrodynamic simulation to create a collection of emitting space time volumes (`particles`). `Particles` are stored in one or more `input files`. Those are hierarchically organised HDF5 files that store particle data in blocks.  For more information you can consult Chapter 5 (page 79) of Aloy C. ([2016][2e979a74]).
2. **[Postprocess][c929024b]** one or (typically) more `input files` to compute the intensity several `pixels`. A `pixel` is a rectangle in the plane of the sky storing the intensity for one more more frequencies. A virtual detector (`VD`) is a collection of `pixels` organised in a given way (typically as a grid of 2D images at different time instants). Each `pixel` is defined independently, so that in principle any arbitrary collection of rectangular `pixels` can be used instead of a rectangula grid. Typicall one or more `VDs` are produced as a result of the postprocessing stage.
3. **[Convert][29558c59]** the `VDs` as needed. There are many tools that can be used, depending on the purpose. Some of the tools integrate the intensity in the detector to obtain the time- and frequency-dependent luminosity or fluxes, while others can join several `VDs` into one (typically used when the resources do not permit simultaneous calculation of the larger `VD`).

# Table of contents

This is the table of contents of the `SPEV` documentation:
1. [Compiling][ede2bea0]
2. [Parameter file][3d8ed2ee]
3. [Preprocessing][0528faf2]
4. [Postprocessing][c929024b]
5. [Converting][29558c59]
6. [Examples][ad7f1642]

  [4c31a2f3]: http://adsabs.harvard.edu/abs/2016JPhCS.719a2008M "Mimica et al. (2016) paper"
  [2e979a74]: https://riunet.upv.es/handle/10251/35350 "Thesis Carmen Aloy"
  [0528faf2]: pre.html "Preprocessing"
  [c929024b]: post.html "Postprocessing"
  [29558c59]: conv.html "Converting"
  [ad7f1642]: examp.html "Examples"
  [3d8ed2ee]: prm.html "Parameter file"
  [ede2bea0]: compile.html "Compiling"
