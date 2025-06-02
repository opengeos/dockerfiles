# Use a basic TeX Live image with full LaTeX distribution
FROM texlive/texlive:latest

# Update and install any additional tools (optional)
RUN apt-get update && apt-get install -y \
    make \
    git \
    python3 \
    python3-pip \
    nodejs \
    npm \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install the mystmd Python package
RUN pip3 install --break-system-packages mystmd

# Set working directory
WORKDIR /doc

# Copy your LaTeX project into the container (optional)
# COPY . /doc

# Default command (can be overridden)
CMD ["pdflatex", "main.tex"]


# # Build the image
# docker build -t latex-builder .

# # Run the container interactively with a local volume mount
# docker run -it --rm -v $(pwd):/doc latex-builder pdflatex main.tex
