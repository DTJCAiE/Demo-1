# See here for a repo gallery of platform specific folder examples: 
# https://github.com/devcontainers

ARG VARIANT="3"
FROM mcr.microsoft.com/devcontainers/anaconda:${VARIANT}

# [Optional] If your pip requirements rarely change, uncomment this section to add them to the image.
COPY requirements.txt /tmp/pip-tmp/
RUN pip3 --disable-pip-version-check --no-cache-dir install -r /tmp/pip-tmp/requirements.txt \
    && rm -rf /tmp/pip-tmp

# Install Julia 1.6.7
ENV JULIA_VERSION=1.6.7
RUN apt-get update && \
    apt-get install -y wget curl git build-essential && \
    wget https://julialang-s3.julialang.org/bin/linux/x64/1.6/julia-${JULIA_VERSION}-linux-x86_64.tar.gz && \
    tar -xvzf julia-${JULIA_VERSION}-linux-x86_64.tar.gz && \
    mv julia-${JULIA_VERSION} /opt/julia && \
    ln -s /opt/julia/bin/julia /usr/local/bin/julia && \
    rm julia-${JULIA_VERSION}-linux-x86_64.tar.gz && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Set the working directory inside the container
WORKDIR /app

# Copy the current directory contents into the container
COPY . /app

# Install Julia dependencies
COPY .devcontainer/dependencies.jl /app/dependencies.jl
RUN julia -e 'include("dependencies.jl")'

# Install JupyterLab extensions for Julia integration
RUN julia -e 'using Pkg; Pkg.add("IJulia"); using IJulia; IJulia.installkernel("Julia")'

# Expose port 8888 for JupyterLab
EXPOSE 8888

# Set the default command to start JupyterLab
CMD ["jupyter", "lab", "--ip=0.0.0.0", "--port=8888", "--no-browser", "--allow-root"]

# [Optional] Uncomment this section to install additional OS packages.
# RUN apt-get update \
#     && export DEBIAN_FRONTEND=noninteractive \
#     && apt-get -y install --no-install-recommends <your-package-list-here>

# [Optional] Allow the vscode user to pip install globally w/o sudo
# ENV PIP_TARGET=/usr/local/pip-global
# ENV PYTHONPATH=${PIP_TARGET}:${PYTHONPATH}
# ENV PATH=${PIP_TARGET}/bin:${PATH}
# RUN mkdir -p ${PIP_TARGET} \
#     && chown vscode:root ${PIP_TARGET} \
#     && echo "if [ \"\$(stat -c '%U' ${PIP_TARGET})\" != \"vscode\" ]; then chown -R vscode:root ${PIP_TARGET}; fi" \
#         | tee -a /root/.bashrc /home/vscode/.bashrc /root/.zshrc >> /home/vscode/.zshrc 
