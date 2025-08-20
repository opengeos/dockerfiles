# ------------------------------
# Base image from Jupyter stack
# ------------------------------
FROM quay.io/jupyter/base-notebook:latest

# ------------------------------
# 1. Switch to root to install system packages
# ------------------------------
USER root

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    git \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

# ------------------------------
# 2. Install conda packages into base env
# ------------------------------
RUN mamba install -n base -c conda-forge -y \
    gdal \
    proj \
    geos \
    fiona \
    rasterio \
    pyproj \
    flask \
    flask-cors \
    leafmap \
    jupyter-server-proxy \
    quak \
    rioxarray \
    polars \
    voila \
    voila_topbar \
    && mamba clean --all --yes \
    && fix-permissions $CONDA_DIR

# ------------------------------
# 2b. Create missing sqlite symlinks
# ------------------------------
RUN ln -s $CONDA_PREFIX/lib/libsqlite3.so.3.50.0 $CONDA_PREFIX/lib/libsqlite3.so \
    && ln -s $CONDA_PREFIX/lib/libsqlite3.so.3.50.0 $CONDA_PREFIX/lib/libsqlite3.so.0

# ------------------------------
# 3. Set geospatial environment variables
# ------------------------------
ENV PROJ_LIB=$CONDA_DIR/share/proj \
    GDAL_DATA=$CONDA_DIR/share/gdal \
    LOCALTILESERVER_CLIENT_PREFIX='proxy/{port}'

# ------------------------------
# 4. Add jupyter_server_config.py to increase websocket limit
# ------------------------------
RUN mkdir -p /home/jovyan/.jupyter && \
    echo "c = get_config()  # noqa" > /home/jovyan/.jupyter/jupyter_server_config.py && \
    echo "c.ServerApp.tornado_settings = {\"websocket_max_message_size\": 100 * 1024 * 1024}" >> /home/jovyan/.jupyter/jupyter_server_config.py && \
    fix-permissions /home/jovyan/.jupyter

    # Copy only the leafmap Python package (no pip install)
COPY leafmap /opt/conda/lib/python3.13/site-packages/leafmap

RUN mkdir -p /home/jovyan/work && \
    fix-permissions /home/jovyan

# ------------------------------
# 5. Switch back to default user
# ------------------------------
USER $NB_UID
WORKDIR /home/jovyan/work

EXPOSE 8866

CMD ["jupyter", "server", "--ServerApp.default_url=/voila", "--port=8866", "--no-browser", "--ServerApp.root_dir=/home/jovyan/work"]


# ------------------------------
# Usage:
# docker build -t giswqs/leafmap:voila .
# docker buildx build --platform linux/amd64,linux/arm64 -t giswqs/leafmap:voila --push .
# docker pull giswqs/leafmap:voila
# docker run -it -p 8866:8866 -v $(pwd):/home/jovyan/work giswqs/leafmap:voila
# ------------------------------
