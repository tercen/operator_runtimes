# Build stage
FROM python:3.14-alpine AS builder

# Install system build dependencies
RUN apk add --no-cache \
    git \
    build-base \
    pkgconfig \
    && rm -rf /var/cache/apk/*

# Install build tools
RUN pip install --no-cache-dir --upgrade pip 'setuptools>=70.0.0' wheel

# Install Python packages
RUN pip install --no-cache-dir --no-build-isolation 'numpy>=1.26.0'
RUN pip install --no-cache-dir --no-build-isolation git+https://github.com/tercen/pytson@1.8.10
RUN pip install --no-cache-dir --no-build-isolation --no-deps git+https://github.com/tercen/tercen_python_client.git@00cebf60e2c7b3d9ef83f84f4848bb64115aebc5

# Runtime stage
FROM python:3.14-alpine

# Copy installed Python packages and binaries from builder stage
COPY --from=builder /usr/local/lib/python3.14/site-packages /usr/local/lib/python3.14/site-packages
COPY --from=builder /usr/local/bin/ /usr/local/bin/

# Ensure python symlink exists
RUN ln -sf python3 /usr/local/bin/python

# Remove unnecessary files to minimize size (but keep python symlinks)
RUN find /usr/local/lib/python3.14/site-packages -name "*.pyc" -delete && \
    find /usr/local/lib/python3.14/site-packages -name "__pycache__" -type d -exec rm -rf {} + && \
    find /usr/local/lib/python3.14/site-packages -name "*.dist-info" -type d -exec rm -rf {} + && \
    find /usr/local/lib/python3.14/site-packages -name "test*" -type d -exec rm -rf {} + && \
    find /usr/local/lib/python3.14/site-packages -name "tests" -type d -exec rm -rf {} +
