# Configuring Traefik on a Mac
This guide describes configuring Traefik on a Mac using Ubuntu Multipass.

These instructions do not cover setting up Multipass. For instructions on Multipass, see [this article](https://ubuntu.com/blog/replacing-docker-desktop-on-windows-and-mac-with-multipass).

After completing this guide, requests to *.docker on your Mac will look like this:

REQUEST ON MAC to *.docker --> dnsmasq (on Mac) --> Virtual Machine --> Traefik Routing --> Docker Container

Container-to-container communication is routed through dnsmasq and Traefik routing on the VM.

## Configuring Traefik on the VM
1. Create a VM with Multipass and SSH into the VM
2. Follow the instructions [here](https://github.com/westonkd/traefik) to configure Traefik on the VM

After completing these instructions, Traefik will be able to resolve host names that map to running Docker containers and proxy the request to those containers.

Next we need to point *.docker requests made on our Mac to the VM so Traefik can route to the proper containers.

## Configuring dnsmasq on the host (Mac)
1. Install dnsmaq on your mac by following the instructions [here](https://gist.github.com/brablc/f48fef6336765212360ed3de66034b90)
2. Discover the IP address of your VM using `multipass list`
3. Follow the instructions [here](https://gist.github.com/brablc/f48fef6336765212360ed3de66034b90) to point *.docker to the IP address of your VM. Note that these instructions are configuring *.test, so be sure to substitute in *.docker as you follow along.

