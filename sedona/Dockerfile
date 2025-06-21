# 1. Base image
FROM apache/sedona:latest

# 2. Metadata
LABEL maintainer="giswqs@gmail.com"

# 3. Set working directory and switch to root for setup
USER root

# 4. Install required packages and Python libraries
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    bash \
    bash-completion \
    git \
    unzip \
    && apt-get clean && rm -rf /var/lib/apt/lists/* \
    && pip install pygis \
    && sed -i 's/"display_name": ".*"/"display_name": "Python 3"/' /usr/local/share/jupyter/kernels/python3/kernel.json

# 5. Create working directories and copy content
WORKDIR /app
RUN mkdir -p /app/workspace && \
    mkdir -p /app/book
COPY book/ /app/book

# 6. Mount workspace directory and run container
# docker run -it -p 8888:8888 -p 4040:4040 -p 8080:8080 -p 8081:8081 -p 7077:7077 -p 8085:8085 -v $(pwd):/app/workspace giswqs/pygis:sedona

# 7. To build the docker image
# docker buildx build --platform linux/amd64,linux/arm64 -t giswqs/pygis:sedona --push .