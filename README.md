#### devcontainer sample workspace for podman+vscode+flatpak+devcontainer


```
flatpak install --user com.visualstudio.code
flatpak install --user com.visualstudio.code.tool.podman

systemctl --user start podman.socket
systemctl --user enable --now podman.socket
systemctl --user status podman.socket

flatpak override --user --filesystem=xdg-run/podman:ro com.visualstudio.code
flatpak override --user --filesystem=/run/user/$UID/podman/podman.sock:ro com.visualstudio.code
flatpak override --user --filesystem=/tmp:rw com.visualstudio.code

curl -w "\n" -H "Content-Type: application/json" --unix-socket /run/user/$UID/podman/podman.sock http://localhost/_ping

"dev.containers.dockerPath": "podman-remote"
OR
/var/home/zihad/.var/app/com.visualstudio.code/config/Code/User/settings.json
{
    "dev.containers.dockerPath": "/app/tools/podman/bin/podman-remote"
}

flatpak run --user --command=bash com.visualstudio.code
/app/tools/podman/bin/podman-remote version
/app/tools/podman/bin/podman-remote run --rm hello-world

```
