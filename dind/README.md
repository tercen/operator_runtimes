https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/supported-platforms.html

```shell
sudo apt install nvidia-driver-535 nvidia-utils-535 -y

nvidia-smi

```

install nvidia-container-toolkit

```shell
curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit-keyring.gpg \
  && curl -s -L https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list | \
    sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit-keyring.gpg] https://#g' | \
    sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
    
sudo apt-get update
sudo apt-get install -y nvidia-container-toolkit

# Configure the container runtime by using the nvidia-ctk command:
sudo nvidia-ctk runtime configure --runtime=docker
sudo systemctl restart docker

nvidia-container-cli --version
```


```shell
IMAGE=tercen/nvidia-dind:12.1.0-runtime-ubuntu22.04
docker build -t ${IMAGE} .
docker push ${IMAGE}

# start docker runtime dind
docker run --gpus all -it --privileged \
      -v ${PWD}/var/run:/var/run \
      -v ${PWD}/var/lib/docker:/var/lib/docker \
      ${IMAGE}
      
# start docker client
docker run -it \
      -v ${PWD}/var/run:/var/run \
      docker:28.0.1-cli-alpine3.21
      
docker run -it --rm --gpus all tensorflow/tensorflow:2.17.0-gpu \
   python -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))" 
   
docker run --gpus all -it --rm pytorch/pytorch:2.6.0-cuda12.4-cudnn9-runtime \
   python -c "import torch; print(torch.__version__) ; print(torch.cuda.is_available())" 
      
# Run a sample CUDA container
docker run --rm --runtime=nvidia --gpus all ubuntu nvidia-smi

docker run -it --rm --runtime=nvidia --gpus all debian bash
import tensorflow as tf
tf.test.is_gpu_available()

docker run -it --rm --runtime=nvidia --gpus all tensorflow/tensorflow \
   python -c "import tensorflow as tf; print(tf.reduce_sum(tf.random.normal([1000, 1000])))"
   
docker run -it --rm --runtime=nvidia --gpus all tensorflow/tensorflow \
   python -c "import tensorflow as tf; print(tf.test.is_gpu_available())"

docker run -it --rm --runtime=nvidia --gpus all tensorflow/tensorflow \
   python -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))"    
   
docker run -it --rm --gpus all tensorflow/tensorflow:2.14.0-gpu bash 

docker run -it --rm --gpus all tercen/runtime-python39:0.1.0 bash
PYTHONPATH="~/.pyenv/versions/3.9.0/bin/python3"
#python3 -m pip install tensorflow==2.17.0
python3 -m pip install tensorflow==2.14.0
python3 -m pip install numpy==1.23.5

python3 -c "import tensorflow as tf; print(tf.config.list_physical_devices('GPU'))"    

nvidia-smi -l 1
```


```shell

docker network create tercen-network
docker volume create dind-docker-certs-ca
docker volume create dind-docker-certs-client

docker run --privileged --name tercen-runtime -d \
    --network tercen-network --network-alias docker \
    -e DOCKER_TLS_CERTDIR=/certs \
    -v dind-docker-certs-ca:/certs/ca \
    -v dind-docker-certs-client:/certs/client \
    -p 23760:2376 \
    tercen/dind
    
docker run --rm --network tercen-network \
    -e DOCKER_TLS_CERTDIR=/certs \
    -v dind-docker-certs-client:/certs/client:ro \
    docker:latest version
    
    
alias dockerx="docker \
  --tlsverify \
  -H=127.0.0.1:23760 \
  --tlscacert=/var/lib/docker/volumes/dind-docker-certs-client/_data/ca.pem \
  --tlscert=/var/lib/docker/volumes/dind-docker-certs-client/_data/cert.pem \
  --tlskey=/var/lib/docker/volumes/dind-docker-certs-client/_data/key.pem"
```