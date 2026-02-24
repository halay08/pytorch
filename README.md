# halay08/pytorch

PyTorch Docker image with CUDA 12.8 and cuDNN 9 (runtime). Suitable for GPU training and inference.

## Image

- **Repository:** `halay08/pytorch`
- **Tag:** `2.10.0-cuda12.8-cudnn9-runtime`
- **Base:** Built from [pytorch/pytorch](https://hub.docker.com/r/pytorch/pytorch) official image
- **Size:** ~4.2 GB

## Quick start

```bash
docker pull halay08/pytorch:2.10.0-cuda12.8-cudnn9-runtime
docker run --gpus all -it halay08/pytorch:2.10.0-cuda12.8-cudnn9-runtime python -c "import torch; print(torch.cuda.is_available())"
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
