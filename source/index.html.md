---
title: Documentação da API

language_tabs: # must be one of https://git.io/vQNgJ
  - shell
  - python
  - javascript

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>

includes:
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

# Multitenancy no banco de dados

> Para dizer à API qual banco deve ser utilizado, use esse código:

```python
import requests

headers = {"x_tenant_id": "homolog"}

# Aqui usaremos um GET, mas poderia ser utilizado qualquer método.
api = requests.get("endpoint_da_api_aqui", headers=headers)
```

```shell
curl "endpoint_da_api_aqui" \
  -H "x_tenant_id: homolog"
```

```javascript
const axios = require("axios");

const apiResponse = await axios.get("endpoint_da_api_aqui", {
  headers: {
    x_tenant_id: "homolog",
  },
});
```

> Você precisa modificar `homolog` com o nome do banco desejado.

A Lab Saúde utiliza uma arquitetura de banco de dados chamada multi-tenant, em que cada empresa parceira da Lab tem o seu próprio banco, a fim de aumentar a segurança dos dados guardados. Caso você não saiba qual banco você deve utilizar, para realizar testes, o banco de homolog é o mais indicado.

A API espera que o identificador do banco seja incluído em todos os requests feitos ao servidor em um `header` seguindo o padrão:

`x_tenant_id: homolog`

# Login

A Lab Saúde possui um ecossistema composto por vários sistemas.

Por isso, tendo em vista o melhor entendimento da API, iremos segmentar entre os diferentes tipos de login.

## Login de pacientes pagos

```python
import requests
import base64

cpf = "cpf-do-paciente-vem-aqui"
password = "senha-do-paciente-vem-aqui"

headers = {"x_tenant_id": "homolog"}
payload = base64.b64encode({"user": cpf, "pass": password})

response = requests.post('https://api.lab-saude.com/tenants/login/user', data={"Authorization": payload}, headers=headers)
```

```shell
curl --location --request POST 'https://api.lab-saude.com/tenants/login/user' \
-H 'x_tenant_id: homolog' \
-H 'Content-Type: application/json' \
--data-raw '{
    "Authorization": "base64-encode-do-objeto-de-autenticacao"
}'
```

```javascript
const axios = require("axios");

const cpf = "cpf-do-paciente-vem-aqui";
const password = "senha-do-paciente-vem-aqui";

const payloadStringified = JSON.stringify({ user: cpf, pass: password });

const payload = Buffer.from(payloadStringified).toString("base64");

const apiResponse = await axios.post(
  "https://api.lab-saude.com/tenants/login/user",
  { Authorization: payload },
  {
    headers: {
      x_tenant_id: "homolog",
    },
  }
);
```

> O comando acima retorna um JSON com a seguinte estrutura:

```json
{
  "status": 200,
  "response": {
    "name": "Alceu Valença",
    "cpf": "544.356.480-32",
    "email": "valencinha@belledejour.com",
    "user_id": "9e2ad447-164a-4d3a-95cc-ebf705ee841e",
    "phone": ""
  },
  "token": "na-bruma-leve-das-paixoes-que-vem-de-dentro",
  "requiresMoreInformation": true
}
```

