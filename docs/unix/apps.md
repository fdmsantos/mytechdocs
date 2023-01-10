# Unix Apps

## Certbot

```bash
certbot certonly -d example.com -d *.example.com --non-interactive --dns-cloudflare --dns-cloudflare-credentials /etc/letsencrypt/cloudflare.ini
```

### Add new alternative name

```bash
certbot --expand certonly -d example.com -d *.example.com -d *.examplenew.com --non-interactive --dns-cloudflare --dns-cloudflare-credentials /etc/letsencrypt/cloudflare.ini
```

### Delete Certificate

```bash
# Get the certificate's name that will delete
sudo certbot certificates

sudo certbot delete --cert-name server.domain.tld
```

## Rsync

```bash
rsync -avu --delete /source/ -e "ssh -i temp-key.pem" --rsync-path="sudo rsync" <username>@<ip>:/dest
```

* -a Mantêm as propriedades do File System
* -v Verbose
* -u Apenas copia ficheiros com nova data de modificação ou então que size seja diferente (Sincronização incremental)
* --delete Apaga os ficheiros no destino se não existem na source
