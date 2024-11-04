<div align="center">
<h2>HSDA: High-frequency Shuffle Data Augmentation for Bird’s-Eye-View Map Segmentation</h2>
  
**Calvin Glisson** · **Qiuxiao Chen**

California State University, San Bernardino

**WACV 2025**

<img src="HSDA.png" width="75%" height="75%">

With runnable code for the paper **Residual Graph Convolutional Network for Bird’s-Eye-View Semantic Segmentation** as well.

</div>

## Getting Started
#### Data Download
Please download the [nuscenes](https://www.nuscenes.org/) dataset in `data/nuscenes` with the following files present.
```
data
│   nuscenes
│   ├── maps
│   ├── samples
│   ├── sweeps
|   ├── v1.0-trainval
```

#### Environment Installation
We provide a dockerfile for simple setup of the environment.
```bash
# (e.g. docker build -t hsda /share/docker_files/HSDA/docker)
docker build -t $ImageName $docker_file_path/
# (e.g. docker run -it --name hsda_container --shm-size=8g --gpus all --mount type=bind,source=/share/docker_files,target=/share/code hsda /bin/bash)
docker run -it --name hsda_container --shm-size=8g --gpus all --mount type=bind,source=/share/docker_files,target=/share/code $ImageName /bin/bash
# now inside docker:
pip install --no-cache-dir -v -e .
```

#### Dataset Preparation
```bash
# Generate annotations for the nuscenes dataset.
python tools/create_data.py nuscenes --root-path data/nuscenes --out-dir data/nuscenes --extra-tag nuscenes --bev True
# Generate new dataset with HSDA shuffled camera images.
# This command may take a while.
# If it is interrupted while running, simply re-run the script and it will resume where it left off.
python prepare-hsda-dataset.py
# Generate annotations for the HSDA dataset.
python tools/create_data.py nuscenes --root-path data/nuscenes-hsda --out-dir data/nuscenes-hsda --extra-tag nuscenes --bev True
```

#### Training
```bash
# Single-GPU
python train.py $config
# Multi-GPU
./dist_train_gpu.sh $config $num
# Example: train baseline+HSDA with 2 gpus
./dist_train_gpu.sh configs/bevdet_hsda/bevdet-multi-map-aug-seg-only-6class-hsda.py 2
```

#### Testing
```bash
# We are interested only in the map results.
python test.py $config $pth --eval=bboxmap
# Example: test baseline+HSDA after training it
python test.py configs/bevdet_hsda/bevdet-multi-map-aug-seg-only-6class-hsda.py work_dirs/bevdet-multi-map-aug-seg-only-6class-hsda/epoch_20.pth --eval=bboxmap
```

## Bibtex
If this work is helpful for your research, please consider citing the following BibTeX entry.
```
(will be added after official publication is done)
```

