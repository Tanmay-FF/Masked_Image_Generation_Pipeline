
# Masked Image Generation Pipeline (Dockerized)

![Docker](https://img.shields.io/badge/Docker-Ready-blue?logo=docker)
![CUDA](https://img.shields.io/badge/CUDA-12.1-green?logo=nvidia)
![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python)
![OpenCV](https://img.shields.io/badge/OpenCV-Enabled-green)
![dlib](https://img.shields.io/badge/dlib-Face%20Landmarks-orange)
![Platform](https://img.shields.io/badge/Platform-Linux%20%7C%20Windows-lightgrey)
![License](https://img.shields.io/badge/License-Internal-yellow)


---

# Masked Image Generation Pipeline (Dockerized)

This repository contains a **single-process, optimized image augmentation pipeline** for generating masked face images using **dlib-based facial landmark detection**.
The pipeline is fully **Dockerized** for reproducibility and portability.

---

## ✨ Key Features

* Single-process optimized execution (no multiprocessing overhead)
* Heavy resources (dlib detector & predictor) initialized **once**
* Randomized mask type, color, offset, and rotation per image
* Automatic ZIP generation of outputs
* Docker-based execution (no environment setup on host)
* OS-agnostic (Windows / Linux / macOS)

---

## 📁 Directory Structure

```text
.
├── assets/
│   └── models/
│       └── shape_predictor_68_face_landmarks.dat
├── utils/
│   └── aux_functions.py
├── params.yaml
├── generate_masked_images.py
├── requirements.txt
├── Dockerfile
└── README.md
```

---

## ⚙️ Configuration (`params.yaml`)

Only **two fields** need to be updated when running the pipeline:

```yaml
input_dir: "/inputs"
output_dir: "/outputs"
```

All other parameters control mask type, color, offsets, and logging behavior but need not to change.

### Important Notes

* Paths **must be Linux-style** (Docker container paths)
* Do **not** use Windows paths (`C:/Users/...`) inside the container

---

## 🐳 Docker Setup

### Build the Docker image

```bash
docker build -t masked-image-pipeline .
```

---

## ▶️ Running the Pipeline

### Windows

```bash
docker run --gpus all ^
  -v <input_folder_path>:/inputs ^
  -v <output_folder_path>:/outputs ^
  masked-image-pipeline \
  python3 generate_masked_images.py
```

### Linux / macOS

```bash
docker run --gpus all \
  -v ~/Wakefern_dataset/probes/white_male/109:/inputs \
  -v ~/wakefern_masked_new:/outputs \
  masked-image-pipeline \
  python3 generate_masked_images.py
```

---

## 📤 Outputs

All results are written to `/outputs` or the output that is mapped to your local machine:

```text
outputs/
├── outputs/
├── missed_images.txt
└── outputs.zip
```

### Generated Files

* **Masked images** (`*_augmented.png`)
* **missed_images.txt** – images where face detection failed
* **outputs.zip** – zipped archive of all outputs

---

## 🧠 Pipeline Overview

1. Load configuration from `params.yaml`
2. Load dlib face detector and landmark predictor once
3. Iterate through all images sequentially
4. Apply randomized mask augmentation
5. Save outputs and logs
6. Zip the final output directory

---

## ❗ Requirements

* Docker
* NVIDIA GPU + CUDA drivers (for GPU support)
* NVIDIA Container Toolkit (`--gpus all`)

---

## 🧪 Debugging Tips

* If no output appears, verify:

  * Input directory contains valid images
  * Predictor model path exists inside container
    
* Check `missed_images.txt` for failed detections

---


## 👤 Maintainer

**Tanmay Thaker**
Machine Learning Engineer
Gatekeeper Systems

---


