# halay08/pytorch

PyTorch Docker image with CUDA 12.8 and cuDNN 9 (runtime). Suitable for GPU training and inference.

## Images

| Tag | Description |
|-----|-------------|
| `2.9.1-cuda12.6-cudnn9-runtime` | PyTorch + CUDA + system libs (OpenCV). Generic base. |
| `2.9.1-cuda12.6-cudnn9-yolo26m` | Same as above + YOLO weights (yolo26m.pt, yolo26n.pt) in `/app/weights` for Ultralytics offline (e.g. ECS training). |

- **Repository:** `halay08/pytorch`
- **Base:** Built from [pytorch/pytorch](https://hub.docker.com/r/pytorch/pytorch) official image

## Build (base images)

```bash
cd pytorch-docker
# Generic GPU base (no YOLO)
docker build --platform=linux/amd64 -f Dockerfile.gpu -t halay08/pytorch:2.9.1-cuda12.6-cudnn9-runtime .
# GPU + YOLO weights (for sphere-ai training)
docker build --platform=linux/amd64 -f Dockerfile.gpu.yolo26m -t halay08/pytorch:2.9.1-cuda12.6-cudnn9-yolo26m .
docker push halay08/pytorch:2.9.1-cuda12.6-cudnn9-yolo26m
```

## Quick start

```bash
docker pull halay08/pytorch:2.9.1-cuda12.6-cudnn9-runtime
docker run --gpus all -it halay08/pytorch:2.9.1-cuda12.6-cudnn9-runtime python -c "import torch; print(torch.cuda.is_available())"
```

## Use as base image

```dockerfile
FROM halay08/pytorch:2.10.0-cuda12.8-cudnn9-runtime
WORKDIR /app
# Install your dependencies and copy app code
```

## Requirements

- Docker with NVIDIA Container Toolkit (for GPU support)
- NVIDIA driver compatible with CUDA 12.8

## License

See [PyTorch license](https://github.com/pytorch/pytorch/blob/main/LICENSE).
