# Gitea Compose

![Gitea](https://img.shields.io/badge/Gitea-609926?logo=gitea&labelColor=gray&logoColor=white)

Docker Compose configuration for Gitea, a self-hosted Git service.

## 📦 Requirements

- Docker

## ⚙️ Setup

### Create user for gitea

Gitea needs a user to run under.
You can create a new user named `git` with the following command:

```sh
sudo adduser git
```

Add the user `git` to the `docker` group to allow it to run Docker commands:

```sh
sudo usermod -aG docker git
```

### Add a gitea shim on the host

_/usr/local/bin/gitea-keys

```sh
#!/bin/sh
# Called by sshd as AuthorizedKeysCommand. Args: %u %t %k
exec docker exec -i gitea /usr/local/bin/gitea keys \
    -c /etc/gitea/app.ini -e git -u "$1" -t "$2" -k "$3"
```

_/usr/local/bin/gitea-keys

```sh
#!/bin/sh
# Login shell for the host 'git' user; runs the forced command inside the container.
exec docker exec -i \
    --env SSH_ORIGINAL_COMMAND="$SSH_ORIGINAL_COMMAND" \
    gitea bash "$@"
```

Set the `git` user login shell to the gitea shim:

```sh
sudo usermod -s /usr/local/bin/gitea-shell git
```

Run the following command to make the shim executable:

```sh
sudo chmod 755 /usr/local/bin/gitea-*
```

### SSH Passthroug setup

_/etc/ssh/sshd_config_

```diff
+ Match User git
+     AuthorizedKeysCommandUser git
+     AuthorizedKeysCommand /usr/local/bin/gitea-keys %u %t %k
+     AuthorizedKeysFile none
+     PasswordAuthentication no
+     PermitTTY no
+     X11Forwarding no
+     AllowAgentForwarding no
+     AllowTcpForwarding no
```

### Create gitea directories

## 🚀 Usage

## 📚 Reference

## 📄 License

MIT License

## 🔗 Related Projects
