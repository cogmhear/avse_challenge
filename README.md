# Audio-Visual Speech Enhancement Challenge (AVSE)

Human performance in everyday noisy situations is known to be dependent upon both aural and visual senses that are contextually combined by the brain’s multi-level integration strategies. The multimodal nature of speech is well established, with listeners known to unconsciously lip read to improve the intelligibility of speech in a real noisy environment. Studies in neuroscience have shown that the visual aspect of speech has a potentially strong impact on the ability of humans to focus their auditory attention on a particular stimulus.

Over the last few decades, there have been major advances in machine learning applied to speech technology made possible by Machine Learning related Challenges including CHiME, REVERB, Blizzard, Clarity and Hurricane. However, the aforementioned challenges are based on single and multi-channel audio-only processing and have not exploited the multimodal nature of speech. The aim of this first audio visual (AV) speech enhancement challenge is to bring together the wider computer vision, hearing and speech research communities to explore novel approaches to multimodal speech-in-noise processing.

In this repository, you will find code to support the AVSE Challenge, including the baseline and scripts for preparing the necessary data.

More details can be found on the challenge website:
https://challenge.cogmhear.org

## Announcements

Any announcements about the challenge will be made in our mailing list (avse-challenge@mlist.is.ed.ac.uk).
See [here](https://challenge.cogmhear.org/#/docs?id=announcements) on how to subscribe to it.

## Installation
*Instructions to build data from previous AVSEC{1,2,3} editions are [here](data_preparation/avse1/)*

**We are currently running the fourth edition of AVSEC**

Follow instructions below to build the **AVSEC-4** dataset

```bash

# Clone repository
git clone https://github.com/cogmhear/avse_challenge.git
cd avse_challenge

# Create & activate environment with conda, see https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html
conda create --name avse python=3.9
conda activate avse

# Install ffmpeg 2.8
conda install -c rmg ffmpeg

# Install requirements
pip install -r requirements.txt
```
## Data preparation

These scripts should be run in a unix environment and require an installed version of the [ffmpeg](https://www.ffmpeg.org) tool (required version 2.8; see Installation for the correct installation command).

1) Download necessary data:

- target videos:  
Lip Reading Sentences 3 (LRS3) Dataset  
https://mm.kaist.ac.kr/datasets/lip_reading/

Follow the instructions on the website to obtain credentials to download the videos.

- Noise maskers and metadata (AVSEC-4):
https://data.cstr.ed.ac.uk/cogmhear/protected/avsec4_data.tar  [4.1GB]

Please register for the AVSE challenge to obtain the download credentials: [registration form](https://challenge.cogmhear.org/#/getting-started/register)

Noise maskers and metadata of previous editions are available [here](data_preparation/avse1/README.md)

- Room simulation data and impulse responses from the [Clarity Challenge (CEC2)](https://github.com/claritychallenge/clarity/tree/main/recipes/cec2) and Head-Related Transfer Functions from [OlHeaD-HRTF Database](https://uol.de/mediphysik/downloads/hearingdevicehrtfs):
 https://data.cstr.ed.ac.uk/cogmhear/protected/clarity_cec2_data.tar [64GB]

<p>AVSEC-4 uses a subset of the data released by the Clarity Enhancement Challenge 2 and a subset of HRTFs of the OlHeaD-HRTF Database from Oldenburg University. 
Download the tar file above to obtain HRTFs, room simulation data and resampled (16000 Hz) impulse responses. </p>


2) Set up data structure and create speech maskers (see EDIT_THIS to change local paths):
```bash
cd data_preparation/avse4
./setup_avsec4_data.sh 
```

3) Change root path defined in [data_preparation/avse4/config.yaml](data_preparation/avse4/config.yaml) to the location of the data.

4) Prepare noisy data:

