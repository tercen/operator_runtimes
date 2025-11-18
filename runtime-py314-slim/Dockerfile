# Multi-stage build to minimize final image size

# Build stage
FROM python:3.14-slim AS builder

# Install system build dependencies
RUN apt-get update && apt-get install -y --no-install-recommends \
    git \
    build-essential \
    pkg-config \
    && rm -rf /var/lib/apt/lists/*

# Install build tools
RUN pip install --no-cache-dir --upgrade pip 'setuptools>=70.0.0' wheel

# Install Python packages
RUN pip install --no-cache-dir --no-build-isolation 'numpy>=1.26.0'
RUN pip install --no-cache-dir --no-build-isolation git+https://github.com/tercen/pytson@1.8.10
RUN pip install --no-cache-dir --no-build-isolation --no-deps git+https://github.com/tercen/tercen_python_client.git@00cebf60e2c7b3d9ef83f84f4848bb64115aebc5

# Runtime stage
FROM python:3.14-slim

# Copy installed Python packages from builder stage
COPY --from=builder /usr/local/lib/python3.14/site-packages /usr/local/lib/python3.14/site-packages
COPY --from=builder /usr/local/bin /usr/local/bin

# Set working directory
WORKDIR /app