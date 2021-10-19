
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