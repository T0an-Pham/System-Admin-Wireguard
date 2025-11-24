### Digital Ocean Account
- use this link https://m.do.co/c/d33d59113ab6 to get 2 month of usage 
- add in payment(won't be charge unless forget to cancel)
- create droplet
- select the $6/month plan
- Choose authentication password and type in password of choice
- create droplet
### SSH into Droplet
- open powershell and `ssh root@(droplet id)`
- say yes to fingerprint and type in password
### Install Docker
- install docker using
	- `sudo apt update`
	- `sudo apt install ca-certificates curl gnupg`
	- `sudo install -m 0755 -d /etc/apt/keyrings`
	- `curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg`
	- `sudo chmod a+r /etc/apt/keyrings/docker.gpg`
- add repo
	- `echo \ "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \ $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null`
- install Docker and compose:
	- `sudo apt update`
	- `sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin`
- check to see if docker is working
	- run `sudo docker run hello-world`
### Install Wireguard
- project folder
	- `mkdir wireguard`
	- `cd wireguard`
- dowlond docker-compose.yml
	- `nano docker-compose.yml
	- inside docker-compose.yml:
		services:
		  wireguard:
		    image: linuxserver/wireguard
		    container_name: wireguard
		    cap_add:
		      - NET_ADMIN
		      - SYS_MODULE
		    environment:
		      - PUID=1000
		      - PGID=1000
		      - TZ=America/Chicago
		      - SERVERURL=DROPLET_IP
		      - SERVERPORT=51820
		      - PEERS=2
		      - PEERDNS=auto
		      - INTERNAL_SUBNET=10.13.13.0
		    volumes:
			-./config:/config
			-/lib/modules:/lib/modules
			ports:
			-51820:51820/udp
			sysctls:
			-net.ipv4.conf.all.src_valid_mark=1
			restart: unless-stopped
- start the VPN container with `sudo docker compose up -d`
- check with `sudo docker ps`
- get wireguard config files 
	- `cd wireguard/config`
	- `ls configs` to see files like peer1 and peer2
### Testing on Phone
- show local IP info with IPleak,net in a browser and screenshot it
- get qr code from peer 1
	- download qrencode to display it in terminal
	- display qr code with `qrencode -t ansiutf8 < peer1.conf`
- download wireguard -> add tunnel -> scan qr code
- turn on vpn and then check ip leak again
### Testing on Laptop
	same process as Iphone
- download wireguard
- screenshot ip before vpn
- screenshot ip after vpn



