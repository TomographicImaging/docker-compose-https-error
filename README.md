# docker networking issues

According to https://stfc.atlassian.net/wiki/spaces/CLOUDKB/pages/211877979/Fault+Fixes#Connectivity-https-services-seem-broken-from-inside-a-Docker-Container, docker https networking issues should be fixed by:

```sh
sudo iptables -I FORWARD -p tcp --tcp-flags SYN,RST SYN -j TCPMSS --clamp-mss-to-pmtu
```

However it only fixes `docker`, not `docker compose`.

## works

```sh
docker run --rm -it -h docker ubuntu
root@docker:/# apt update -qq
root@docker:/# apt install -yqq wget
root@docker:/# wget https://github.com
```

## fails


```yml
services:
  test:
    image: ubuntu
    command:
    - /bin/sh
    - -c
    - apt update -qq && apt install -yqq wget && wget -O- https://github.com
```

```sh
docker compose up
```
