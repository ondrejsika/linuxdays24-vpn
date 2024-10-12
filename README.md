# linuxdays24-vpn

## Run Hello LinuxDays

Locally

```bash
COLOR='#FAAF3A' BACKGROUND_COLOR='#EEEEEE' TEXT='Hello LinuxDays!' hello-world-server
```

On port 8000

```bash
docker run -d --name hello-linuxdays -p 8000:8000 sikalabs/hello-world-server:linuxdays
```

On port 80 (linux only)

```bash
docker run -d --name hello-linuxdays --net host -e PORT=80 sikalabs/hello-world-server:linuxdays
```

## Cloudflare Access

https://linuxdays-cloudflare-access.sikademo.com/

## WireGuard

### Generate config

https://www.wireguardconfig.com/

### Install WireGuard

```bash
apt install -y wireguard
```

### Server

```bash
scp wireguard/server.conf root@wg.sikademo.com:/etc/wireguard/wg0.conf
```

Start WireGuard

```
systemctl enable wg-quick@wg0 --now
```

Restart WireGuard

```
systemctl restart wg-quick@wg0
```

### Client

```bash
scp wireguard/client1.conf root@vm1.sikademo.com:/etc/wireguard/wg0.conf
```

Start WireGuard

```
systemctl enable wg-quick@wg0 --now
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
