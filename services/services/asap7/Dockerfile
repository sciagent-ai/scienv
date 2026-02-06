# asap7: ASAP7 7nm FinFET Predictive Process Design Kit
# Extends OpenROAD with ASAP7 PDK, standard cells, and SRAM compiler
# Note: Uses amd64 base image, runs under emulation on ARM
FROM --platform=linux/amd64 openroad/orfs:latest

LABEL org.opencontainers.image.source="https://github.com/sciagent-ai/scienv"
LABEL org.opencontainers.image.description="ASAP7 7nm Predictive PDK with OpenROAD design flow"
LABEL org.opencontainers.image.licenses="BSD-3-Clause"

ENV PYTHONUNBUFFERED=1
ENV PATH="/OpenROAD-flow-scripts/tools/install/OpenROAD/bin:/OpenROAD-flow-scripts/tools/install/yosys/bin:${PATH}"

# Install git for cloning ASAP7 repository
RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    && rm -rf /var/lib/apt/lists/*

# Clone ASAP7 PDK with submodules (contains PDK, standard cells, SRAM)
WORKDIR /opt
RUN git clone --recursive --depth 1 https://github.com/The-OpenROAD-Project/asap7.git

# Set environment variables for ASAP7 PDK paths
ENV ASAP7_PDK_PATH="/opt/asap7/asap7_pdk_r1p7"
ENV ASAP7_SC7P5T_PATH="/opt/asap7/asap7sc7p5t_28"
ENV ASAP7_SC6T_PATH="/opt/asap7/asap7sc6t_26"
ENV ASAP7_SRAM_PATH="/opt/asap7/asap7_sram_0p0"

# Create workspace
WORKDIR /workspace

# Verify installation - check that PDK and standard cells are present
RUN ls -la /opt/asap7 && \
    openroad -version && \
    yosys -V

CMD ["openroad"]
