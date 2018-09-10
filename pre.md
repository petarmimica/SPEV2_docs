# The preprocessing stage

At this stage the hydro snapshots are processed for the first time. The objective is to create a database of the spacetime emitting volumes (`particles`). These can have 2 spatial dimensions (for case of spherically or axially symmetric GRB/TDE/jet simulations) or 3 spatial dimensions (for cosmological or other cartesian simulations). We discuss each of these separately.

_Note_: all the codes (both preprocessing and post-processing) use the same format for the parameter file. They each read only the information they need from the files, while the rest is ignored.

## GRB/TDE 1D/2D simulations

The emitting volumes' geometry and kinematics are defined by the following quantities:
- `xm`: lower x-direction boundary
- `xp`: upper x-direction boundary
- `ym`: lower y-direction boundary
- `yp`: upper y-direction boundary
- `vxm`: lower x-velocity
- `vxp`: higher x-velocity
- `vym`: lower y-velocity
- `vyp`: higher y-velocity
- `time`: start of the lab frame time interval
- `ntime`: end of the lab frame time interval

`x` and `y` can mean radius/angle in the spherical case or radius/z in the cylindrical case. Typically the lower and higher velocities will be the same except for the particles being accelerated at shocks (see below).

The particle position is interpolated between `time` and `ntime` by assuming that its corners move with the corresponding velocities. This is done during the [postprocessing][d5aeb4f3] stage.

Apart from the above data, the particle also needs to store other information needed to compute its emission. This is problem-dependent.

We now discuss the two programs that preprocess 1D and 2D simulations.

### `shprSPEV2`

This code processes the results of 1D and 2D simulations in spherical coordinates. Its name means `sh`ock`pr`eprocessor for `SPEV2`. The program is used as follows:

`$ ./shprSPEV2 params-file input-root output-root start-file end-file block-size file-size [snap-skip]`

The arguments (all obligatory) are:
- `params-file`: name of the [parameters file][b53a3586]
- `input-root`: root of the hydro file names (i.e. if the files are `result-00000.h5`, `result-00001.h5`, etc. then the root is `result`)
- `output-root`: root for the output file name(s)
- `start-file`: a number denoting the first hydro file to be processed.
- `end-file`: a number denoting the last hydro file to be processed. _Note_: it may happen that all the snapshots are contained in one file. In that case `end_file` is equal to `start_file`.
- `block-size`: the number of particles in a block. _Note_: the recommended size is between 10000 and 100000.
- `file-size`: the number of blocks in the output file. If you are processing 1D simulations, you can set this number to a large value to store all particles in a single file. For 2D simulations it is recommended to use not more than 100 blocks to avoid large file sizes.
- `snap-skip`: how many snapshots to skip at the start of the simulation. This is used to avoid being affected by initial numerical transients and artefacts. If left out, it is assumed to be 0.


There is an [example](examp.md#shprspev2) of the preprocessing of a 1D simulation.

The particles that are created at the shocks always have `xm` equal to `xp`, but `vxp` > `vxm`. This means that in the time interval between `time` and `ntime` the particle grows from zero x-width to a finite x-width `(vxp - vxm) * (ntime - time)`. In `shprSPEV2` the value of `vxp` is set to the estimated shock front velocity. Once the particle is created, in the subsequent iterations `vxm` = `vxp` and are set to the local fluid velocity behind the shock. In this way it is ensured that the particles will fill the volume behind the shock regardless of the snapshot frequency.

A standard shock acceleration phenomenology is used to initialize the electron energy distribution and to estimate the magnetic field.

  [d5aeb4f3]: post.md "postprocessing"
  [b53a3586]: prm.md "Parameters"
