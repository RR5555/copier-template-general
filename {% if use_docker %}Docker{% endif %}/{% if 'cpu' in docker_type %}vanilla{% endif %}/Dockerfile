FROM ubuntu:24.04

RUN apt update -y && apt upgrade -y \
	&& apt install -y curl git make \
	&& curl -LsSf https://astral.sh/uv/install.sh | UV_INSTALL_DIR="/usr/local/bin" sh \
	&& apt autoremove

ENV PATH="/usr/local/bin:$PATH"

RUN uv python install 3.13

# WORKDIR /root
# RUN uv init ...


# WORKDIR /root/...
# RUN uv add ...