# DGU-HAO: A Dataset with Daily Life Objects for Comprehensive 3D Human Action Analysis

This repository holds the codebase, dataset, and models for the work:
**MMNet: A Model-based Multimodal Network for Human Action Recognition in RGB-D Videos**
Bruce X.B. Yu, Yan Liu, Xiang Zhang, Sheng-hua Zhong, Keith C.C. Chan, TPAMI 2022 ([PDF](https://ieeexplore.ieee.org/document/9782511))

We built the **DGU-HAO: A Dataset with Daily Life Objects for Comprehensive 3D Human Action Analysis** dataset and validated it using the **MMNet: A Model-based Multimodal Network for Human Action Recognition in RGB-D Videos**.

## Dataset Download


## Dataset Structure
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


