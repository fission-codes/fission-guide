# Usando a linha de comando do Fission para publicar sites feitos em React

Fission trabalha com qualquer site ou aplicação estática/client side, independentemente se for uma única página html, ou um gerador de sites estáticos como [Gatsby](https://www.gatsbyjs.org/) ou [Jekyll](https://jekyllrb.com/).

Neste tutorial, vamos utilizar o Gatsby para construir e publicar um site estático, mas você pode usar qualquer outro de sua preferência.

Primeiramente, escolha um projeto, e entre no seu diretório `cd`. Vamos utilizar [reactjs.org](https://github.com/reactjs/reactjs.org) para que todos nós fiquemos na mesma página, caso tenha uma outra preferência, fique a vontade para usá-la.

```bash
$ git clone https://github.com/reactjs/reactjs.org
$ cd reactjs.org
```

Instale as dependências:

```bash
$ yarn
```

Construa o site:

```bash
$ yarn build
```

`cd` para dentro do diretório:

```bash
$ cd public
```

Lance-o no IPFS!

```bash
$ fission up
🚀 Now live on the network
👌 QmTurDD2LNBmmrxP2czBmL15415KFoxEXQ8nvJGhLrgJvU
📝 DNS Updated. Check out your site at:
🔗 21ebedd5d4070a521f83.demo.runfission.com
```

Agora copie o link gerado no seu terminal para o browser, e veja seu site hospedado em uma rede descentralizada!