Data preparation scripts were adapted from original code by [Clarity Enhancement Challenge 2](https://github.com/claritychallenge/clarity/tree/main/recipes/cec2) under MIT License. 

```bash
cd data_preparation/avse4
python build_scenes.py
```
Tu build data locally single-run:
```bash
python render_scenes.py
```
Alternatively, if using multi-run:

[//]: # (# python render_scenes.py 'render_starting_chunk=range&#40;0, 494, 13&#41;' --multirun  )
```bash
#20 subjobs, starting in scene 0 and rendering 400 scenes
python render_scenes.py 'render_starting_chunk=range(0, 400, 20)' --multirun  
```
**Rendering binaural and/or monoaural signals**

Scripts allow you to render binaural and monoaural signals. To choose which signals to render set the corresponding parameters in the [config](data_preparation/avse4/config.yaml) file to *True* for the set of signals you want to render:
```bash
  binaural_render: True
  monoaural_render: True
```
#### Data structure

```bash
└── avsec4
    ├── dev
    │   ├── interferers
    │   ├── rooms 
    │   │   ├─ ac [20 MB]
    │   │   ├─ HOA_IRs_16k [18.8 GB]
    │   │   ├─ rpf [79 MB]
    │   ├── scenes [12 GB]
    │   ├── targets
    │   └── targets_video 
    ├── hrir
    │    ├─ HRIRs_MAT
    ├── maskers_music [607 MB]
    ├── maskers_noise [3.9 GB]
    ├── maskers_speech [5.3 GB]
    ├── metadata 
    └── train
    │    ├── interferers
    │    ├── rooms
    │    │    ├─ ac [48 MB]
    │    │    ├─ HOA_IRs_16k [45.2 GB]
    │    │    ├─ rpf [189 MB]
    │    ├── scenes [141 GB]
    │    ├── targets
    │    └── targets_video 
```

## Baseline

AVSEC-4 baseline:

[Binaural baseline](./baseline/avse4/)  

The credentials to download the pretrained model are the same as the ones used to download the noise maskers and the metadata.

## Evaluation

**Binaural signals**

We provide a script to compute MBSTOI from binaural signals. We use MBSTOI scripts from the [Clarity Challenge](https://github.com/claritychallenge/clarity/tree/main/clarity/evaluator/mbstoi). The original MBSTOI Matlab implementation is available [here.](http://ah-andersen.net/code/<http://ah-andersen.net/code/>)

```
cd evaluation/avse4/
python objective_evaluation.py
```
Note: before running this script please edit the paths and file name formats defined in evaluation/avse1/config.yaml (see EDIT_THIS).

**Monophonic signals**

To compute objective metrics using monophonic signals (i.e., STOI and PESQ) please use evaluation scripts from in AVSEC-1. 

```
cd evaluation/avse1/
python objective_evaluation.py
```
that require the following libraries:
```
pip install pystoi==0.3.3
pip install pesq==0.0.4
```

## Challenges

Current challenge

- The 4th Audio-Visual Speech Enhancement Challenge (AVSEC-4)  
[data_preparation](./data_preparation/avse4/)  
[baseline](./baseline/avse4/)  
[evaluation](./evaluation/avse4/)  

## License

Videos are derived from:
- [LRS3 dataset](https://mm.kaist.ac.kr/datasets/lip_reading/)  
Creative Commons BY-NC-ND 4.0 license.

Interferers are derived from:
- [Clarity Enhancement Challenge (CEC1)](https://github.com/claritychallenge/clarity/tree/main/recipes/cec1)  
Creative Commons Attribution Share Alike 4.0 International.

- [DNS Challenge second edition](https://github.com/microsoft/DNS-Challenge).  
Only Freesound clips were selected   
Creative Commons 0 License.

- [LRS3 dataset](https://mm.kaist.ac.kr/datasets/lip_reading/)  
Creative Commons BY-NC-ND 4.0 license.

- [MedleyDB audio](https://medleydb.weebly.com/)   
The dataset is licensed under CC BY-NC-SA 4.0.

Impulse responses and room simulation data derived from:
- [Clarity Enhancement Challenge (CEC2)](https://github.com/claritychallenge/clarity/tree/main/recipes/cec2)
The dataset is licensed under CC BY-SA 4.0.

Head-Related Transfer Functions derived from:
-  [OlHeaD-HRTF Database](https://uol.de/mediphysik/downloads/hearingdevicehrtfs):
The dataset is licensed under CC BY-NC-SA 4.0.

Scripts:

Data preparation scripts were adapted from original code by [Clarity Enhancement Challenge 2](https://github.com/claritychallenge/clarity/tree/main/recipes/cec2). Modifications include: extracting target audio from video and different settings for sampling rate (16kHz), no random starting time for target speaker and no head rotations.


