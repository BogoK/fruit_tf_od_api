FROM tensorflow/tensorflow:2.2.0-gpu

ARG DEBIAN_FRONTEND=noninteractive

# Install apt dependencies
RUN apt-get update && apt-get install -y \
    git \
    gpg-agent \
    python3-cairocffi \
    protobuf-compiler \
    python3-pil \
    python3-lxml \
    python3-tk \
    wget


# Add new user to avoid running as root
RUN useradd -ms /bin/bash tensorflow
USER tensorflow
WORKDIR /home/tensorflow/

# Copy this version of of the model garden into the image
COPY --chown=tensorflow . /home/tensorflow/fruit_tf_od_api

# Compile protobuf configs
RUN (cd /home/tensorflow/fruit_tf_od_api/ && protoc object_detection/protos/*.proto --python_out=.)
WORKDIR /home/tensorflow/fruit_tf_od_api/

RUN mkdir -p models/pretrained_models
RUN mkdir -p models/exported_models

RUN mkdir -p data/tfrecords

RUN cp object_detection/packages/tf2/setup.py ./
ENV PATH="/home/tensorflow/.local/bin:${PATH}"

RUN python -m pip install -U pip
RUN python -m pip install .

ENV TF_CPP_MIN_LOG_LEVEL 3

