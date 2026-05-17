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

And you have to get UID and GID of the user `git`:

**UID**:

```sh
id -u git
```

**GID**:

```sh
id -u git
```

### Add a gitea shim on the host

_/usr/local/bin/gitea_

```sh
ssh-original-command="$SSH_ORIGINAL_COMMAND" \
    /usr/bin/docker exec -i \
    --env SSH_ORIGINAL_COMMAND \
    -u git \
    gitea \
    /usr/local/bin/gitea "$@"
```

Run the following command to make the shim executable:

```sh
chmod 755 /usr/local/bin/gitea
```

### Create .env file

Create a `.env` file in the root of the project with the following content:

```env
UID=<UID of git user>
GID=<GID of git user>
```

For example:

```env
UID=1001
GID=1001
```

### Create gitea directories

## 🚀 Usage

## 📚 Reference

## 📄 License

## 🔗 Related Projects
