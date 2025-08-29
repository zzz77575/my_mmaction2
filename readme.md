# MMAction2 Custom Video Training & Inference Guide

---

## Environment Setup

- Follow MMAction2 installation guide:  
  https://mmaction2.readthedocs.io/en/latest/get_started/installation.html

- The following package versions are compatible with CUDA 12.6 + python 3.8:

| Package      | Version          | Install Command                                                                                   |
|--------------|------------------|-------------------------------------------------------------------------------------------------|
| mmcv         | 2.0.1            | `pip install mmcv==2.0.1 -f https://download.openmmlab.com/mmcv/dist/cu113/torch1.11/index.html` |
| torch        | 1.11.0 + cu113   | `pip install torch==1.11.0+cu113 torchvision==0.12.0+cu113 torchaudio==0.11.0 --extra-index-url https://download.pytorch.org/whl/cu113` |

---


## Step 1: Clone MMAction2 Repo

```bash
git clone https://github.com/open-mmlab/mmaction2.git
```

## Step 2: Prepare Video Dataset

- Create folders under `data/myvideo`:
  - `videos_train`
  - `videos_val`

- Organize videos into corresponding action folders inside `videos_train` and `videos_val`.

---

## Step 3: Config & Annotation Preparation

- generate annotations  files:  
  - `myvideo_train_list_videos.txt`  
  - `myvideo_val_list_videos.txt`

- Place these files under `data/myvideo`

- Put `classInd.txt` under `data/myvideo/myvideo`

- Copy the config file:  
  `configs/recognition/swin/swin-base-p244-w877_in1k-pre_8xb8-amp-32x2x1-30e_kinetics400-rgb.py`  
  Rename to `my_swin.py`

- Modify in `my_swin.py` (around lines 16-20):  
  - `data_root`  
  - `data_root_val`  
  - `ann_file_train`  
  - `ann_file_val`  
  - `ann_file_test`

- Modify number of classes in model config:  
  - Open `configs/_base_/models/swin_tiny.py`  
  - Change `num_classes` (line 25) to the actual number of classes

---

## Step 4: Training & Testing

- Follow the official MMAction2 training and testing guide:  
  https://mmaction2.readthedocs.io/en/latest/user_guides/train_test.html

---

## Step 5: Action Inference on Long Videos

- Follow `demo/README.md` to run inference on long videos.

- Run `long_video_demo.py` with your config, checkpoint, video, and label map to get `.json` or `.mp4` results.

- **Bug fix:**  
  Directly using `test_pipeline` with `Resize` (scale=(-1, 224)) and `CenterCrop` raises an error.

- **Solution:**  
  Follow Kataglyphis's answer in  mmaction2 [Issue #2783](https://github.com/open-mmlab/mmaction2/issues/2783) to add fix lines in `long_video_demo.py`.
---

## VSCode Video Preview Tip

- VSCode supports previewing **H.264-encoded MP4** videos.

- It is convenient to keep videos encoded in H.264 format for easy preview in VSCode.


