# PyTorch Docker + YOLO

PyTorch Docker image with CUDA 12.6 and cuDNN 9 (runtime). Suitable for GPU training and inference.

## Images

| Tag                             | Description                                                                                                          |
| ------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
| `2.9.1-cuda12.6-cudnn9-runtime` | PyTorch + CUDA + system libs (OpenCV). Generic base.                                                                 |
| `2.9.1-cuda12.6-cudnn9-yolo26m` | Same as above + YOLO weights (yolo26m.pt, yolo26n.pt) in `/app/weights` for Ultralytics offline (e.g. ECS training). |

- **Repository:** `movahub/pytorch`
- **Base:** Built from [pytorch/pytorch](https://hub.docker.com/r/pytorch/pytorch) official image

## Build (base images)

```bash
cd pytorch-docker
# 1. Generic GPU base (no YOLO) — phải build + push trước vì gpu.yolo26m kế thừa từ đây
docker build --platform=linux/amd64 -f Dockerfile.gpu -t movahub/pytorch:2.9.1-cuda12.6-cudnn9-runtime .
docker push movahub/pytorch:2.9.1-cuda12.6-cudnn9-runtime
# 2. GPU + YOLO weights (for sphere-ai training) — extends từ image trên
docker build --platform=linux/amd64 -f Dockerfile.gpu.yolo26m -t movahub/pytorch:2.9.1-cuda12.6-cudnn9-yolo26m .
docker push movahub/pytorch:2.9.1-cuda12.6-cudnn9-yolo26m
```

## Quick start

```bash
docker pull movahub/pytorch:2.9.1-cuda12.6-cudnn9-runtime
docker run --gpus all -it movahub/pytorch:2.9.1-cuda12.6-cudnn9-runtime python -c "import torch; print(torch.cuda.is_available())"
```

## Use as base image

```dockerfile
FROM movahub/pytorch:2.9.1-cuda12.6-cudnn9-runtime
WORKDIR /app
# Install your dependencies and copy app code
```

## Requirements

- Docker with NVIDIA Container Toolkit (for GPU support)
- NVIDIA driver compatible with CUDA 12.6

## License

See [PyTorch license](https://github.com/pytorch/pytorch/blob/main/LICENSE).
