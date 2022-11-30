# WireGuardVPN - Installation Instructions

## -> Create Digital Ocean Account

I created a digital ocean account and added my card information.
Then, I created a droplet and followed the intructions as stated in the slides deck provided by Prof. West.

## -> Install Docker

This process began by first going to the following link: https://thematrix.dev/setup-wireguard-vpn-server-with-docker/


First, I installed docker with the following code:

    sudo apt install apt-transport-https ca-certificates curl software-properties-common -y

    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

This next command makes a 32bit / 64bit OS
    
    sudo add-apt-repository \
    "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
    $(lsb_release -cs) \
    stable"

Once the above commands were executed, the terminal asked for me to click ENTER to proceed or exit to cancel the process. 

Then, to switch to the correct repo:

    apt-cache policy docker-ce

And to install Docker:

    sudo apt install docker-ce -y

Now, to install Docker-Compose

    sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

And to set permission:

    sudo chmod +x /usr/local/bin/docker-compose

## -> Setup Wireguard

To setup wireguard, I ran the following commands on the web terminal which caused the nano page to open.

    mkdir -p ~/wireguard/
    mkdir -p ~/wireguard/config/
    nano ~/wireguard/docker-compose.yml


Then, I pasted the following code in the nano page and saved it.

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

## -> Starting Wireguard

To start the wireguard, I intalled the WireGuard App on my phone.
Then, I copied the following command.

    cd ~/wireguard/
    docker-compose up -d

This above line causes the wireguard to start running.

The last line creates my own vpn. It outputs QR codes that I then scanned with my phone and which enabled me to access control over my own vpn on my phone. 

    docker-compose logs -f wireguard

THIS was SUCH a COOL project!