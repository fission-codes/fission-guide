---
description: >-
  Instruções para configuração e registro do DNS e domínio customizado para se
  utilizar no Fission
---

# Domínios Customizados

Toda aplicação no Fission ganha um subdomínio grátis.

Atualmente, esses domínios são do tipo `YOURUSERNAME.fission.name`, mas estamos trabalhando em melhorias onde suas aplicações terão o domínio `YOURAPPNAME.fission.app`.

Para utilizar um domínio customizado, como `YOURAPPNAME.fission.app`, você deve fazer o apontamento dele nos nossos servidores que controlam os nomes e registros.

{% hint style="warning" %}
Nota: Atualmente os domínios customizados precisam de nossa aprovação manual. Futuramente isso será automatizado e integrado no Fission CLI. Enquanto isso, para utilizar um domínio customizado, [contate nosso suporte em](https://fission.codes/support).
{% endhint %}

## Nome dos servidores \(Nameservers\) do Fission

* ns1.fission.systems
* ns2.fission.systems
* ns3.fission.systems
* ns4.fission.systems

## Mudando o nome dos servidores no Namecheap

Vá ao domínio que você deseja hospedar no Fission no seu 'Namecheap admin':

![](../.gitbook/assets/screenshot-2020-02-12-at-11.07.56-am.png)

No local onde está escrito "Namecheap BasicDNS", selecione "Custom DNS" no menu suspenso:

![](../.gitbook/assets/screenshot-2020-02-12-at-11.08.39-am.png)

Entre em `ns1.fission.systems` e `ns2.fission.systems` e clique no marcador de 'correto' verde para salvar.

