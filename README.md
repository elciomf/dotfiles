# dotfiles

Development environment configuration for WSL2 running Debian.

---

## Contents

| File | Description |
|---|---|
| `.bashrc` | Shell configuration (aliases, environment variables, nvm, pyenv) |
| `.gitconfig` | Global Git configuration |
| `install.sh` | Bootstrap script for setting up a new machine |

---

## Installation

**Prerequisites:** WSL2 with Debian and Docker already configured.

```bash
git clone git@github.com:elciomf/dotfiles.git
cd dotfiles && ./install.sh
```

The script will:

- Create symlinks for all configuration files
- Install **nvm** (Node.js version manager)
- Install **pyenv** (Python version manager)

---

## Keeping dotfiles up to date

All configuration files are managed via **symlinks**, so any changes made to `.bashrc` or `.gitconfig` are automatically reflected in `~/dotfiles`. To persist changes:

```bash
cd ~/dotfiles
git add .
git commit -m "Description of changes"
git push
```

---

## Docker

Docker does not start automatically in WSL2. The `.bashrc` includes an auto-start configuration, but it can also be started manually:

```bash
sudo service docker start
```

---

## Node.js (nvm)

```bash
nvm install --lts        # install the latest LTS release
nvm install 20           # install a specific version
nvm use 20               # switch version in the current session
nvm alias default 20     # set the default version
nvm ls                   # list installed versions
```

**Per-project version pinning:** create an `.nvmrc` file at the project root and run `nvm use`.

```bash
echo "20" > .nvmrc
nvm use
```

---

## Python (pyenv)

```bash
pyenv install 3.13       # install a specific version
pyenv global 3.13        # set the global default version
pyenv local 3.11         # set a version for the current directory only
pyenv versions           # list installed versions
```

Running `pyenv local` automatically creates a `.python-version` file at the project root.

---

## SSH

The SSH key is located at `~/.ssh/id_ed25519` and is registered on GitHub.

To generate a new key on a fresh machine:

```bash
ssh-keygen -t ed25519 -C "your@email.com"
cat ~/.ssh/id_ed25519.pub  # copy the output and add it to GitHub -> Settings -> SSH keys
ssh -T git@github.com      # verify the connection
```

---

## Resetting WSL from scratch

1. From a Windows PowerShell session: `wsl --unregister Debian`
2. Reinstall the distribution: `wsl --install -d Debian`
3. Set up Docker (see below)
4. Clone this repository and run `./install.sh`

### Installing Docker on Debian

```bash
sudo apt update
sudo apt install -y ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] \
  https://download.docker.com/linux/debian \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo usermod -aG docker $USER
```

Restart WSL after installation: run `wsl --shutdown` and reopen the terminal.
