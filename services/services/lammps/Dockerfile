# lammps: Large-scale Atomic/Molecular Massively Parallel Simulator
# Uses unofficial pip wheels (njzjz/lammps-wheel) for ARM64/x86_64 support
# Size target: ~600MB
FROM python:3.11-slim

LABEL org.opencontainers.image.source="https://github.com/sciagent-ai/scienv"
LABEL org.opencontainers.image.description="LAMMPS molecular dynamics simulation environment"
LABEL org.opencontainers.image.licenses="GPL-2.0"

ENV PYTHONUNBUFFERED=1

# Install OpenMPI runtime (required by LAMMPS pip wheel)
RUN apt-get update && apt-get install -y --no-install-recommends \
    libopenmpi3 \
    && rm -rf /var/lib/apt/lists/*

# Install LAMMPS and common dependencies via pip
RUN pip install --no-cache-dir \
    lammps \
    numpy \
    scipy \
    matplotlib

# Create workspace
WORKDIR /workspace

# Verify installation
RUN python -c "import lammps; lmp = lammps.lammps(); print(f'LAMMPS version: {lmp.version()}'); lmp.close()"

CMD ["python3"]
