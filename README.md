<h1 align="center">MRtrix2 Tracking</h1>

<p align="center">
  Three whole-brain tractograms (DT_STREAM, SD_STREAM and SD_PROB) and a tensor and CSD reconstruction from single-shell diffusion data and FreeSurfer output, with MRtrix 0.2.12.
</p>

<p align="center">
  <a href="https://github.com/brainlife/abcd-spec"><img src="https://img.shields.io/badge/ABCD_Spec-v1.1-green.svg" alt="ABCD Spec v1.1"></a>
  <a href="https://doi.org/10.25663/bl.app.105"><img src="https://img.shields.io/badge/Brainlife-bl.app.105-blue.svg" alt="Run on brainlife.io"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-MIT-lightgrey.svg" alt="MIT license"></a>
</p>

> **This branch, `1.5`, runs 2 brainlife.io Apps** that share the same code:
>
> - MRtrix2 Tracking (dwi) ([doi:10.25663/bl.app.105](https://doi.org/10.25663/bl.app.105))
> - MRtrix2 Tracking with dtiInit ([doi:10.25663/bl.app.59](https://doi.org/10.25663/bl.app.59))
>
> The other Apps in this repository are listed under **Versions on brainlife.io** below.

## At a glance

<!-- brainlife-app:begin (generated from the App manifest; edit the manifest, not this block) -->

### MRtrix2 Tracking (dwi)

> **Tracks whole-brain streamlines from single-shell DWI with MRtrix 0.2.12 using DT_STREAM, SD_STREAM and SD_PROB, outputting three .tck tractograms and a reconstruction.**

Fits a tensor and a constrained spherical deconvolution model (lmax auto-calculated from the bvals if not given), builds a white-matter mask from FreeSurfer aparc+aseg registered to the b0 with FSL flirt, and runs streamtrack with three algorithms seeded and masked in white matter. Use it to obtain whole-brain tractograms for comparison of tracking methods or as input to tract segmentation. Needs a FreeSurfer run of the same subject. The code loads MRtrix, FreeSurfer and FSL as HPC modules, so it is tied to that environment.

| | |
|---|---|
| **Inputs** | `neuro/dwi` · `neuro/freesurfer` |
| **Outputs** | `neuro/track/tck` ×3 · `neuro/recon` |
| **Container** | `brainlife/dipy:0.14.1` |
| **Resources** | 8 CPUs |
| **Run it** | [doi.org/10.25663/bl.app.105](https://doi.org/10.25663/bl.app.105) |

#### Inputs

| Input | Datatype | Required | Description |
|---|---|---|---|
| `dwi` | `neuro/dwi` (single_shell) | yes | Single-shell diffusion-weighted image with bvals and bvecs. Single shell (datatype tag single_shell). |
| `freesurfer` | `neuro/freesurfer` | yes | FreeSurfer recon-all output of the same subject; aparc+aseg is used to build the white-matter mask. Should be roughly aligned to the DWI; the white-matter mask is brought into DWI space with FSL flirt. |

#### Outputs

| Output | Datatype | Description |
|---|---|---|
| `dt_stream` | `neuro/track/tck` | Whole-brain tractogram from tensor-based deterministic tracking (DT_STREAM). |
| `sd_stream` | `neuro/track/tck` | Whole-brain tractogram from CSD-based deterministic tracking (SD_STREAM). |
| `sd_prob` | `neuro/track/tck` | Whole-brain tractogram from CSD-based probabilistic tracking (SD_PROB). |
| `recon` | `neuro/recon` | Model outputs: CSD coefficients, FA, tensor, white-matter mask and brain mask. |

<details>
<summary><b>Parameters</b> (3)</summary>

| Parameter | Type | Default | Description |
|---|---|---|---|
| `lmax` | number |  | Maximum spherical harmonic order for the CSD fit; leave empty to calculate it from the bvals. |
| `fibers` | number | `500000` | set the desired number of tracks. streamtrack will continue to generate tracks until this number of tracks have been selected and written to the output file. |
| `fibers_max` | number | `1000000` | set the maximum number of tracks to generate. streamtrack will not generate more tracks than this number, even if the desired number of tracks hasn't yet been reached (default is 100 x fibers). Specifying zero for this option removes any limit - the algorithm will keep generating tracks until the number required has been reached |

</details>

### MRtrix2 Tracking with dtiInit

> **Tracks whole-brain streamlines from a dtiInit output with MRtrix 0.2.12 using DT_STREAM, SD_STREAM and SD_PROB, outputting three .tck tractograms and a reconstruction.**

Fits a tensor and a constrained spherical deconvolution model (lmax auto-calculated from the bvals if not given), builds a white-matter mask from FreeSurfer aparc+aseg registered to the b0 with FSL flirt, and runs streamtrack with three algorithms seeded and masked in white matter. Use it to obtain whole-brain tractograms for comparison of tracking methods or as input to tract segmentation. Needs a FreeSurfer run of the same subject. The code loads MRtrix, FreeSurfer and FSL as HPC modules, so it is tied to that environment.

| | |
|---|---|
| **Inputs** | `neuro/dtiinit` · `neuro/freesurfer` |
| **Outputs** | `neuro/track/tck` ×3 · `neuro/recon` |
| **Container** | `brainlife/dipy:0.14.1` |
| **Resources** | 8 CPUs |
| **Run it** | [doi.org/10.25663/bl.app.59](https://doi.org/10.25663/bl.app.59) |

#### Inputs

| Input | Datatype | Required | Description |
|---|---|---|---|
| `dwi` | `neuro/dtiinit` | yes | dtiInit output; the aligned DWI, bvals and bvecs listed in dt6.json are tracked. |
| `freesurfer` | `neuro/freesurfer` | yes | FreeSurfer recon-all output of the same subject; aparc+aseg is used to build the white-matter mask. Should be roughly aligned to the DWI; the white-matter mask is brought into DWI space with FSL flirt. |

#### Outputs

| Output | Datatype | Description |
|---|---|---|
| `dt_stream` | `neuro/track/tck` | Whole-brain tractogram from tensor-based deterministic tracking (DT_STREAM). |
| `sd_stream` | `neuro/track/tck` | Whole-brain tractogram from CSD-based deterministic tracking (SD_STREAM). |
| `sd_prob` | `neuro/track/tck` | Whole-brain tractogram from CSD-based probabilistic tracking (SD_PROB). |
| `recon` | `neuro/recon` | Model outputs: CSD coefficients, FA, tensor, white-matter mask and brain mask. |

<details>
<summary><b>Parameters</b> (3)</summary>

| Parameter | Type | Default | Description |
|---|---|---|---|
| `lmax` | number |  | Maximum spherical harmonic order for the CSD fit; leave empty to calculate it from the bvals. |
| `fibers` | integer | `500000` | set the desired number of tracks. streamtrack will continue to generate tracks until this number of tracks have been selected and written to the output file. |
| `fibers_max` | integer | `1000000` | set the maximum number of tracks to generate. streamtrack will not generate more tracks than this number, even if the desired number of tracks hasn't yet been reached (default is 100 x fibers). Specifying zero for this option removes any limit - the algorithm will keep generating tracks until the number required has been reached |

</details>

<!-- brainlife-app:end -->

## Versions on brainlife.io

This repository hosts two Apps built from the same branch; they differ only in the diffusion input they take.

| App | Diffusion input | Branch | Run it | Status |
|---|---|---|---|---|
| **MRtrix2 Tracking (dwi)** (this branch) | `neuro/dwi` (single shell) | [`1.5`](https://github.com/brainlife/app-tracking/tree/1.5) | [doi:10.25663/bl.app.105](https://doi.org/10.25663/bl.app.105) | current |
| **MRtrix2 Tracking with dtiInit** (this branch) | `neuro/dtiinit` | [`1.5`](https://github.com/brainlife/app-tracking/tree/1.5) | [doi:10.25663/bl.app.59](https://doi.org/10.25663/bl.app.59) | available |

## Running the App

### On brainlife.io

Open the App for your input type, [doi.org/10.25663/bl.app.105](https://doi.org/10.25663/bl.app.105) for a single-shell DWI or [doi.org/10.25663/bl.app.59](https://doi.org/10.25663/bl.app.59) for a dtiInit output (its aligned DWI, bvals and bvecs listed in `dt6.json` are used), choose the FreeSurfer output of the same subject, set the parameters and press **Execute**.

- `fibers`: number of streamlines to write for each algorithm (default 500000).
- `fibers_max`: maximum number of streamlines to generate before giving up (default 1000000; 0 removes the limit).
- `lmax`: maximum spherical harmonic order for the CSD fit; leave empty to compute it from the number of non-zero bvals.

The App archives one `neuro/recon` dataset and three `neuro/track/tck` datasets, tagged `dt_stream`, `sd_stream` and `sd_prob`, which can be compared or passed to tract segmentation Apps.

### Locally

This App loads MRtrix 0.2.12, FreeSurfer, FSL and Python as HPC environment modules (`module load`), so it is tied to that environment. You also need [Singularity/Apptainer](https://apptainer.org/) (DIPY 0.14.1 runs from `brainlife/dipy:0.14.1`) and [`jq`](https://jqlang.github.io/jq/).

1. Clone this repository and check out the App branch: `git checkout 1.5`.
2. Create `config.json` with the paths to your inputs (use `"dtiinit": "/path/to/dtiinit"` instead of the `dwi`, `bvals` and `bvecs` keys for a dtiInit input):

   ```json
   {
       "dwi": "/path/to/dwi/dwi.nii.gz",
       "bvals": "/path/to/dwi/dwi.bvals",
       "bvecs": "/path/to/dwi/dwi.bvecs",
       "freesurfer": "/path/to/freesurfer/output",
       "fibers": 500000,
       "fibers_max": 1000000
   }
   ```

3. Run the App:

   ```bash
   ./main
   ```

### Sample data

Download a sample DWI and FreeSurfer output from brainlife.io with the [brainlife CLI](https://github.com/brainlife/cli):

```bash
npm install -g brainlife
bl login
mkdir input
bl dataset download 5a050a00eec2b300611abff3 && mv 5a050a00eec2b300611abff3 input/dwi
bl dataset download 5a065cc75ab38300be518f51 && mv 5a065cc75ab38300be518f51 input/freesurfer
```

## Outputs

The App builds a white-matter mask from the FreeSurfer `aparc+aseg` and brings it to the DWI with FSL `flirt`, makes a brain mask from the DWI, fits a tensor (`dwi2tensor`), estimates the single-fibre response from high-FA white matter and fits CSD (`csdeconv`). It then runs `streamtrack` with each algorithm, seeded and masked in white matter.

Written to the working directory:

```text
output.DT_STREAM.tck   tensor-based deterministic tractogram
output.SD_STREAM.tck   CSD-based deterministic tractogram
output.SD_PROB.tck     CSD-based probabilistic tractogram
csd.nii.gz             CSD coefficients          ┐
fa.nii.gz              fractional anisotropy     │
dt.nii.gz              diffusion tensor          │ neuro/recon
whitematter.nii.gz     white-matter mask         │
brainmask.nii.gz       DWI brain mask            ┘
product.json           track_info and tractography metadata for each tractogram
```

## Troubleshooting

- **The white-matter mask does not cover the DWI**: the FreeSurfer output must be roughly aligned to the DWI; the mask is only registered to the mean b0 with `flirt`.

## Citing

> [!TIP]
> If you use this App in your work, please cite the App, the software it builds on and brainlife.io (below). GitHub's **Cite this repository** button gives the App citation in APA and BibTeX.

- MRtrix2 Tracking (dwi). brainlife.io App. [doi:10.25663/bl.app.105](https://doi.org/10.25663/bl.app.105)
- MRtrix2 Tracking with dtiInit. brainlife.io App. [doi:10.25663/bl.app.59](https://doi.org/10.25663/bl.app.59)
- Fischl. FreeSurfer. *NeuroImage* 62:774-781 (2012). [doi:10.1016/j.neuroimage.2012.01.021](https://doi.org/10.1016/j.neuroimage.2012.01.021)
- Jenkinson et al. FSL. *NeuroImage* 62:782-790 (2012). [doi:10.1016/j.neuroimage.2011.09.015](https://doi.org/10.1016/j.neuroimage.2011.09.015)
- Tournier et al. MRtrix3: A fast, flexible and open software framework for medical image processing and visualisation. *NeuroImage* 202:116137 (2019). [doi:10.1016/j.neuroimage.2019.116137](https://doi.org/10.1016/j.neuroimage.2019.116137)
- Garyfallidis et al. Dipy, a library for the analysis of diffusion MRI data. *Frontiers in Neuroinformatics* 8 (2014). [doi:10.3389/fninf.2014.00008](https://doi.org/10.3389/fninf.2014.00008)
- Hayashi S, Caron BA, Heinsfeld AS, et al. brainlife.io: a decentralized and open-source cloud platform to support neuroscience research. *Nature Methods* 21:809–813 (2024). [doi:10.1038/s41592-024-02237-2](https://doi.org/10.1038/s41592-024-02237-2)
- Avesani P, McPherson B, Hayashi S, et al. The open diffusion data derivatives, brain data upcycling via integrated publishing of derivatives and reproducible open cloud services. *Scientific Data* 6:69 (2019). [doi:10.1038/s41597-019-0073-y](https://doi.org/10.1038/s41597-019-0073-y)

## Authors

- Brent McPherson
- Lindsey Kitchell
- Soichi Hayashi
- Franco Pestilli (project director)
- Paolo Avesani

## Funding

[![Wellcome-226486/Z/22/Z](https://img.shields.io/badge/Wellcome-226486%2FZ%2F22%2FZ-purple.svg)](https://wellcome.org/)
[![NIH-NINDS-U24NS140384](https://img.shields.io/badge/NIH_NINDS-U24NS140384-green.svg)](https://reporter.nih.gov/project-details/11261037)
[![NIH-NINDS-UM1NS132207](https://img.shields.io/badge/NIH_NINDS-UM1NS132207-green.svg)](https://reporter.nih.gov/project-details/11411328)
[![NIH-NIBIB-R01EB029272](https://img.shields.io/badge/NIH_NIBIB-R01EB029272-green.svg)](https://reporter.nih.gov/project-details/10262925)
[![NIH-NIBIB-R01EB030896](https://img.shields.io/badge/NIH_NIBIB-R01EB030896-green.svg)](https://reporter.nih.gov/project-details/10428625)
[![NIH-NIMH-R01MH126699](https://img.shields.io/badge/NIH_NIMH-R01MH126699-green.svg)](https://reporter.nih.gov/project-details/10460628)
[![NIH-NIDCD-R01DC020088](https://img.shields.io/badge/NIH_NIDCD-R01DC020088-green.svg)](https://reporter.nih.gov/project-details/11140284)
[![NSF-BCS-1734853](https://img.shields.io/badge/NSF_BCS-1734853-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1734853)
[![NSF-BCS-2203524](https://img.shields.io/badge/NSF_BCS-2203524-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2203524)
[![NSF-IIS-1636893](https://img.shields.io/badge/NSF_IIS-1636893-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1636893)
[![NSF-OAC-2148729](https://img.shields.io/badge/NSF_OAC-2148729-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148729)
[![NSF-IIS-1912270](https://img.shields.io/badge/NSF_IIS-1912270-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1912270)
[![NSF-IIS-2148700](https://img.shields.io/badge/NSF_IIS-2148700-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=2148700)
[![NSF-OAC-1916518](https://img.shields.io/badge/NSF_OAC-1916518-blue.svg)](https://www.nsf.gov/awardsearch/showAward?AWD_ID=1916518)

<sub>brainlife.io is publicly funded, and acknowledging that support helps keep it running. This App was developed with support from Wellcome, the U.S. National Institutes of Health (including the BRAIN Initiative BRAIN CONNECTS program: The Axonal Projectome EXchange, U24NS140384, and the Center for Mesoscale Connectomics, UM1NS132207) and the U.S. National Science Foundation.</sub>

## License

[MIT](LICENSE). MRtrix, FreeSurfer, FSL and DIPY are distributed under their own licenses.
