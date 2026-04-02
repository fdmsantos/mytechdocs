# Podman

## userns

```sh
podman run -it --rm \
  --entrypoint /bin/bash \
  -v ./test:/test:Z \
  --userns=keep-id:uid=999,gid=999 \
  docker.io/networktocode/nautobot:2.4.20-py3.12
```

* keep-id = Adiciona o container user ao grupo do host e mantem os ficheiros com mesmo uid e gid