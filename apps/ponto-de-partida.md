---
description: >-
  Como criar seu primeiro site rodando na plataforma IPFS usando esse guia para
  iniciantes
---

# Ponto de Partida

## Criando uma conta ou fazendo Login

Agora que você terminou a instalação da plataforma `fission`, é necessário criar uma conta:

```bash
$ fission register
✅ Registered & Login in
```

Agora está tudo certo! Suas credenciais de acesso estão dentro de `~/.fission.yaml`

Caso você já possua uma conta, faça o login dessa forma:

```bash
$ fission login
Username: #Siga os 'prompts' para informar seu usuário e senha
```

{% hint style="info" %}
Por padrão, os endereços da rede são baseados no seu usuário de acesso, se parecendo com - `USERNAME.fission.name`. No futuro, será possível adicionar domínios customizados.
{% endhint %}

## Executando o IPFS

Para utilizar o `fission` você precisará estar executando o IPFS

Se sua versão do IPFS for [ipfs-desktop](https://github.com/ipfs-shipyard/ipfs-desktop), garanta que ele esteja em execução.

Se sua versão do `ipfs` for via linha de comando, execute o seguinte código em outra aba do terminal:

```bash
$ ipfs daemon
```

## Criando uma webpage simples

Vamos criar uma página 'interplanetária' simples!

Crie um novo diretório de projeto e adicione um arquivo `index.html`:

```bash
$ mkdir hello-universe
$ cd hello-universe
$ touch index.html
```

Adicione um conteúdo inicial a esse arquivo `index.html`:

{% code title="index.html" %}
```markup
<html>
  <head>
    <title>Olá Universo!</title>
  </head>
  <body>
    <h1>Olá Universo!</h1>
    <p>Isto está no InterPlanetary File System.</p>
    <p>Assisted by <a href="https://fission.codes">Fission</a>.</p>
  </body>
</html>
```
{% endcode %}

## Hora do deploy interplanetário!

Tenha certeza de que você está na pasta correta do projeto \(`hello-universe`\), e execute:

```bash
$ fission up
🚀 Now live on the network
👌 QmPmZDd6esqzsc2R1i8t5DcRGeRKrNomMhqj232Cz6heNW
📝 DNS Updated. Check out your site at:
🔗 diffusiondemo.fission.name
```

Copie a URL que aparece na última linha \(🔗\) e cole no seu navegador para ver seu novo site, hospedado em uma rede descentralizada. Fácil assim!

_Note: Pode levar algum tempo para a replicação do DNS. Então aguarde 1 ou 2 minutos se o site não carregar imediatamente. Se você é do tipo impaciente, copie o `content-id` da saída da linha de comando e veja em `https://ipfs.runfission.com/ipfs/{YOUR_CONTENT_ID}`_

{% hint style="success" %}
Curiosidade sobre o IPFS: Todos que seguirem esse tutorial do começo ao fim, acabarão com a mesma chave de conteúdo \(Content ID\). Isso significa pode puxar instantaneamente esse site da demonstração do seu "vizinho" \(se estiver com pressa\), caso ele já tenha feito o deploy.
{% endhint %}

