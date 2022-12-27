# Certbot

## Generate Certificate via Cloudflare

```bash
certbot certonly -d example.com -d *.example.com --non-interactive --dns-cloudflare --dns-cloudflare-credentials /etc/letsencrypt/cloudflare.ini
```