# The converting stage

At this stage the output `VD` files produced by various postprocessors are converted into light curves, cropped, joined, split etc.

## Light curve converters

These are the tools that integrate the `VD` in space and produce a time- and frequency-dependent light curve. There is a general converter `lcproch5` that makes no assumptions about geometry and an on-axis axisymmetri converter `oaproch5` that only works for a 1D `VDs` such as the ones used in the [`mshpoSPEV2` example](./examp.md#mshpospev2).

The usage of both `lcproch5` and `oaproch5` is the same:

`$ ./oaproch5 input_file output_file freq [length_norm [time_unit [use mJy]]]`

The arguments are:
- `input_file`: a `VD` file in HDF5 format
- `output_file`: a text file in which the light curve will be stored
- `freq`: frequency of the light curve. If the value is negative, then all frequencies will be written. If the value is positive, than the frequency closest to the desired one will be written.
- `length_norm`: the length normalization of the detector (typically equal to the length normalization specified in the postprocessor parameter file). The default values is 1.
- `time_unit`: the default values is 1 second, but other useful values are 86400 for days and 3.15576d7 for years.
- `usemJy`: if set to 1, then the light curve is expressed in milli Jansky.

There is an [example](./examp.md#oaproch5) for `oaproch5`.
