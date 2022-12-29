# Unix Apps

## Certbot

```bash
certbot certonly -d example.com -d *.example.com --non-interactive --dns-cloudflare --dns-cloudflare-credentials /etc/letsencrypt/cloudflare.ini
```

## Rsync

```bash
rsync -avu --delete /source/ -e "ssh -i temp-key.pem" --rsync-path="sudo rsync" <username>@<ip>:/dest
```

* -a Mantêm as propriedades do File System
* -v Verbose
* -u Apenas copia ficheiros com nova data de modificação ou então que size seja diferente (Sincronização incremental)
* --delete Apaga os ficheiros no destino se não existem na source
