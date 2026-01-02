FROM nvidia/cuda:13.0.2-cudnn-runtime-ubuntu24.04

RUN apt update -y && apt upgrade -y \
	&& apt install -y curl git make \
	&& curl -LsSf https://astral.sh/uv/install.sh | UV_INSTALL_DIR="/usr/local/bin" sh \
	&& apt autoremove

ENV PATH="/usr/local/bin:$PATH"

RUN uv python install 3.13

RUN mkdir /project
WORKDIR /project

# RUN uv init ...
# RUN uv add ...
# RUN uv sync ...
