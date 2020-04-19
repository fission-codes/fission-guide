---
description: Instalando as ferramentas de linha de comando do FISSION para iniciar as publicações do seu desktop
---

# Instalação 

Nossa plataforma é construída sobre algumas ferramentas de descentralização da internet, como o InterPlanetary File System \(IPFS\), onde é possíel você publicar diretamente do seu ambiente local de desenvolvimento, isso sem necessitar aprender ou implementar tecnologias DevOps.

Agora, vamos instalar o IPFS e a CLI do Fission para fazer tudo isso funcionar. Seguindo essas instruções abaixo, as instalações serão bem fáceis e rápidas. 

## Instalando o IPFS

### ipfs-desktop

[ipfs-desktop] (https://github.com/ipfs-shipyard/ipfs-desktop), se você prefere uma interface gráfica, o ipfs-desktop é uma ótima opção, esta te possibilita rodar o ipfs como serviço, além de facilitar o start/stop da aplicação (daemon).

Você pode fazer o download pelas 'releases' no link acima, ou usar seu gerenciador de pacotes de preferência:

* Homebrew \(macOS\): `brew cask install ipfs` 
* Chocolatey \(Windows\): `choco install ipfs-desktop` 
* Snap \(Linux\): `snap install ipfs-desktop` 

{% hint style="info" %}
Essa instalação também inclui a linha de comando 'ipfs-daemon'.
{% endhint %}

### IPFS por linha de comando

#### MacOs

Se você não estiver utilizando o ipfs-desktop, instale usando o brew:

```bash
brew install ipfs
```

Para executar o serviço do ipfs em background:

```bash
brew services start ipfs
```

#### Linux e Windows / WSL

Faça o download do binário para linux na [página de distribuições oficiais do IPFS](https://dist.ipfs.io/#go-ipfs).

Descompacte o arquivo e execute o script `./install.sh` \(Isso moverá o binary para o PATH do bin\).

```bash
$ tar xvfz go-ipfs.tar.gz
$ cd go-ipfs
$ ./install.sh
```

### Para todos os sistemas

Após a instalação, se feita corretamente, o seguinte comando funciona para todos os sitemas operacionais:

```bash
ipfs init
```
Por padrão, os arquivos ficam armazenados no diretório local em `.ipfs`.

O ipfs-desktop pode ser ligado e desligado pela interface gráfica. Para sistemas Linux, execute o daemon em segundo plano:

```bash
ipfs daemon &
```

Se você quiser facilitar o stop/start do daemon, favor visitar a [página de Troubleshooting](../appendix/troubleshooting.md#initd)

## Instalando o Fission CLI
 
 O Fission pela linha de comando \(CLI\) é a principal forma de se interagir com os serviços do Fission.

 {% hint style="info" %}
Para usuários do Windows, nós recomandamos usar o 'Windows Subsystem for Linux' \(WSL\). As instruções escritas abaixo para Linux, também se aplicam para o WSL, exceto as demarcadas em nota.
{% endhint %}

#### MacOs

Nós temos comandos para o brew que facilitam a instalação e execução do Fission:

```bash
$ brew tap fission-suite/fission
$ brew install fission-cli
```

#### Linux \(e manualmente no MacOS\)

Vá até a nossa página de [releases](https://github.com/fission-suite/cli/releases) no Github e baixe a última release para seu sistema operacional.

{% hint style="warning" %}
Nota: Ubuntu 19+ atualmente não é suportado devido [a um problema de build \#51](https://github.com/fission-suite/cli/issues/51)
{% endhint %}

Descompacte o arquivo e o movimente para o PATH:

```bash
$ sudo mv ./fission-cli-exe /usr/local/bin/fission
```

_**\(Apenas Linux\)**_  
****Linux requires an additional dependency:

```bash
$ sudo apt update
$ sudo apt install libpq-dev
```

Pronto! Faça uma última checagem para ver se está tudo correto:


```bash
$ fission --help
CLI to interact with Fission services

Usage: fission [--version] [--help] COMMAND
  Fission makes developing, deploying, updating and iterating on web
  applications quick and easy.
...
```
