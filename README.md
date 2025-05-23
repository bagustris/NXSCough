# 🔊 NEXUS Cough Classification

This repository contains a training pipeline for a Cough classification model using Various Model. It supports audio augmentation via the [RIRS noise dataset](https://www.openslr.org/28/) for realistic room simulation.

---
## :trophy: Getting Started

| Model | Accuracy | Balanced Accuracy | AUC | ROC AUC | F1 Score Accuracy |
|:-----:|:--------:|:-----------------:|:---:|:-------:|:-----------------:|
| LSTM | 0.87 | 0.88 | 0.95 | 0.88 | 0.87 |
| ResNet101 | 0.91 | 0.91 | 0.97 | 0.91 | 0.91 |
| InceptionV3 | 0.92 | 0.92 | 0.98 | 0.92 | 0.92 |

---

## :rocket: Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/arkiven4/NXSCough
cd NXSCough
```

### 2. Preparation
Install dependencies with:
```bash
pip install -r requirements.txt
```
Before training, ensure that the path to your dataset is correctly set in your config in the `config/lstm.json` folder:

```bash
"db_path": "/run/media/fourier/Data1/Pras/Database_ThesisNew/data/"
```
This folder must contain:

- somedatasets.csv: includes audiopath, labels
- audiodata folder: contains the actual audio files or processed data

```
/run/media/fourier/Data1/Pras/Database_ThesisNew/
├── somedatasets.csv
└── audiodata/
    ├── file1.wav
    └── file2.wav
```
Update the path according to your environment before running the training script.


### 3. Train the Model

```bash
python ztrain_nonssl.py
```

Training logs will be saved to the ./logs/ directory. You can Check with this command:


```sh
tensorboard --logdir ./logs/lstm_try1
```
Open the provided [localhost](http://localhost:6006) link in your browser to monitor loss, accuracy, and other metrics during training.

After training is complete, please check the `logs` folder.

## :rocket: Getting Started For Singularity User

Follow the steps below to clone the repository, build the Singularity image, bind necessary folders, and execute your application.
### 1. Clone the Repository

```bash
git clone https://github.com/arkiven4/NXSCough
cd NXSCough
```
### 2. Build the Singularity Image

```bash
singularity build nxscough.sif nxscough.def
```
### 3. Configure Config
Before training, ensure that the path to your dataset is correctly set in your config like this:

```bash
"db_path": "/mnt/data/"
```

### 4. Run Training

```bash
sudo singularity exec --bind /run/media/fourier/Data1/Pras/Database_ThesisNew/:/mnt/data --nv nxscough.sif python3 ztrain_nonssl.py
```
The path `/run/media/fourier/Data1/Pras/Database_ThesisNew/` replace with path on your host machine and **must contain**:
```
/run/media/fourier/Data1/Pras/Database_ThesisNew/
├── metadata_combine.csv
└── CombineData/
├── file1.wav
├── file2.wav
└── ...
```
- `metadata_combine.csv`: A CSV file containing metadata about the audio files.
- `CombineData/`: A folder containing `.wav` files used for training.

These files will be mounted to `/mnt/data` inside the container.

### Optional: Data Augmentation with RIRS

To apply room impulse response (RIR) augmentation:

Download the RIRS dataset from:
👉 https://www.openslr.org/28/

Extract the contents and place them in a suitable path used by your config or augmentation script.
