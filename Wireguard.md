# Wireguard on DigitalOcean 
For this project we will be installing wireguard through docker-compose using digital ocean

## Creating a DigitalOcean Droplet
1. To create a DO droplet foirst you need to head ot digitalocean.com and make an account

It is important you have a proper form of payment, I had quite an issue with my debit card so a credit card would be the best option

2. Once an account is made click on spijn up a droplet, choose a region where you (or your users) are. I used ubuntu OS, with a $42 a month plan and used a password isntead of the ssh key.

## installin Wireguard

1. Now log into the droplet using putty or something similar using the ip adress for you droplet

2. Install docker and docker compose

use this to install both

    sudo apt install docker.io
    sudo apt install docker-compose

3. Next is to get wireguard setup

Here is a link to follow for the wireguardf aspect
- https://thematrix.dev/setup-wireguard-vpn-server-with-docker/

Run this to get started

    mkdir -p ~/wireguard/
    mkdir -p ~/wireguard/config/
    nano ~/wireguard/docker-compose.yml  

Once that is set up copy this code but make a few adjustments. You can copy this here or from the WireGuard setup link

    version: '3.8'
    services:
    wireguard:
        container_name: wireguard
        image: linuxserver/wireguard
        environment:
        - PUID=1000
        - PGID=1000
        - TZ=Asia/Hong_Kong
        - SERVERURL=1.2.3.4
        - SERVERPORT=51820
        - PEERS=pc1,pc2,phone1
        - PEERDNS=auto
        - INTERNAL_SUBNET=10.0.0.0
        ports:
        - 51820:51820/udp
        volumes:
        - type: bind
            source: ./config/
            target: /config/
        - type: bind
            source: /lib/modules
            target: /lib/modules
        restart: always
        cap_add:
        - NET_ADMIN
        - SYS_MODULE
        sysctls:
        - net.ipv4.conf.all.src_valid_mark=1

- TZ is timezone I set mine to America/Goose_Bay because I liked the name, but use this link to find a timezone for you 
https://en.wikipedia.org/wiki/List_of_tz_database_time_zones?ref=thematrix.dev
- SERVERURL needs to be changed to the IP adress for digital ocean
- PEERS sets how many peers you want, i did not change anything

SAVE IT

    CTRL + X
    Y
    ENTER

4. now its time to start wireguard

Input this to start wireguard

    cd ~/wireguard/
    docker-compose up -d

Error kept popping up saying i am in the worng directory
5. Connect the phone to wireguard

You hould be able to scan into WireGuard on you phone through the QR code when you type in this

    docker-compose logs -f wireguard