# 1. Base image
FROM apache/sedona:latest

# 2. Metadata (optional)
LABEL maintainer="giswqs@gmail.com"

# 3. Set working directory and switch to root for setup
USER root

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    bash \
    bash-completion \
    git \
    unzip \
    && apt-get clean && rm -rf /var/lib/apt/lists/* \
    && pip install pygis \
    && sed -i 's/"display_name": ".*"/"display_name": "Python 3"/' /usr/local/share/jupyter/kernels/python3/kernel.json


WORKDIR /app
RUN mkdir -p /app/workspace  && \
    mkdir -p /app/book
# 4. Copy files and set permissions
COPY book/ /app/book

WORKDIR /app

# ✅ Make entire /book directory owned by the notebook user

# 5. Switch to notebook user
# 6. Set default working directory (optional)

# 7. Command to run container
CMD ["jupyter", "lab", "--ip=0.0.0.0", "--port=8888", "--allow-root", "--no-browser"]

# 8. Mount workspace directory and run container
# docker run -it -p 8888:8888 -v $(pwd):/app/workspace giswqs/pygis:sedona

# 9. To build the docker image
# docker buildx build --platform linux/amd64,linux/arm64 -t giswqs/pygis:sedona --push .