# 🗂️ dotfiles

Configurações de ambiente de desenvolvimento no WSL2 com Debian.

---

## 📦 O que está aqui

| Arquivo | O que faz |
|---|---|
| `.bashrc` | Configurações do terminal (aliases, variáveis de ambiente, nvm, pyenv) |
| `.gitconfig` | Configurações globais do Git |
| `install.sh` | Script que configura tudo automaticamente numa máquina nova |

---

## 🚀 Instalação numa máquina nova

> Pré-requisitos: WSL2 com Debian instalado e Docker configurado.

```bash
git clone git@github.com:elciomf/dotfiles.git
cd dotfiles && ./install.sh
```

O script vai:
- Criar os symlinks dos arquivos de configuração
- Instalar o **nvm** (gerenciador de versões do Node.js)
- Instalar o **pyenv** (gerenciador de versões do Python)

---

## 🔄 Como atualizar os dotfiles

Como os arquivos usam **symlinks**, qualquer edição no `.bashrc` ou `.gitconfig` já reflete automaticamente na pasta `~/dotfiles`. Basta commitar:

```bash
cd ~/dotfiles
git add .
git commit -m "Descreve o que mudou"
git push
```

---

## 🐳 Docker

Docker não inicia automaticamente no WSL2. O `.bashrc` já possui um auto-start configurado, mas caso precise iniciar manualmente:

```bash
sudo service docker start
```

---

## 🟢 Node.js (nvm)

```bash
nvm install --lts        # instala a versão LTS mais recente
nvm install 20           # instala uma versão específica
nvm use 20               # troca de versão no terminal atual
nvm alias default 20     # define versão padrão
nvm ls                   # lista versões instaladas
```

**Por projeto:** cria um `.nvmrc` na raiz com a versão desejada e roda `nvm use`.

```bash
echo "20" > .nvmrc
nvm use
```

---

## 🐍 Python (pyenv)

```bash
pyenv install 3.13       # instala uma versão específica
pyenv global 3.13        # define versão padrão global
pyenv local 3.11         # define versão só para a pasta atual
pyenv versions           # lista versões instaladas
```

**Por projeto:** o `pyenv local` cria um `.python-version` na raiz da pasta automaticamente.

---

## 🔑 SSH

A chave SSH está em `~/.ssh/id_ed25519` e está configurada no GitHub.

Para gerar uma nova chave numa máquina nova:

```bash
ssh-keygen -t ed25519 -C "seu@email.com"
cat ~/.ssh/id_ed25519.pub  # copia e adiciona no GitHub → Settings → SSH keys
ssh -T git@github.com      # testa a conexão
```

---

## 🛠️ Resetar o WSL do zero

1. No PowerShell do Windows: `wsl --unregister Debian`
2. Reinstalar: `wsl --install -d Debian`
3. Configurar Docker (ver abaixo)
4. Clonar este repositório e rodar `./install.sh`

### Instalação do Docker no Debian

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

Reinicia o WSL: `wsl --shutdown` e reabre o terminal.