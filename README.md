# DGU-HAO: A Dataset with Daily Life Objects for Comprehensive 3D Human Action Analysis ([PDF](https://ieeexplore.ieee.org/document/10385044?source=authoralert))

This repository holds the codebase, dataset, and models for the work:
**MMNet: A Model-based Multimodal Network for Human Action Recognition in RGB-D Videos**
Bruce X.B. Yu, Yan Liu, Xiang Zhang, Sheng-hua Zhong, Keith C.C. Chan, TPAMI 2022 ([PDF](https://ieeexplore.ieee.org/document/9782511))

We built the **DGU-HAO: A Dataset with Daily Life Objects for Comprehensive 3D Human Action Analysis** dataset and validated it using the **MMNet: A Model-based Multimodal Network for Human Action Recognition in RGB-D Videos**.

## Dataset Download

### Raw Data & Labeled Data
[Data access](https://farmnas.synology.me:6953/sharing/IhijV5Rkr)
To access the data, please use a VPN to change your location to South Korea and then access the link above.

### Pre-processed Skeleton data
We pre-processed human motion capture data (bvh, json) to the skeleton data with 25 joints.

We have 67,505 samples of pre-processed skeleton data.

[Pre-processed skeleton data access](https://farmnas.synology.me:6953/sharing/xhc6fCBxZ)


## Dataset Structure
Regular expression of the data file name

![image](https://github.com/CSID-DGU/NIA-MoCap-1/assets/46514182/7e0dab6f-a4d3-444b-80a0-8f139b9f328f)

```
.
├── raw_data
│   ├── EL
│   │   │── HE
│   │   │   ├── HE01
│   │   │   │   ├── A16
│   │   │   │   │   ├── F
│   │   │   │   │   │   ├── MH01
│   │   │   │   │   │   │   ├── .jpg
│   │   │   │   │   │   │   ├── .mp4
│   │   │   │   │   │   │   ├── .bvh
│   │   │   │   │   │   │   ├── .qtm
│   │   │   │   │   │   │   ├── .jpg
│   │   │   │   │   │   │   ├── .mp4
│   │   │   │   │   │   │   ├── .bvh
│   │   │   │   │   │   │   ├── .qtm
│   │   │   │   │   │   │   └── ...
│   │   │   │   │   │   └── ...
│   │   │   │   │   └── M
│   │   │   │   └── ...
│   │   │   └── ...
│   │   │
│   │   └── OE
│   │       │── ...
│   ├── FN
│   │   │── ...
│   └── 3D_object_modeling
│       │── CH
│       │   │── CH01
│       │   │   │── BaseColor.png
│       │   │   │── Metallic.png
│       │   │   │── Normal.png
│       │   │   │── Roughness.png
│       │   │   └── .fbx
│       │   └── ...
│       └── ...
│ 
└── labeled_data
    ├── EL
    │   │── HE
    │   │   ├── HE01
    │   │   │   ├── A16
    │   │   │   │   ├── F
    │   │   │   │   │   ├── MH01
    │   │   │   │   │   │   ├── .json
    │   │   │   │   │   │   ├── ...
    │   │   │   │   │   │   └── .json
    │   │   │   │   │   └── ...
    │   │   │   │   └── M
    │   │   │   └── ...
    │   │   └── ...
    │   │
    │   └── OE
    │       │── ...
    └── FN
        │── ...
```

### Action Classes (TBU)


## Environment Setting

### 1. Clone the repository
``` shell
git clone https://github.com/CSID-DGU/NIA-MoCap-1.git
cd NIA-MoCap-1
```

### 2. Install the requriements
#### Prerequisites
- Python3 (>3.5)
- [PyTorch](http://pytorch.org/) depending on user's machine.
``` shell
pip install -r requirement.txt
```
``` shell
cd torchlight; python setup.py install; cd ..
```

### 3. Dataset Citation
```
@article{park2024dgu,
  title={DGU-HAO: A Dataset With Daily Life Objects for Comprehensive 3D Human Action Analysis},
  author={Park, Jiho and Kim, Junghye and Gil, Yujung and Kim, Dongho},
  journal={IEEE Access},
  year={2024},
  publisher={IEEE}
```

