# KANAAL — Chat Server

Self-hosted WebSocket chat server for the [Kanaal](https://kanaal.chat) frontend.

## Quick start

```bash
# 1. Clone
git clone https://github.com/kanaal-chat/server
cd server

# 2. Install dependencies
npm install

# 3. Start
npm start
```

Connect with `ws://localhost:3001` from the frontend.

## Requirements

- Node.js ≥ 18
- npm

## Environment variables

| Variable | Default | Description |
|----------|---------|-------------|
| `PORT`   | `3001`  | Port the server listens on |

```bash
PORT=8080 npm start
```

## Run as a background service (pm2)

```bash
npm install -g pm2
pm2 start server.js --name kanaal
pm2 save
pm2 startup
```

## HTTPS / WSS

For a secure connection (`wss://`) you need a reverse proxy.

### Nginx example

```nginx
server {
    listen 443 ssl;
    server_name yourdomain.com;

    ssl_certificate /etc/letsencrypt/live/yourdomain.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/yourdomain.com/privkey.pem;

    location / {
        proxy_pass http://localhost:3001;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "Upgrade";
        proxy_set_header Host $host;
    }
}
```

Get a free SSL certificate via [Certbot](https://certbot.eff.org).

### Easier options

- **[Railway](https://railway.app)** — Free tier, automatic SSL, deploy in 2 clicks
- **[Render](https://render.com)** — Free tier, automatic SSL
- **[Fly.io](https://fly.io)** — Free tier, automatic SSL

## Protocol

The server communicates via JSON WebSocket messages.

### Client → Server

| Type | Fields | Description |
|------|--------|-------------|
| `join` | `name: string` | Join with a display name |
| `message` | `text: string` | Send a message |
| `typing` | — | Typing indicator |

### Server → Client

| Type | Fields | Description |
|------|--------|-------------|
| `welcome` | `name, color, users` | Connection confirmed |
| `message` | `name, color, text, time` | Message from a user |
| `system` | `text` | System notification |
| `users` | `users[]` | Updated user list |
| `typing` | `name` | Someone is typing |

## License

MIT
