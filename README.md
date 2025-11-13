
Docker containers runtime environment for Tercen operators.

# Runtime Specifications

| Runtime | Base Image | Size | R Version | Features | Use Case | Key Packages |
|---------|------------|------|-----------|----------|----------|--------------|
| **runtime-r44** | tercen/tidyverse:4.4.3 | 1.26GB | 4.4.3 | Complete R + tidyverse + Rust 1.85.0 | Complex analysis | renv, dplyr, tidyr, data.table, BiocManager |
| **runtime-r44-minimal** | r-minimal:4.4.3 (Alpine) | 87MB | 4.4.3 | Ultra-lightweight R | Simple operators | tercen, teRcenHttp, mtercen, teRcenApi |
| **runtime-r44-minimal-plot** | r-minimal:4.4.3 (Alpine) | 221MB | 4.4.3 | Minimal R + Cairo graphics | Plotting operators | tercen packages + data.table, dtplyr, Cairo |
| **runtime-r44-minimal-dev** | r-minimal:4.4.3 (Alpine) | ~150MB* | 4.4.3 | Minimal R + dev tools | Development | tercen packages + dev dependencies |
| **runtime-r40** | rocker/tidyverse:4.0.3 | ~2GB* | 4.0.3 | R 4.0 + Rust + Dart + Python | Legacy operators | tidyverse, rust 1.40.0, dart 2.10.5-1 |
| **runtime-r40-slim** | rocker/tidyverse:4.0.3 | ~1.5GB* | 4.0.3 | Slimmed R 4.0 | Legacy (optimized) | Essential R 4.0 packages |
| **runtime-r35** | rocker/tidyverse:3.5.3 | ~2GB* | 3.5.3 | R 3.5 + multi-language | Legacy operators | tidyverse, rust 1.40.0, dart 2.10.5-1 |
| **runtime-r35b** | rocker/tidyverse:3.5.3 | ~2GB* | 3.5.3 | R 3.5 variant | Legacy operators | tidyverse, rust 1.40.0, dart 2.10.5-1 |
| **runtime-python39** | debian:bullseye-slim | ~500MB* | - | Python 3.9 environment | Python operators | Python 3.9 + build tools |
| **runtime-tf** | tensorflow:2.17.0-gpu | ~3GB* | - | TensorFlow + GPU support | ML/AI operators | TensorFlow 2.17.0, CUDA, Python |
| **runtime-stats** | Custom | ~800MB* | Latest | Statistical computing | Advanced statistics | Statistical packages |
| **runtime-flowsuite** | Custom | ~1GB* | Latest | Flow cytometry analysis | Flow cytometry | Flow analysis packages |
| **dart** | Custom | ~200MB* | - | Dart development | Dart operators | Dart SDK + tools |
| **dind** | Docker | 2.65GB | - | Docker-in-Docker + GPU | CI/CD builds | Docker daemon, NVIDIA runtime |
| **matlab** | MATLAB Runtime | ~2GB* | - | MATLAB R2020b | MATLAB operators | MATLAB Runtime R2020b |

*Estimated sizes - actual sizes may vary based on build configuration

 
# Building and Deployment

## Automated Builds via GitHub Actions

Runtime images are automatically built and pushed using GitHub Actions when you create tags:

### R Runtimes
```bash
# Latest R 4.4 runtimes
git tag runtime-r44_4.4.3-12
git push --tags
git tag runtime-r44-minimal_4.4.3-2
git push --tags
git tag runtime-r44-minimal-dev_4.4.3-3
git push --tags
git tag runtime-r44-minimal-plot_4.4.3-2
git push --tags

# Legacy R runtimes
git tag runtime-r40_4.0.4-6
git tag runtime-r40-slim_4.0.4-1
git tag runtime-r35_3.5.3-8
git tag runtime-r35b_3.5.3-2
git push --tags
```

### Specialized Runtimes
```bash
# Python and ML runtimes
git tag runtime-python39_3.9-1
git tag runtime-tf_2.17.0-gpu-1
git tag runtime-stats_1.0-1
git tag runtime-flowsuite_1.0-1
git push --tags

# Development tools
git tag dart_3.5.3-1
git tag dind_latest-1
git tag runtime-matlab-image_r2020b-2
git push --tags
```

## Example Manual Build and Push Commands

```bash
# Build R 4.4 runtimes
IMAGE=tercen/runtime-r44:4.4.3-12
docker build --no-cache -t ${IMAGE} runtime-r44
docker push ${IMAGE}
```

## Testing Runtimes

### Performance Testing
```bash
# Test startup times
time docker run --rm --network=host tercen/runtime-r44-minimal:4.4.3-2 echo "hello"
time docker run --rm --network=host tercen/runtime-r44-minimal-plot:4.4.3-1 echo "hello"

# Test Tercen connectivity
docker run --rm --network=host tercen/runtime-r44-minimal:4.4.3-2 \
  R -e "teRcenHttp::GET('https://tercen.com')"
```

### Interactive Testing

```bash
# Interactive R session
docker run -it --rm tercen/runtime-r44-minimal:4.4.3-2 R

# Test plotting capabilities
docker run --rm tercen/runtime-r44-minimal-plot:4.4.3-1 \
  R -e "library(Cairo); CairoPNG('/tmp/test.png'); plot(1:10); dev.off(); cat('Plot created\n')"
```

