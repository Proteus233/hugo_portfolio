# [www.proteushomealb.dpdns.org](https://proteushomelab.dpdns.org)
# hugo_portfolio
Repo for my own personal portfolio thatt i selfhost on my homelab.


This is my personal website that i selfhost on my server.

# About the website
Personal portfolio and blog built with [Hugo](https://gohugo.io/) and the [Blowfish](https://blowfish.page/) theme. Covers my homelab server, personal projects and more to come.

# Hosting
The website is selfhosted on my homelab. The server is a repusposed second hand office.
## Server specs
> Model
> - HP elitedesk 800 SFF G3

> Operating System
> - Proxmox VE 9.1.1

> CPU - Intel i5-7500

> Memory
> - 16 GB
## Networking
To access the website there is a CLoudfare tunnel that fowards all trafic to Nginx Proxy Manager. From there it gets redirecterd to an isolated NGINX container hostting the static website. 

