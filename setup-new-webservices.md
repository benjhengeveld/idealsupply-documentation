# New Webservices Setup

#### Update WSL

```bash
wsl --update
```


#### Install Ubuntu-22.04 WSL distro

```bash
wsl --install Ubuntu-22.04
```


#### Set Ubuntu-22.04 WSL version to 2

```bash
wsl --set-version Ubuntu-22.04 2
```


#### Set default WSL distro to Ubuntu-22.04

```bash
wsl --set-default Ubuntu-22.04
```


#### Start WSL in the users home directory

```bash
wsl ~
```


#### Install Docker

```bash
curl -s https://raw.githubusercontent.com/karaage0703/ubuntu-setup/master/install-docker.sh | bash
```



#### Install NVM / NodeJs

```bash
curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash
source ~/.bashrc
nvm install 18
```



#### Install dcm

```bash
npm i -g @cpdevtools/dcman-cli && dcm install
```



#### Add/create profile source

```bash
dcm profile-sources create <GITHUB USERNAME>/dc-profiles
or
dcm profile-sources add <GITHUB USERNAME>/dc-profiles
```



#### Create profile

```bash
dcm profiles create <GITHUB USERNAME>/dc-profiles idealsupply
```



#### Set profile

```bash
dcm profile set <GITHUB USERNAME>/dc-profiles#idealsupply
```



#### Open workspace

```bash
dcm open IdealSupply/dcm-dc-dotnet <WORKSPACE>
```

## Workspaces
- corporate-operations
- dashboard
- ideal-vmi
- ideallink
- tradeshow
