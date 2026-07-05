# Public Web Server Behind CGNAT

## Overview

Built and deployed a publicly accessible website hosted on an Ubuntu Server VM using Nginx and Cloudflare Tunnel.

The project involved Linux administration, networking, DNS configuration, firewall management, service management, and troubleshooting residential ISP CGNAT restrictions.

Due to not being able to always having my server running, I have provided screenshots as proof of the functioning, running instance of the static site and services

## Technologies

- Ubuntu Server (VMware Fusion ISO)
- Nginx
- Cloudflare Tunnel
- Cloudflare DNS
- UFW
- systemd
- VMware Fusion
- SSH

## Problem

I initially attempted to publish the website through the method of traditional NAT port forwarding and implementation of my self configured Firewall (ufw) over ports 80 (nginx) and 22 (ssh). 
I would have then certified and provided an encrypted connection over port 443 between any clients and my server.

However, my ISP used Carrier Grade NAT (CGNAT), preventing incoming Internet request traffic from reaching my router.

## Investigation

I discovered:

Router WAN IP:
10.x.x.x

Public IP:
91.x.x.x

Since the WAN address listed on my router's interface was different from the public IP address of my router, I determined that the ISP was using CGNAT.

## Solution

To work around CGNAT:

- Purchased a custom domain through Cloudflare (tristanreid.dev)
- Created and connected to a named Cloudflare Tunnel
- Connected DNS records to the tunnel
- Configured cloudflared to forward requests to Nginx
- Installed cloudflared as a systemd service for automatic startup at server boot

## Architecture

Traditional Port Forwarding (Blocked by CGNAT):
                Internet
                    │
                    ▼
                91.x.x.x
                (Orange ISP)
                    │
                    ▼
                CGNAT
                    │
                    ✖
                    │
                Router
                    │
                    ▼
                Ubuntu VM
                    │
                    ▼
                Nginx

Cloudflare Deployment Tunnel:
                Visitor
                    │
                    ▼
                tristanreid.dev
                    │
                    ▼
                Cloudflare DNS
                    │
                    ▼
                Cloudflare Tunnel
                    │
                    ▼
                Ubuntu VM
                    │
                    ▼
                cloudflared
                    │
                    ▼
                localhost:80
                    │
                    ▼
                Nginx
                    │
                    ▼
                Portfolio Website

## Screenshots

<img width="1470" height="879" alt="website-live" src="https://github.com/user-attachments/assets/92652f2a-f06d-4039-bd8a-025007581678" />
(website runnning on the internet)

<img width="566" height="163" alt="image" src="https://github.com/user-attachments/assets/8cf78f72-ddf2-47bf-af77-7746949ef948" />
(cloudflared service running)

<img width="565" height="285" alt="image" src="https://github.com/user-attachments/assets/888824d3-16cf-4080-9305-f307434c791b" />
(nginx service running)

<img width="435" height="188" alt="image" src="https://github.com/user-attachments/assets/5ee935cf-ddb9-4425-9c3d-21837e5171bb" />
(configured firewall up and running)

<img width="739" height="100" alt="image" src="https://github.com/user-attachments/assets/219990e8-1a5b-4aee-acd5-17be666df64d" />
(tunnel up and in healthy status)

## Skills Demonstrated

- Linux Administration
- Networking
- DNS
- NAT and CGNAT
- Port Forwarding
- Nginx
- Cloudflare Tunnel
- systemd
- UFW Firewall
- SSH
- Virtualization
