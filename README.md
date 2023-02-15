# Long Tail Point Cloud Segmentation

In this project, we adapt the well-known attention mechanism to 3D point cloud segmentation and demonstrate its effectiveness in combination with U-Net-inspired architecture and various long-tail recognition (LTR) techniques on the SCANNET200 dataset.

🎉We were voted as the best by 123 classmates and 29 teams through inter-team evaluation!🎉

Our poster:
![poster](poster.png)

## Environment Setup

Our GPU is a single 4090 with 24GB VRAM

### Option 1: Install using anaconda

```bash
cd point-transformer
bash conda_env_setup.sh pt
```

### Option 2: Manual installation

1. Install torch 1.9.0 corresponds to your cuda version (11.1 in our case)
2. `pip install h5py pyyaml sharedarray tensorboardx plyfile`

3. ```bash
    cd point-transformer/lib/pointops

    python3 setup.py install
    ```

    to install custom kernel functions

## Dataset Setup for Training

Only place the dataset if you plan to train the model. \
Place the train and test set in `point-transformer/dataset/scannet200` \
The directory should look like:

```plain text
point-transfomer/
├─ dataset/
│  ├─ scannet200/
│  │  ├─ train/
│  │  │  ├─ scene0000_00.ply
│  │  │  ├─ ...
│  │  ├─ test/
│  │  │  ├─ scene0500_00.ply
│  │  │  ├─ ...
│  │  ├─ train_split.txt
│  │  ├─ val_split.txt
│  │  ├─ test_split.txt
```

## Train

```bash
cd point-transformer
bash train.sh
```

## Inference

### Option 1: Ensemble with five models (24.2 mIoU)

Since we use five models as ensemble voting classifier, we provide five different weights to download. \
Follow the procedure to repreduce the results: \

1. `cd point-transformer` \
2. For i in [0, 1, 2, 3, 4]: \
    a. `bash download{i}.sh` \
        "\${1}" is the path to test set folder  (e.g., ./dataset/test)  \
        "\${2}" is the path to test.txt         (e.g., ./dataset/test.txt) \
        "\${3}" is the path to output folder    (e.g., ./submission) \
    b. `bash inference.sh $1 $2 $3` \
    c. Move the output folder (e.g., ./submission) into `../Ensemble/all_submissions`
3. `cd ../Ensemble`
4. Run `python3 Ensemble.py`
5. The result is in `Ensemble/ensemble_results`

### Option 2: Use the best single model (23.4 mIoU)

"\${1}" is the path to test set folder  (e.g., ./dataset/test)  \
"\${2}" is the path to test.txt         (e.g., ./dataset/test.txt) \
"\${3}" is the path to output folder    (e.g., ./submission)

Run:

```bash
cd point-transformer
bash download0.sh
bash inference.sh $1 $2 $3
```
