# linuxdays24-vpn

## Run Hello LinuxDays

On port 8000

```bash
docker run -d --name hello-linuxdays -p 8000:8000 sikalabs/hello-world-server:linuxdays
```

On port 80 (linux only)

```bash
docker run -d --name hello-linuxdays --net host -e PORT=80 sikalabs/hello-world-server:linuxdays
```

## Tailscale

### Install Tailscale Client

```bash
curl -fsSL https://tailscale.com/install.sh | sh
```

### Run Tailscale Client

```bash
tailscale up
```

## Headscale

### Install Headscale Server

https://headscale.net/setup/install/official/#using-packages-for-debianubuntu-recommended

```bash
HEADSCALE_VERSION="0.23.0"
HEADSCALE_ARCH="amd64"

wget --output-document=headscale.deb \
 "https://github.com/juanfont/headscale/releases/download/v${HEADSCALE_VERSION}/headscale_${HEADSCALE_VERSION}_linux_${HEADSCALE_ARCH}.deb"
```

Copy Headscale configuration file

```bash
scp headscale/config.yaml root@headscale.sikademo.com:/etc/headscale/config.yaml
```

```
systemctl enable headscale --now
```

Install Caddy proxy

```
apt install -y caddy
```

Copy Caddyfile

```bash
scp headscale/Caddyfile root@headscale.sikademo.com:/etc/caddy/Caddyfile
```

Restart Caddy

```
systemctl restart caddy
```

### Run Tailscale Client with Headscale server

```bash
tailscale up --login-server https://hsld.sl.zone
```
