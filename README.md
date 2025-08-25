# devops-sample-apps

## golang

Application in runtime needs p12 file(filename: file.p12) next to application binary.

go mod init doc-go # module name can be anything
go mod tidy # download dependencies, create go.sum file, if needed
docker build -t doc-go .    # build docker image

docker tag doc-go:latest 019496914213.dkr.ecr.eu-north-1.amazonaws.com/doc-go:1.0
docker push 019496914213.dkr.ecr.eu-north-1.amazonaws.com/doc-go:1.0

docker run -p 8080:80 doc-go:latest

## php

Application to run on production needs env `APP_ENV=prod` and file `config` next to index.php, 
repository contains `config.prod` and `config.dev`, for production purposes `config.prod` needs to be renamed to `config`. 

docker tag doc-php:latest 019496914213.dkr.ecr.eu-north-1.amazonaws.com/doc-php:1.0
docker push 019496914213.dkr.ecr.eu-north-1.amazonaws.com/doc-php:1.0


docker run -p 8080:80 -e APP_ENV=prod doc-php:latest

#test
kubectl kustomize deployment/go/prod/
#deploy
kubectl apply -k deployment/go/prod
kubectl apply -k deployment/php/prod