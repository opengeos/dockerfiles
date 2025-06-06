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
    "gdal=3.11.0" \
    proj \
    geos \
    pyproj \
    && mamba clean --all --yes \
    && fix-permissions $CONDA_DIR

RUN mamba install -c conda-forge --force-reinstall gdal

# ------------------------------
# 2b. Create missing sqlite symlinks (after files exist)
# ------------------------------
RUN ln -s $CONDA_PREFIX/lib/libsqlite3.so.3.50.0 $CONDA_PREFIX/lib/libsqlite3.so \
    && ln -s $CONDA_PREFIX/lib/libsqlite3.so.3.50.0 $CONDA_PREFIX/lib/libsqlite3.so.0


# ------------------------------
# 3. Set geospatial environment variables
# ------------------------------
ENV PROJ_LIB=$CONDA_DIR/share/proj \
    GDAL_DATA=$CONDA_DIR/share/gdal
    

# ------------------------------
# 4. Fix permissions
# ------------------------------
    RUN fix-permissions /home/jovyan

# ------------------------------
# 5. Switch back to default user
# ------------------------------
USER $NB_UID
WORKDIR /home/jovyan

# 6. Expose port
EXPOSE 8888

CMD ["jupyter", "lab", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root"]

# ------------------------------
# Usage:
# docker pull giswqs/gdal:v3.11.0
# docker run -it -p 8888:8888 -v $(pwd):/home/jovyan/work giswqs/gdal:v3.11.0
# ------------------------------