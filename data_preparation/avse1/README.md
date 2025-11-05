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

```bash
# Clone repository
git clone https://github.com/cogmhear/avse_challenge.git
cd avse-challenge

# Create & activate environment with conda, see https://docs.conda.io/projects/conda/en/latest/user-guide/install/index.html
conda create --name avse python=3.8
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
- noise maskers and metadata (AVSEC-3):
https://data.cstr.ed.ac.uk/cogmhear/protected/avsec3_data.tar  
Please register for the AVSE challenge to obtain the download credentials: [registration form](https://challenge.cogmhear.org/#/getting-started/register)

Noise maskers and metadata (AVSEC-1 and AVSEC-2): https://data.cstr.ed.ac.uk/cogmhear/protected/avse2_data.tar

**Note that the AVSEC-2 dataset is identical to that used in the 1st edition of the Challenge, <avse1_data_v2.tar>**

2) Set up data structure and create speech maskers (see EDIT_THIS to change local paths):
```bash
cd data_preparation/avse1
./setup_avse1_data.sh 
```

3) Change root path defined in [data_preparation/avse1/data_config.yaml](data_preparation/avse1/data_config.yaml) to the location of the data.

4) Prepare noisy data:
```bash
cd data_preparation/avse1
python prepare_avse1_data.py 
```

## Baseline

[code](./baseline/avse1/)

[pretrained_model](https://data.cstr.ed.ac.uk/cogmhear/protected/avse1_baseline.ckpt)

The credentials to download the pretrained model are the same as the ones used to download the noise maskers and the metadata.

## Evaluation

We provide a script to extract STOI and PESQ for the devset.

Note: before running this script please edit the paths and file name formats defined in evaluation/avse1/config.yaml (see EDIT_THIS).

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

- The 1st Audio-Visual Speech Enhancement Challenge (AVSE1)  
[data_preparation](./data_preparation/avse1/)  
[baseline](./baseline/avse1/)  
[evaluation](./evaluation/avse1/)  

## License

Videos are derived from:
- [LRS3 dataset](https://mm.kaist.ac.kr/datasets/lip_reading/)  
Creative Commons BY-NC-ND 4.0 license

Interferers are derived from:
- [Clarity Enhancement Challenge (CEC1)](https://github.com/claritychallenge/clarity/tree/main/recipes/cec1)  
Creative Commons Attribution Share Alike 4.0 International

- [DEMAND](https://zenodo.org/record/1227121#.YpZHLRPMLPY):  
Creative Commons Attribution 4.0 International

- [DNS Challenge second edition](https://github.com/microsoft/DNS-Challenge).  
Only Freesound clips were selected   
Creative Commons 0 License

- [LRS3 dataset](https://mm.kaist.ac.kr/datasets/lip_reading/)  
Creative Commons BY-NC-ND 4.0 license

- [MedleyDB audio](https://medleydb.weebly.com/)

The dataset is licensed under CC BY-NC-SA 4.0.

- [ESC-50 Dataset for Environmental Sound Classification](https://github.com/karolpiczak/ESC-50)

Creative Commons Attribution-NonCommercial license

Data preparation scripts were adapted from original code by [Clarity Challenge](https://github.com/claritychallenge/clarity). Modifications include: extracting target target audio from video and different settings for sampling rate (16kHz), number of channels (one channel) and scenes simulation (additive noise only, no room impulse responses and no room simulation).


