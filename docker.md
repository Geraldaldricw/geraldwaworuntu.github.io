1. Introduction

For this project, I installed Docker on my Ubuntu machine. I made a few mistakes at first and had to fix them, so I decided to write everything down exactly how it happened. I did not have a perfect install on the first try. This is the actual way I installed Docker and got Uptime Kuma running.

2. Updating My System

I started by updating my packages:

sudo apt update

3. First Attempt at Installing Docker (Wrong Way)

At first, I thought Docker was installed by doing this:

sudo apt install docker.io docker-compose


This installed:

docker.io

old docker-compose

python3 docker packages

Docker itself worked, but the new docker compose command didn’t exist.
When I typed:

docker compose version


It said:

docker: unknown command: docker compose


So I realized this method was not good for this project.

4. Removing the Old Docker Installation

To fix the problem, I removed everything:

sudo apt remove $(dpkg --get-selections docker.io docker-compose docker-compose-v2 docker-doc podman-docker containerd runc | cut -f1)


This removed:

docker.io

docker-compose

old containerd

old runc

I wanted to start fresh.

5. Installing Docker the Correct Way (From Docker Website)

After I cleaned everything, I used the official Docker documentation:

https://docs.docker.com/engine/install/ubuntu/

5.1 Install required tools
sudo apt update
sudo apt install ca-certificates curl

5.2 Add Docker’s GPG key
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

5.3 Add Docker’s repository
sudo tee /etc/apt/sources.list.d/docker.sources <<EOF
Types: deb
URIs: https://download.docker.com/linux/ubuntu
Suites: $(. /etc/os-release && echo "${UBUNTU_CODENAME:-$VERSION_CODENAME}")
Components: stable
Signed-By: /etc/apt/keyrings/docker.asc
EOF

5.4 Update again
sudo apt update

5.5 Install Docker Engine + Compose plugin
sudo apt install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin


NOW the correct docker compose command is available.

6. Enabling Docker

At first I accidentally typed:

sudo systemct1


with a number “1”.

The correct command is:

sudo systemctl enable --now docker


This started Docker and made it auto-start in the future.

7. Testing Docker

I tested Docker:

docker --version
docker compose version


Then I ran the test container:

sudo docker run hello-world


This confirmed that Docker was fully installed.

8. Setting Up Uptime Kuma

I created a folder:

mkdir uptime-kuma
cd uptime-kuma


Then I opened the compose file:

nano docker-compose.yml


And I pasted this:

version: "3.8"
services:
  uptime-kuma:
    image: louislam/uptime-kuma:latest
    container_name: uptime-kuma
    ports:
      - "3001:3001"
    volumes:
      - ./data:/app/data
    restart: always

9. Running Uptime Kuma

I used the new compose command:

docker compose up -d


Then I checked the containers:

docker ps


Uptime Kuma was running.

10. Accessing the Dashboard

I got my IP:

hostname -I


Then I opened:

http://<my-ip>:3001


The Uptime Kuma setup page worked.
I created an account and added a monitor.

11. Summary

Here is what I actually did:

Installed Docker the wrong way

Removed all the old Docker packages

Followed the official Docker website

Added the Docker repo and GPG key

Installed Docker Engine and Compose plugin correctly

Enabled Docker

Tested Docker

Used docker compose to run Uptime Kuma

Accessed it on port 3001

This is the complete process I followed for this project.
