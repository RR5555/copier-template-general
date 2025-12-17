FROM nvidia/cuda:13.0.2-cudnn-runtime-ubuntu24.04

RUN apt update -y && apt upgrade -y && apt install -y curl git make && curl -LsSf https://astral.sh/uv/install.sh | sh && apt autoremove

ENV PATH="/root/.local/bin/:$PATH"

RUN uv python install 3.13

# WORKDIR /root
# RUN uv init ...


# WORKDIR /root/...
# RUN uv add ...