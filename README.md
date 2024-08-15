#### devcontainer sample workspace for podman+vscode+flatpak+devcontainer


```
flatpak install -y --user \
  com.visualstudio.code \
  com.visualstudio.code.tool.podman

systemctl --user enable --now podman.socket && \
systemctl --user status podman.socket


flatpak override --user \
  --filesystem=xdg-run/podman:ro \
  --filesystem=/run/user/$UID/podman/podman.sock:ro \
  --filesystem=/tmp:rw \
  com.visualstudio.code


curl -w "\n" \
  -H "Content-Type: application/json" \
  --unix-socket /run/user/$UID/podman/podman.sock \
  http://localhost/_ping

flatpak run --user --command=sh com.visualstudio.code -c \
  "/app/extra/vscode/bin/code --install-extension ms-vscode-remote.remote-containers"

"dev.containers.dockerPath": "/app/tools/podman/bin/podman/podman-remote"
OR
mkdir -p ~/.var/app/com.visualstudio.code/config/Code/User
nano ~/.var/app/com.visualstudio.code/config/Code/User/settings.json
{
    "dev.containers.dockerPath": "/app/tools/podman/bin/podman-remote"
}

flatpak run --user --command=sh com.visualstudio.code
/app/tools/podman/bin/podman-remote version
/app/tools/podman/bin/podman-remote run --rm hello-world

```
