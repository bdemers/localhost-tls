# localhost TLS

Access services on `localhost:8080` via `https://local.example` using `mkcert` and Nginx.

Add `local.example` to hosts file:

```sh
sudo sh -c 'echo "127.0.0.1 local.example" >> /etc/hosts'
```

Create directories:

```sh
mkdir devcerts
mkdir logs
```

Set up certificates with `mkcert`

```sh
# Install mkcert if needed
brew install mkcert

mkcert --install
mkcert -key-file devcerts/key.pem -cert-file devcerts/cert.pem local.example
```

Start up Nginx

```sh
docker compose up
```

Start your application on port 8080

Access your service from `https://local.example`

> **NOTE:** HTTPie does NOT use the system certificates, you must point to the mkcert's directly

```sh
http --verify="$(mkcert --CAROOT)/rootCA.pem" https://local.example
```
Or set up an alias:

```sh
alias http="http --verify=\"$(mkcert --CAROOT)/rootCA.pem\""
```
