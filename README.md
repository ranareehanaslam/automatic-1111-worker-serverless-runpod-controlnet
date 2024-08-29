# Serverless Automatic 1111 on RunPod

This repository contains a serverless setup for running Automatic 1111 on RunPod. The setup leverages Docker and NVIDIA GPU support for efficient inference using the Stable Diffusion model.

## Features

- **Serverless Inference:** Efficiently handle inference requests using the serverless handler.
- **API Integration:** Easily interact with the inference process via a dedicated API.
- **NVIDIA GPU Support:** Full GPU acceleration with NVIDIA CUDA for faster processing.

## Prerequisites

- **NVIDIA CUDA Toolkit:** Ensure that your environment has the NVIDIA CUDA Toolkit installed.
- **Docker with NVIDIA Runtime:** Docker must be configured with the NVIDIA runtime for GPU support.

### Installation Steps

1. **Update and Upgrade Your System:**

    ```bash
    sudo apt-get update -y
    sudo apt-get upgrade -y
    ```

2. **Install NVIDIA CUDA Toolkit:**

    ```bash
    sudo apt install -y nvidia-cuda-toolkit
    ```

3. **Verify CUDA Installation:**

    Run the following Docker command to verify that CUDA is working correctly:

    ```bash
    sudo docker run --rm --gpus all nvidia/cuda:11.6.2-base-ubuntu20.04 nvidia-smi
    ```

4. **Configure Docker with NVIDIA Runtime:**

    Edit the Docker daemon configuration:

    ```bash
    sudo nano /etc/docker/daemon.json
    ```

    Add the following configuration:

    ```json
    {
        "runtimes": {
            "nvidia": {
                "path": "/usr/bin/nvidia-container-runtime",
                "runtimeArgs": []
            }
        },
        "default-runtime": "nvidia"
    }
    ```

    Save and exit the file, then restart Docker.

5. **Build the Docker Image:**

    Use the following command to build your Docker image:

    ```bash
    DOCKER_BUILDKIT=0 docker build -t nvidia-test -f Dockerfile .
    ```

## API

The worker provides an API for inference, set up using `supervisor`. The API is configured to run on port 3000, allowing external requests to perform inference.

## Serverless Handler

The serverless handler (`rp_handler.py`) is a Python script responsible for managing inference requests. It defines the `handler(event)` function, which processes incoming requests, runs the inference using the Stable Diffusion model, and returns the output.

## Usage

To start the serverless instance and begin handling inference requests, follow the instructions provided in the guide above.

## Contributing

Contributions are welcome! Please feel free to submit issues, fork the repository, and send pull requests.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
