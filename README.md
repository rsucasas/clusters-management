# clusters-management

## Environment setup

### Ubuntu 20

```Vagrantfile
Vagrant.configure("2") do |config|
  config.vm.box = "bento/ubuntu-20.04"
  config.vm.network "public_network", ip: "192.168.1.152"
end
```

#### Golang

https://tecadmin.net/how-to-install-go-on-ubuntu-20-04/

```
sudo apt-get update  
sudo apt-get -y upgrade  

wget  https://go.dev/dl/go1.19.linux-amd64.tar.gz 

sudo tar -xvf go1.19.linux-amd64.tar.gz   
sudo mv go /usr/local 

export GOROOT=/usr/local/go 
export GOPATH=$HOME/Projects/Proj1 
export PATH=$GOPATH/bin:$GOROOT/bin:$PATH 

source ~/.profile

go version
```

----------------------------

## Kind

### Requirements



----------------------------

## OCM



----------------------------

## Openwhisk