Referente ao login no aplicativo da Lab Saúde. Apenas pacientes pagantes da Lab Saúde possuem acesso a esse login, caso você queria fazer login como um paciente gratuito, por favor, se refira ao [login de pacientes gratuitos](#login-de-pacientes-gratuitos).

Este endpoint requer que o CPF e a senha do pacientes sejam transformados em uma string em base64 e enviados como um objeto com a chave `Authorization`.

### Objeto de resposta descomplicado

| Campo                   | Descrição                                                                              | Tipo     |
| ----------------------- | -------------------------------------------------------------------------------------- | -------- |
| status                  | O status do request HTTP                                                               | Inteiro  |
| response                | Objeto contendo informações do paciente                                                | Objeto   |
| name                    | Nome do paciente                                                                       | String   |
| cpf                     | cpf do paciente                                                                        | String   |
| email                   | email do paciente                                                                      | String   |
| user_id                 | ID do paciente no banco da Lab                                                         | UUID     |
| phone?                  | Número de telefone do paciente, ou uma string vazia                                    | String   |
| token                   | Token que será utilizado em requests futuros                                           | String   |
| requiresMoreInformation | Booleano que informa se o paciente está com todos os dados atualizados no banco ou não | Booleano |

## Login de pacientes gratuitos

```python
import requests

cpf = "cpf-do-paciente-vem-aqui"
password = "senha-do-paciente-vem-aqui"

headers = {"x_tenant_id": "homolog"}
payload = {"CPF": cpf, "password": password}

response = requests.post('https://api.lab-saude.com/tenants/login/free', data=payload, headers=headers)
```

```shell
curl --location --request POST 'https://api.lab-saude.com/tenants/login/free' \
-H 'x_tenant_id: homolog' \
-H 'Content-Type: application/json' \
--data-raw '{
    "CPF": "cpf-do-paciente-vem-aqui",
    "password": "senha-do-paciente-vem-aqui"
}'
```

```javascript
const axios = require("axios");

const cpf = "cpf-do-paciente-vem-aqui";
const password = "senha-do-paciente-vem-aqui";

const payload = { CPF: cpf, password: password };

const apiResponse = await axios.post(
  "https://api.lab-saude.com/tenants/login/free",
  payload,
  {
    headers: {
      x_tenant_id: "homolog",
    },
  }
);
```

> O comando acima retorna um JSON com a seguinte estrutura:

```json
{
  "status": 200,
  "response": {
    "name": "Rihanna Fenty",
    "cpf": "424.637.140-85",
    "email": "rihanna@diamonds.com",
    "user_id": "736088ee-4247-4972-b35a-e9e76e8a8545",
    "phone": "+99 (99) 9.9999-9999"
  },
  "token": "shine-bright-like-a-diamond",
  "requiresMoreInformation": false
}
```

Referente ao login no sistema para pacientes gratuitos da Lab Saúde. Qualquer paciente tem acesso a esse login, seja ele pagante ou não.

Diferente do login para pacientes pagos, aqui só é necessário enviar o CPF e a senha o paciente em um objeto simples.

### Objeto de resposta descomplicado

| Campo                   | Descrição                                                                              | Tipo     |
| ----------------------- | -------------------------------------------------------------------------------------- | -------- |
| status                  | O status do request HTTP                                                               | Inteiro  |
| response                | Objeto contendo informações do paciente                                                | Objeto   |
| name                    | Nome do paciente                                                                       | String   |
| cpf                     | cpf do paciente                                                                        | String   |
| email                   | email do paciente                                                                      | String   |
| user_id                 | ID do paciente no banco da Lab                                                         | UUID     |
| phone?                  | Número de telefone do paciente, ou uma string vazia                                    | String   |
| token                   | Token que será utilizado em requests futuros                                           | String   |
| requiresMoreInformation | Booleano que informa se o paciente está com todos os dados atualizados no banco ou não | Booleano |

# Kittens

## Get All Kittens

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get()
```

```shell
curl "http://example.com/api/kittens" \
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require("kittn");

let api = kittn.authorize("meowmeowmeow");
let kittens = api.kittens.get();
```

> The above command returns JSON structured like this:

```json
[
  {
    "id": 1,
    "name": "Fluffums",
    "breed": "calico",
    "fluffiness": 6,
    "cuteness": 7
  },
  {
    "id": 2,
    "name": "Max",
    "breed": "unknown",
    "fluffiness": 5,
    "cuteness": 10
  }
]
```

This endpoint retrieves all kittens.

### HTTP Request

`GET http://example.com/api/kittens`

### Query Parameters

| Parameter    | Default | Description                                                                      |
| ------------ | ------- | -------------------------------------------------------------------------------- |
| include_cats | false   | If set to true, the result will also include cats.                               |
| available    | true    | If set to false, the result will include kittens that have already been adopted. |

<aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside>

## Get a Specific Kitten

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.get(2)
```

```shell
curl "http://example.com/api/kittens/2" \
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require("kittn");

let api = kittn.authorize("meowmeowmeow");
let max = api.kittens.get(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "name": "Max",
  "breed": "unknown",
  "fluffiness": 5,
  "cuteness": 10
}
```

This endpoint retrieves a specific kitten.

<aside class="warning">Inside HTML code blocks like this one, you can't use Markdown, so use <code>&lt;code&gt;</code> blocks to denote code.</aside>

### HTTP Request

`GET http://example.com/kittens/<ID>`

### URL Parameters

| Parameter | Description                      |
| --------- | -------------------------------- |
| ID        | The ID of the kitten to retrieve |

## Delete a Specific Kitten

```python
import kittn

api = kittn.authorize('meowmeowmeow')
api.kittens.delete(2)
```

```shell
curl "http://example.com/api/kittens/2" \
  -X DELETE \
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require("kittn");

let api = kittn.authorize("meowmeowmeow");
let max = api.kittens.delete(2);
```

> The above command returns JSON structured like this:

```json
{
  "id": 2,
  "deleted": ":("
}
```

This endpoint deletes a specific kitten.

### HTTP Request

`DELETE http://example.com/kittens/<ID>`

### URL Parameters

| Parameter | Description                    |
| --------- | ------------------------------ |
| ID        | The ID of the kitten to delete |
