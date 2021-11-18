---
title: Documentação da API

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - python
  - javascript

toc_footers:
  - <a href='https://github.com/slatedocs/slate'>Documentação Powered By Slate</a>

includes:
  - multitenancy
  - login
  - patients
  - professionals
  - example
  - errors

search: true

code_clipboard: true

meta:
  - name: description
    content: Documentation for the Kittn API
---

# Introdução

Bem-vindo(a) à documentação oficial da API da Lab Saúde! Aqui você vai poder explorar todas as funcionalidades da Lab em um ambiente de homologação criado especialmente para você.

Nós temos exemplos em Shell, Python, and JavaScript (usando a biblioteca axios)! Você pode visualizar os exemplos de implementação das chamadas à api na área mais escura a direita, e você pode trocar para a sua linguagem de preferência nas abas do canto superior direito.

Caso você tenha alguma dúvida que não consiga ser respondida por meio dessa documentação, sinta-se livre para entrar em contato com o nosso [suporte técnico](mailto:ricardo@zeropay.io)

# Autenticação

> Para se autenticar, use esse código:

```python
import requests

headers = {"x_auth_token": "na-bruma-leve-das-paixoes-que-vem-de-dentro"}

# Aqui usaremos um GET, mas poderia ser utilizado qualquer método.
api = requests.get("endpoint_da_api_aqui", headers=headers)
```

```shell
curl "endpoint_da_api_aqui" \
  -H "x_auth_token: na-bruma-leve-das-paixoes-que-vem-de-dentro"
```

```javascript
const axios = require("axios");

const apiResponse = await axios.get("endpoint_da_api_aqui", {
  headers: {
    x_auth_token: "na-bruma-leve-das-paixoes-que-vem-de-dentro",
  },
});
```

> Você precisa modificar `na-bruma-leve-das-paixoes-que-vem-de-dentro` com o token que você recebeu na hora do login.

<!-- TODO Linkar seções de paciente, profissional e clínica aos respectivos links -->

A Lab Saúde utiliza tokens recebidos no login para autorizar o acesso à API. Para realizar login você precisa de uma conta válida de uma pessoa paciente, profissional ou uma clínica.

A API espera que o token recebido no login seja incluído em todos os requests feitos ao servidor em um `header` seguindo o padrão:

`x_auth_token: na-bruma-leve-das-paixoes-que-vem-de-dentro`

<aside class="notice">
Você precisa modificar <code>na-bruma-leve-das-paixoes-que-vem-de-dentro</code> com o token que você recebeu na hora do login.
</aside>

<!-- TODO Add HTTP Request seguindo exemplo do tópico Kittens -->
