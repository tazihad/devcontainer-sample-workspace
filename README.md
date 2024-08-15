# Devcontainer setup with flatpak vscode with podman

NOTE: you must install `podman` in host machine.

Install flatpak vscode and podman
```
flatpak --user install -y \
  com.visualstudio.code \
  com.visualstudio.code.tool.podman
```

start podman socket
```
systemctl --user enable --now podman.socket && \
systemctl --user status podman.socket
```

override vscode flatpak settings
```
flatpak --user override \
  --filesystem=xdg-run/podman:ro \
  --filesystem=/run/user/$UID/podman/podman.sock:ro \
  --filesystem=/tmp:rw \
  com.visualstudio.code
```

check podman socket connection
```
curl -w "\n" \
  -H "Content-Type: application/json" \
  --unix-socket /run/user/$UID/podman/podman.sock \
  http://localhost/_ping
```

install [devcontainers](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers) extension
```
flatpak --user run --command=sh com.visualstudio.code -c \
  "/app/extra/vscode/bin/code --install-extension ms-vscode-remote.remote-containers"
```

replace docker with podman
change from vscode user settings
```
"dev.containers.dockerPath": "/app/tools/podman/bin/podman/podman-remote"
OR
mkdir -p ~/.var/app/com.visualstudio.code/config/Code/User
nano ~/.var/app/com.visualstudio.code/config/Code/User/settings.json
{
    "dev.containers.dockerPath": "/app/tools/podman/bin/podman-remote"
}
```

testing if vscode flatpak is connected and working with podman
```
flatpak --user run --command=sh com.visualstudio.code
/app/tools/podman/bin/podman-remote version
/app/tools/podman/bin/podman-remote run --rm hello-world
```
