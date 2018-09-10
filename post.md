# The postprocessing stage

At this stage the intensity of the virtual detector (`VD`) pixels (`px`) is computed. Typically, each `px` will have one or more frequencies associated to it. The `px` are independent, but are typically organized as one or more 2D images, each corresponding to an instant of the observer time (this is important: the image is not an average over a finite time interval, but is rather an instantaneous snapshot in the observer frame). The intensity of each `px` can be multiplied by its area to obtain the outgoing `px` luminosity, which, when divided by the square of the distance to the observer (and, if necessary, applying the cosmological conversions) gives the observed flux. (**Important**: it is enough to divide by the square of the distance, the factor 4 pi is not needed!). The output of a postprocessor is a virtual detector (`VD`) file stored in HDF5 (`.h5`) format.

_Note_: all the codes (both preprocessing and post-processing) use the same format for the parameter file. They each read only the information they need from the files, while the rest is ignored.

## GRB/TDE 1D/2D simulations

At this stage the file(s) produced by the pre-processor(s) (`shprSPEV2` or `thprSPEV2`) are read block by block, the visibility of each `particle` is determined, and if visible, its emission is computed and the contribution to the corresponding `px` is recorded. Once all the file(s) have been processed, the radiative transfer equation is solved for each `px` by first sorting all the contributions by distance from the observer, and then starting with the one furthest away and moving towards the observer.

We now discuss how the post-processors are used.

### `shpoSPEV2` and `mshpoSPEV2`

These codes post-processes the files produced by [`shprSPEV2`](./pre.html#shprspev2). The code `shpoSPEV2` (`sh`ock `po`stprocessor for `SPEV2`) only accepts a single input file, while the code `mshpoSPEV2` (`m`ultiple `sh`ock `po`stprocessor for `SPEV2`) accepts an arbitrary number of input files. Otherwise they work in exactly the same way (they use the same main routine and only differ in the way the command line is interpreted).

The usage of `shpoSPEV2` is:
`$ ./shpoSPEV2 params-file input-file output-root [start-block end-block]`

The usage of `mshpoSPEV2` is:
`./mshpoSPEV2 params-file input-file1 input-file2 ... input-file output-root`

The arguments are:
- `params-file`: name of the [parameters file][55fdff28]
- `input-file`: a (single) input (preprocessed) file
- `input-file1` `input-file2` `...`: a list of input files
- `output-root`: root for the output file names
- `start-block` and `end-block`: optional parameters that indicate the first and the last block to be read from the input file. These parameters only apply for `./shpoSPEV2`.

There is an [example](./examp.html#mshpoSPEV2) of the postprocessing stage.

  [55fdff28]: prm.html "parameters"
