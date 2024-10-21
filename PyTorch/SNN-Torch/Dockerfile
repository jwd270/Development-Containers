FROM pytorch/pytorch:2.3.0-cuda12.1-cudnn8-runtime

ARG USERNAME=vscode
ARG USER_UID=1000
ARG USER_GID=$USER_UID

RUN apt update && apt install -y build-essential git ffmpeg x11-utils libgstreamer-plugins-base1.0-dev python-gi-dev libgirepository1.0-dev libzmq3-dev gcc-9 g++-9
RUN pip install --upgrade pip && pip install scipy jupyterlab snntorch fpbinary plotly onnx onnxscript

RUN groupadd --gid $USER_GID $USERNAME \
    && useradd --uid $USER_UID --gid $USER_GID -m $USERNAME \
    && apt-get update \
    && apt-get install -y sudo \
    && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
    && chmod 0440 /etc/sudoers.d/$USERNAME

USER $USERNAME
WORKDIR /workspaces