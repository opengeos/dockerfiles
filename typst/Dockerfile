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
    mystmd \
    typst \
    && mamba clean --all --yes \
    && fix-permissions $CONDA_DIR

# 3. Set workspace directory
WORKDIR /home/jovyan/workspace

# 4. Fix permissions
RUN fix-permissions /home/jovyan

# 5. Remove old workspace
RUN rm -rf /home/jovyan/work

# 6. Switch back to default user
USER $NB_UID

# 7. Set working directory
WORKDIR /home/jovyan

# 8. Expose port
EXPOSE 8888

CMD ["jupyter", "lab", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root"]

# Launch Jupyter Lab
# docker run -it -p 8888:8888 -v $(pwd):/home/jovyan/workspace giswqs/typst:latest