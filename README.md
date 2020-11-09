# Traefik Local

My simple, local repo for Traefik local dev

## Setup
### 0. Create a new docker network for all your docker apps to share
```
docker network create traefik_net --gateway=172.26.0.1 --subnet=172.26.0.0/16
```

### 1. Enable dnsmasq in NetworkManager
- Edit the file /etc/NetworkManager/NetworkManager.conf, and add the line dns=dnsmasq to the [main] section, it will look like this :
```
[main]
plugins=ifupdown,keyfile
dns=dnsmasq

[ifupdown]
managed=false

[device]
wifi.scan-rand-mac-address=no
```

### 2. Let NetworkManager manage `/etc/resolv.conf`
```
sudo rm /etc/resolv.conf ; sudo ln -s /var/run/NetworkManager/resolv.conf /etc/resolv.conf
```

### 3. Configure *.docker
```
echo 'address=/docker/172.26.0.1' | sudo tee /etc/NetworkManager/dnsmasq.d/docker.conf
```

### 4. Reload NetworkManager
```
sudo systemctl reload NetworkManager
```

You should now be able to ping `test.docker`.

### 5. Copy the scripts to somewhere in your path

### 6. Start Traefik and a project:
```
# in this repo
docker-compose up

# in project repor
dev_setup docker-compose.override.yml web <host>

# To expost via local tunnel
expose
```

### Canvas
Canvas is a little different. The web services labels should be: 
```
- traefik.http.routers.canvas.entrypoints=web
- traefik.http.routers.canvas.rule=HostRegexp(`{subdomain:[a-z]+}.canvas.docker`,`canvas.docker`, `publiccanvas.docker`)
- lt.host=publiccanvas
```

This works for Shard one with AccountDomain host `canvas.docker` and second Shard AccountDomain host `second.canvas.docker`, etc.
It also works for a special, third shard: a public shard. This can be shared via local tunnel. The AccountDomain host for this shard should be `publiccanvas.loca.lt`