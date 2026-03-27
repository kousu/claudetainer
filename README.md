# claudtainer

Run claude-code in a rootless container, so it can't accidentally rm your system.

Claude is sandboxed, even in the container: it doesn't even have container root.

## Dependencies

You need docker, but actually I'm 99% sure you need podman.
Install [podman-docker](https://pkgs.org/download/podman-docker).

Also you need `jq`.

## Usage

The first time you run this it will build a container image and
put `claude-code` in it.

When you run it, your **current directory** is mounted into the container
as the working directory, with full read-write access, so Claude can work
on your project. The rest of your system is protected from mistakes.

If you have used `claude-code` on your host system already, your credentials
in `~/.claude.json` will be reused (but only the _credentials_); if not, you
will be prompted to log in; the credentials made that way will persist across
container runs but they will not touch your host `~/.claude.json`.

Credentials and settings (theme, etc)

If you need to debug something you can make extra shells in the container with

```
docker exec -it $CONTAINER_ID bash
```

If you need to debug something *as root* or, say, install software, use

```
docker exec -it --user root $CONTAINER_ID bash
```

## Changing credentials

Delete `~/.config/claudetainer/claude.json` and re-run.
