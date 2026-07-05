# Public Web Server Behind CGNAT

## Overview

Built and deployed a publicly accessible website hosted on an Ubuntu Server VM using Nginx and Cloudflare Tunnel.

The project involved Linux administration, networking, DNS configuration, firewall management, service management, and troubleshooting residential ISP CGNAT restrictions.

Due to not being able to always having my server running, I have provided screenshots as proof of the functioning, running instance of the static site and services

The website 

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

(The website design and portions of the frontend implementation were developed with the assistance of AI tools. All infrastructure setup, networking configuration, server administration, DNS configuration, Cloudflare Tunnel deployment, troubleshooting, and system integration were implemented and managed by me.)
