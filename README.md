# mac-silicon-ci-setup
Setup a Mac Silicon for Gitlab CI

## Install Apps

#### Brew

[brew.sh](https://brew.sh/)

`/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`

#### Oh My ZSH

[oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh)

`sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`

### GCloud SDK

[gCloud SDK](https://cloud.google.com/sdk/docs/install)

`gcloud auth configure-docker`

### Other Apps

* xCode
* [Docker](https://docs.docker.com/docker-for-mac/apple-m1/)
* [Go](https://golang.org/dl/)
* [Node](https://nodejs.org/en/)


## Ci Setup

### Gitlab Runner

[GitLab Runner on macOS](https://gitlab.com/gitlab-org/gitlab-runner/blob/master/docs/install/osx.md)

`brew install gitlab-runner`

### Register Runner

`gitlab-runner register`

* Enter Gitlab URL: `https://gitlab.com`
* Enter Registration Token: `xxxx`
* Enter Description: `MacSilicon`
* Enter Tags: `mac`
* Enter Executor: `shell`

### Run as service 

`brew services start gitlab-runner`

## Create Docker Images

### Go

`GOOS=linux GOARCH=amd64 go build -v`

### React

`CI=false npm run-script build`

### Docker Runner

`docker buildx build --platform linux/amd64 -t us.gcr.io/repo-name/go-service:alpha .`


`docker buildx build --platform linux/amd64 -t username/demo:latest --push .`

### Push To GCloud

```
. ~/google-cloud-sdk/path.bash.inc
echo $GCLOUD_SERVICE_KEY | base64 -d > $HOME/gcloud-service-key.json
gcloud auth activate-service-account --key-file $HOME/gcloud-service-key.json
gcloud config set project $BUILD_REGISTRY
```