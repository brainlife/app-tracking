[![Abcdspec-compliant](https://img.shields.io/badge/ABCD_Spec-v1.1-green.svg)](https://github.com/brain-life/abcd-spec)
[![Run on Brainlife.io](https://img.shields.io/badge/Brainlife-bl.app.105-blue.svg)](https://doi.org/10.25663/bl.app.105)

# app-tracking

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

**Tracks whole-brain streamlines from single-shell DWI with MRtrix 0.2.12 using DT_STREAM, SD_STREAM and SD_PROB, outputting three .tck tractograms and a reconstruction.**

Fits a tensor and a constrained spherical deconvolution model (lmax auto-calculated from the bvals if not given), builds a white-matter mask from FreeSurfer aparc+aseg registered to the b0 with FSL flirt, and runs streamtrack with three algorithms seeded and masked in white matter. Use it to obtain whole-brain tractograms for comparison of tracking methods or as input to tract segmentation. Needs a FreeSurfer run of the same subject. The code loads MRtrix, FreeSurfer and FSL as HPC modules, so it is tied to that environment.

Run it on brainlife.io: [https://doi.org/10.25663/bl.app.105](https://doi.org/10.25663/bl.app.105)

| Input | Datatype | Required | Description |
|---|---|---|---|
| `dwi` | `neuro/dwi` (single_shell) | yes | Single-shell diffusion-weighted image with bvals and bvecs. Single shell (datatype tag single_shell). |
| `freesurfer` | `neuro/freesurfer` | yes | FreeSurfer recon-all output of the same subject; aparc+aseg is used to build the white-matter mask. Should be roughly aligned to the DWI; the white-matter mask is brought into DWI space with FSL flirt. |

| Output | Datatype | Description |
|---|---|---|
| `dt_stream` | `neuro/track/tck` | Whole-brain tractogram from tensor-based deterministic tracking (DT_STREAM). |
| `sd_stream` | `neuro/track/tck` | Whole-brain tractogram from CSD-based deterministic tracking (SD_STREAM). |
| `sd_prob` | `neuro/track/tck` | Whole-brain tractogram from CSD-based probabilistic tracking (SD_PROB). |
| `recon` | `neuro/recon` | Model outputs: CSD coefficients, FA, tensor, white-matter mask and brain mask. |

| Parameter | Type | Default | Description |
|---|---|---|---|
| `lmax` | number |  | Maximum spherical harmonic order for the CSD fit; leave empty to calculate it from the bvals. |
| `fibers` | number | `500000` | set the desired number of tracks. streamtrack will continue to generate tracks until this number of tracks have been selected and written to the output file. |
| `fibers_max` | number | `1000000` | set the maximum number of tracks to generate. streamtrack will not generate more tracks than this number, even if the desired number of tracks hasn't yet been reached (default is 100 x fibers). Specifying zero for this option removes any limit - the algorithm will keep generating tracks until the number required has been reached |

Runtime: container `brainlife/dipy:0.14.1`; 8 CPUs.

<!-- brainlife-app:end -->

This service runs mrtrix tracking using SD_PROB, SD_STREAM, and DT_STREAM algorithms. It generates 3 separate whole brain tracking output for each algorithms.

### Authors
- Brent McPherson (bcmcpher@iu.edu)
- Lindsey Kitchell (kitchell@iu.edu)
- Soichi Hayashi (hayashis@iu.edu)
- Franco Pestilli (frakkopesto@gmail.com)
- Paolo Avesani (avesani@fbk.eu)

### Project director
- Franco Pestilli (franpest@indiana.edu)

### Funding 
[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-1636893](https://img.shields.io/badge/NSF_BCS-1636893-blue.svg)](https://nsf.gov/awardsearch/showAward?AWD_ID=1636893)

## Running the App 

### On Brainlife.io

You can submit this App online at ..

* For dtiinit input [https://doi.org/10.25663/bl.app.59](https://doi.org/10.25663/bl.app.59)
* For dwi input [https://doi.org/10.25663/bl.app.105](https://doi.org/10.25663/bl.app.105) 

via the "Execute" tab.

### Running Locally (on your machine)

1. git clone this repo.
2. Inside the cloned directory, create `config.json` with something like the following content with paths to your input files.

```json
{
    "dwi":  "/path/to/dwi/dwi.nii.gz",
    "bvals":  "/path/to/dwi/dwi.bvals",
    "bvecs":  "/path/to/dwi/dwi.bvecs",
    "freesurfer": "/path/to/freesurfer/output",
    "fibers": 500000,
    "fibers_max": 1000000
}
```

3. Launch the App by executing `main`

```bash
./main
```

### Sample Datasets

If you don't have your own input file, you can download sample datasets from Brainlife.io, or you can use [Brainlife CLI](https://github.com/brain-life/cli).

```
npm install -g brainlife
bl login
mkdir input
bl dataset download 5a050a00eec2b300611abff3 && mv 5a050a00eec2b300611abff3 input/dwi
bl dataset download 5a065cc75ab38300be518f51 && mv 5a065cc75ab38300be518f51 input/freesurfer
```

## Output

The output for App is composed by two [brainlife.io/DataTypes](brainlife.io/docs/user/datatypes) and four datasets. 

```
brainlife.io DataTypes: 
    - recon
    - track/tck
    
datasets:
    - recon
    - track/tck:dt_stream
    - track/tck:sd_stream
    - track/tck:sd_prob
```

#### Product.json

The secondary output of this app is `product.json`. This file allows web interfaces, DB and API calls on the results of the processing. 

### Dependencies

This App only requires [singularity](https://www.sylabs.io/singularity/) to run. If you don't have singularity, you will need to install following dependencies.  

  - Matlab: https://www.mathworks.com/products/matlab.html
  - jsonlab: https://www.mathworks.com/matlabcentral/fileexchange/33381-jsonlab-a-toolbox-to-encode-decode-json-files
  - VISTASOFT: https://github.com/vistalab/vistasoft/
  - MRTRIX 2.0: https://jdtournier.github.io/mrtrix-0.2/index.html
