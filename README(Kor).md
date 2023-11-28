# 가구가전사무기기 사용 모션캡처 데이터 검증

This repository holds the codebase, dataset and models for the work:
**MMNet: A Model-based Multimodal Network for Human Action Recognition in RGB-D Videos**
Bruce X.B. Yu, Yan Liu, Xiang Zhang, Sheng-hua Zhong, Keith C.C. Chan, TPAMI 2022 ([PDF](https://ieeexplore.ieee.org/document/9782511))


본 검증 과정은 가구가전사무기기 사용 모션캡처 데이터를 **MMNet: A Model-based Multimodal Network for Human Action Recognition in RGB-D Videos** 모델을 활용하여 검증하였다.

본 검증 과정은 크게 아래와 같이 5단계로 이루어져있다.
* 검증 환경 세팅
* 원천 데이터 전처리
* 데이터셋 세팅 (train/test dataset)
* Train
* Test


## 검증 환경 세팅

### 1. 아래의 명령어를 사용하여 github에서 소스코드를 clone 받는다.
``` shell
git clone https://github.com/CSID-DGU/NIA-MoCap-1.git
cd NIA-MoCap-1
```

### 2.아래의 명령어를 사용하여 `requirement.txt`에 있는 모듈을 모두 설치한다.
#### Prerequisites
- Python3 (>3.5)
- [PyTorch](http://pytorch.org/)
``` shell
pip install -r requirement.txt
```
다만, 검증하는 PCdml GPU 사양에 따라 `torch` 및 `torchvision` 모듈의 버전 수정은 가능하다.
``` shell
cd torchlight; python setup.py install; cd ..
```
### 3. 원천 데이터셋을 다운받는다. 
이때, 다운받은 경로는 이후 단계에서 활용되기 때문에 데이터셋 경로를 따로 저장해둔다.

## skeleton 원천 데이터 전처리
원천 데이터를 모델의 입력 데이터로 사용하기 위해 전처리하는 단계로, 구간태깅 정보가 있는 `*.json` 파일을 활용하여 `*.bvh` 파일을 `*.csv` 파일로 변환한 후, `*.txt` 파일로 저장한다.

### 1. 아래 명령어를 사용하여 디타스에서 제공한 bvh file의 리스트를 저장한다.
본 코드는 원천 데이터가 저장된 폴더에서 `bvh`, `json`이 모두 존재하는 파일만 추출하여 저장한다. `GetList.py`가 위치한 경로에 `Updated_bvhlist`라는 폴더를 만들어 `txt`를 저장한다. 이때 병렬 처리를 위해, `txt`에는 파일 목록이 1000개씩 저장되고, 이러한 `txt`들은 `bvh_list_{num}.txt`로 저장된다.
``` shell
cd tools/skeleton_preprocessing
mkdir Updated_bvhlist
python GetList.py
```

### 2. `bvh2Csv_{num}.py` 를 실행하면 `bvh_list_{num}.txt`와 일치하는 `num`에 대해 `bvh`파일을 `csv`파일로 변환한다.
``` shell
python bvh2Csv_{num}.py
```

### 3. `csv_to_skeleton_{num}.py`를 실행하면 `bvh_list_{num}.txt`의 파일 목록 내 일대일 매칭되는 csv 파일을 skeleton 파일로 변환한다.
``` shell
python csv_to_skeleton_{num}.py
```

## 데이터셋 세팅 (train/test dataset)
### 1. `NIA-MoCap-1/tools/data_gen/dtaas_gendata.py` 파일을 확인한다.
``` shell
cd NIA-MoCap-1/tools/data_gen
vim dtaas_gendata.py
```

### 2. 로컬 환경에 맞게 아래에서 언급한 소스코드 내의 데이터 경로를 수정한다.
위에서 저장한 검증할 `skeleton(*.txt)` 파일 경로를 원래 코드에 입력되어있는 `/home/irteam/YJ2/Final_skeletons/` 대신 입력한다. 
``` shell
parser.add_argument('--data_path', default='{your_path}')
```
해당 폴더 내에 `skeleton` 형식이 아닌 다른 파일이 있다면, 데이터셋 생성시 포함되지 않아야 하므로 `NIA-Mocap-1/resource/ignore_files.txt`에 파일명을 추가한다.

혹은 자체적으로 포함되지 않아야하는 파일명 목록을 만든 후, 아래의 코드에 입력되어있는 파일 경로를 변경한다.
``` shell
parser.add_argument('--ignored_sample_path', default='{your_path}')
```

최종적으로 train/test set을 저장할 경로를 입력한다.
``` shell
parser.add_argument('--out_folder', default='{your_path}')
```

### 3. dtaas_gendata.py를 실행한다.
`skeleton` 형식으로 전처리된 원천데이터를 모델의 `Input` 형식에 맞게 전처리한다.
``` shell
cd ../..
python tools/data_gen/dtaas_gendata.py
```

### 4. 전처리완료 후 생성된 파일 체크
위에서 설정한 `out_folder`의 경로에 `train_data_joint.npy`, `train_label.pkl`, `val_data_joint.npy`, `val_label.pkl` 이 제대로 생성되었는지 체크한다.

## Train
### 1. `NIA-MoCap-1/config/ntu60_xsub/train_joint.yaml` 파일을 확인한다.
``` shell
cd NIA-MoCap-1/config/ntu60_xsub
vim train_joint.yaml
```

### 2. 로컬 환경에 맞게 아래에서 언급한 소스코드 내의 경로를 수정한다.
model을 학습할 때 도출되는 결과물이 저장되는 경로이다. `log.txt` 파일과 학습된 파라미터 `*.pt` 파일 등이 저장된다.
``` shell
work_dir: {your_path}
```

위의 데이터셋 세팅 단계의 마지막에 생성된`train dataset`과 `test dataset`의 경로도 설정해준다. `*.npy`에는 `skeleton` 정보를 `numpy`로 변환한 결과가, `*.pkl` 파일에는 `numpy` 파일의 `label(Action class)` 정보가 명시되어있다.

```
train_feeder_args:
  centralization: False
  random_move: False
  if_bone: False
  data_path: {your_path}/train_data_joint.npy
label_path: {your_path}/train_label.pkl
test_feeder_args:
  centralization: False
  if_bone: False
  data_path: {your_path}/val_data_joint.npy
  label_path: {your_path}/val_label.pkl
```

Training 환경 세팅에 관한 매개변수들도 각자 시험 환경에 맞게 설정해준다. 본 검증 환경에서는 0~4번까지 총 5개의 GPU를 사용하였고, train, test batch size는 8로 진행하였으며, num_epoch은 14로 진행하였다. 
```
device: [0,1,2,3,4] // gpu 개수 
batch_size: 8
test_batch_size: 8
num_epoch: 14
```
gpu 개수가 다른 경우 `NIA-MoCap-1/processor/processor.py`에서 아래의 `default` 숫자를 gpu 숫자에 맞게 변경한다.
```
parser.add_argument('--num_worker', type=int, default={your_gpu_num}, help='the number of worker per gpu for data loader')
```

### 3. 아래의 명령어로 학습을 진행한다.
``` shell
python main_skeleton.py recognision -c config/ntu60_xsub/train_joint.yaml
```

## Test
처음 단계부터 직접 수행한 경우, 1번은 건너 뛴다. 데이터 검증을 위해 바로 Test 단계를 진행하는 경우에만 1번 사항을 수행하면 된다.

### 1. 아래의 링크에서 유효성 증빙 목적의 `평가용 데이터셋` 폴더에 저장된 `preprocessed_test_final` 폴더 전체를 다운받는다. (Optional)
[평가용 전처리 데이터셋 다운로드 링크](https://farmnas.synology.me:6953/sharing/wHyDgQkhu)
다운받은 폴더 내의 `val_data_joint.npy`, `val_label.pkl` 파일을 확인한다.

### 2. `NIA-MoCap-1/config/ntu60_xsub/test_joint.yaml` 파일을 확인한다.
``` shell
cd NIA-MoCap-1/config/ntu60x_xsub
vim test_joint.yaml
```

### 3. 로컬 환경에 맞게 yaml 파일 내의 경로를 수정한다.
Test 결과를 저장할 경로를 설정한다.
```
work_dir: {your_path}
```

Test 하려는 model의 경로이다. 학습된 모델의 파라미터를 아래의 경로에서 다운받아 다운받은 경로를 넣어준다.
[모델 파라미터 다운로드 링크](https://farmnas.synology.me:6953/sharing/6ZtqlpGlx)
```
weights: {your_path}/epoch14_model.pt
```

다음으로는 `test dataset`을 로드하기 위해 `test datset` 경로를 세팅한다.
```
test_feeder_args:
  centralization: False
  if_bone: False
  data_path: {your_path}/val_data_joint.npy
  label_path: {your_path}/val_label.pkl
```

Test 하는 환경에 관한 정보이다. 본 검증환경에서는 앞서 Train 파트에서 설정한 것과 동일하게 설정하였다. 
<u>검증 결과를 동일하게 얻기 위해서는 본 검증환경 세팅과 동일하게 진행하여야 하며, 수정하는 경우 결과가 다르게 나올 수 있다.</u>
```
phase: test
device: [0,1,2,3,4]
test_batch_size: 8
```

### 4. 아래의 명령어로 테스트를 시작한다.
``` shell
python main_skeleton.py recognition -c config/ntu60_xsub/test_joint.yaml
```

### 5. 결과 확인
결과는 위에서 지정한 `work_dir` 폴더에서 확인하면 된다.

