# PyTorch with YOLO

> **Minimal, production-ready PyTorch base images for GPU and CPU workloads — with optional pre-bundled YOLO weights for offline inference.**

[![Docker Pulls](https://img.shields.io/docker/pulls/pytorchlab/pytorch)](https://hub.docker.com/r/pytorchlab/pytorch)
[![Docker Image Size](https://img.shields.io/docker/image-size/pytorchlab/pytorch)](https://hub.docker.com/r/pytorchlab/pytorch)
[![License](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

---

## Overview

`pytorchlab/pytorch` provides slim, multi-variant Docker images on top of the official [pytorch/pytorch](https://hub.docker.com/r/pytorch/pytorch) base. Each image adds only what is needed for real-world deployments:

- **System libraries** required by OpenCV (`libGL`, `libglib2.0`, `libpq`, `libgeos`)
- **Clean layer** — `__pycache__` removed at build time to minimize image size
- **YOLO variants** — pre-downloaded Ultralytics weights bundled inside the image so that ECS/Kubernetes workers have zero outbound dependency at runtime

---

## Available Tags

### GPU (CUDA 12.6 + cuDNN 9)

> Built from `pytorch/pytorch:2.9.1-cuda12.6-cudnn9-runtime`. Requires NVIDIA drivers compatible with CUDA 12.6+.

| Tag | Description |
|---|---|
| `2.9.1-cuda12.6-cudnn9-runtime` | PyTorch + CUDA + system libs. Generic GPU base. |
| `2.9.1-cuda12.6-cudnn9-yolo26m` | Above + YOLO v2.6m & v2.6n weights in `/app/weights` (Ultralytics offline). |
| `2.9.1-cuda12.6-cudnn9-yolo11m` | Above + YOLO v11m & v2.6n weights in `/app/weights` (Ultralytics offline). |

### CPU (Python 3.11 Slim)

> Built from `python:3.11-slim` with CPU-only PyTorch (~1.5 GB lighter than the CUDA variant).

| Tag | Description |
|---|---|
| `2.4.1-cpu-py3.11-slim` | PyTorch (CPU-only) + system libs. Generic CPU base. |
| `cpu-python3.11-yolo26m` | Above + YOLO v2.6m & v2.6n weights in `/app/weights` (Ultralytics offline). |
| `cpu-python3.11-yolo11m` | Above + YOLO v11m & v2.6n weights in `/app/weights` (Ultralytics offline). |

---

## Quick Start

```bash
# GPU — verify CUDA is available
docker pull pytorchlab/pytorch:2.9.1-cuda12.6-cudnn9-runtime
docker run --gpus all --rm pytorchlab/pytorch:2.9.1-cuda12.6-cudnn9-runtime \
  python -c "import torch; print('CUDA:', torch.cuda.is_available())"

# CPU — lightweight inference
docker pull pytorchlab/pytorch:2.4.1-cpu-py3.11-slim
docker run --rm pytorchlab/pytorch:2.4.1-cpu-py3.11-slim \
  python -c "import torch; print('PyTorch:', torch.__version__)"
```

---

## Use as a Base Image

### GPU workload

```dockerfile
FROM pytorchlab/pytorch:2.9.1-cuda12.6-cudnn9-runtime

WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .
CMD ["python", "train.py"]
```

### CPU workload / lightweight inference

```dockerfile
FROM pytorchlab/pytorch:2.4.1-cpu-py3.11-slim

WORKDIR /app
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY . .
CMD ["python", "inference.py"]
```

### Offline YOLO inference (no internet required at runtime)

```dockerfile
FROM pytorchlab/pytorch:2.9.1-cuda12.6-cudnn9-yolo26m

# Weights are pre-downloaded at /app/weights/yolo26m.pt & yolo26n.pt
# YOLO_OFFLINE=1 and YOLO_CONFIG_DIR=/app/weights are already set
WORKDIR /app
COPY . .
CMD ["python", "detect.py"]
```

---

## Requirements

| Requirement | Notes |
|---|---|
| Docker Engine 20.10+ | Tested on Docker Desktop & Docker CE |
| NVIDIA Container Toolkit | GPU tags only — [install guide](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html) |
| NVIDIA driver ≥ 525 | Required for CUDA 12.6 compatibility |

---

## Environment Variables (pre-set in YOLO variants)

| Variable | Value | Description |
|---|---|---|
| `YOLO_CONFIG_DIR` | `/app/weights` | Ultralytics config & weights directory |
| `YOLO_OFFLINE` | `1` | Disables Ultralytics auto-download at runtime |

---

## License

- **Image build scripts**: [MIT License](LICENSE)
- **PyTorch**: [BSD-style License](https://github.com/pytorch/pytorch/blob/main/LICENSE)
- **Ultralytics YOLO** (weights in YOLO variants): [AGPL-3.0 License](https://github.com/ultralytics/ultralytics/blob/main/LICENSE)

> **Note:** The YOLO weight files bundled in the YOLO-variant images are subject to the Ultralytics AGPL-3.0 license. Ensure your use case complies with this license.
