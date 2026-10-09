# syncro-agent

Installs the Syncro RMM agent on a headless Linux machine and confirms that its systemd service (`syncro.service`) is running.

## Run

```bash
curl -fsSL https://davidaday.github.io/syncro-agent/install | sudo bash
```

The script prompts for the installer link (Syncro > Agent download > Linux > copy link). To skip the prompt, pass the link as an argument:

```bash
curl -fsSL https://davidaday.github.io/syncro-agent/install | sudo bash -s -- "<installer link>"
```

Quote the link, because it contains `&`.

If GitHub Pages is unavailable, use the raw file instead: `https://raw.githubusercontent.com/davidaday/syncro-agent/main/install`.

## What it does

1. Downloads the installer with `curl -L -OJ`, which keeps the server-provided filename. The installer reads its registration token from the `[[...]]` part of that filename.
2. Adds `--skip-os-check` automatically when `/etc/os-release` `ID` is not one of the distros Syncro supports: `ubuntu`, `debian`, `rhel`, `centos`, `fedora`. Pass `--skip-os-check` or `--no-skip-os-check` to override.
3. Runs the installer as root, then waits up to 30s for `syncro.service` (or the older `syncro-agent.service`) to become active.
4. Prints `SUCCESS` and exits 0, or prints `FAILURE` with the reason and recent service logs and exits 1. If the agent is already installed and running, it reports that and exits 0.

Installer links are signed and expire, so don't commit them here.
