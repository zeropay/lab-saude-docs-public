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
| email                   | E-mail do paciente                                                                     | String   |
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
| email                   | E-mail do paciente                                                                     | String   |
| user_id                 | ID do paciente no banco da Lab                                                         | UUID     |
| phone?                  | Número de telefone do paciente, ou uma string vazia                                    | String   |
| token                   | Token que será utilizado em requests futuros                                           | String   |
| requiresMoreInformation | Booleano que informa se o paciente está com todos os dados atualizados no banco ou não | Booleano |

## Login de profissionais e clínicas

```python
import requests

email = "email-do-profissional-ou-clinica-vem-aqui@mail.com"
password = "senha-do-profissional-ou-clinica-vem-aqui"

headers = {"x_tenant_id": "homolog"}
payload = {"email": email, "password": password}

response = requests.post('https://api.lab-saude.com/doctors/login/clinics-medic', data=payload, headers=headers)
```

```shell
curl --location --request POST 'https://api.lab-saude.com/doctors/login/clinics-medic' \
-H 'x_tenant_id: homolog' \
-H 'Content-Type: application/json' \
--data-raw '{
    "email": "email-do-profissional-ou-clinica-vem-aqui@mail.com",
    "password": "senha-do-profissional-ou-clinica-vem-aqui"
}'
```

```javascript
const axios = require("axios");

const email = "email-do-profissional-ou-clinica-vem-aqui@mail.com";
const password = "senha-do-profissional-ou-clinica-vem-aqui";

const payload = { email: email, password: password };

const apiResponse = await axios.post(
  "https://api.lab-saude.com/doctors/login/clinics-medic",
  payload,
  {
    headers: {
      x_tenant_id: "homolog",
    },
  }
);
```

> O comando acima retorna um JSON com a seguinte estrutura caso o login seja para um profissional:

```json
{
  "type": "medic",
  "user": {
    "id": "c185b4a0-b5ff-4165-ab6b-54b27407fc34",
    "name": "Dr. Mundo",
    "email": "mundo@lol.com",
    "crm": "CRM-PE-123456",
    "accepts_presential_appointments": true,
    "office_state": "Pernambuco",
    "office_city": "Zaun",
    "office_neighbor": "Top Lane",
    "office_zip_code": "99.999-999",
    "office_street": "Avenida League Barros de Legends",
    "office_number": "999",
    "image_link": null,
    "appointment_price": "50.00",
    "specialties": ["Facadas", "Injeções"]
  },
  "accessToken": {
    "token": "Olhem-so!-Estou-carregando-o-Mundo-nos-ombros!",
    "reference_id": "c185b4a0-b5ff-4165-ab6b-54b27407fc34",
    "created_at": "2009-10-27T00:00:00.466+00:00",
    "updated_at": "2021-06-09T00:00:00.466+00:00",
    "id": 3
  }
}
```

> O comando acima retorna um JSON com a seguinte estrutura caso o login seja para uma clínica:

```json
{
  "type": "clinic",
  "user": {
    "id": "b0d190a9-d3bc-42d5-97b3-9746a8e45c31",
    "state": "PE",
    "city": "Zaun",
    "street": "Avenida dia do progresso",
    "number": "123",
    "complement": "Prédio de dois andares destruído",
    "name": "Academia de Zaun",
    "cnpj": "0000000000000.0001",
    "email": "academia.zaun@mail.com",
    "medics": [
      {
        "id": "c185b4a0-b5ff-4165-ab6b-54b27407fc34",
        "name": "Dr. Mundo",
        "email": "mundo@lol.com",
        "crm": "CRM-PE-123456",
        "accepts_presential_appointments": true,
        "office_state": "Pernambuco",
        "office_city": "Runeterra",
        "office_neighbor": "Top Lane",
        "office_zip_code": "99.999-999",
        "office_street": "Avenida League Barros de Legends",
        "office_number": "999",
        "image_link": null,
        "appointment_price": "50.00",
        "specialties": ["Facadas", "Injeções"]
      },
      {
        "id": "d4db9b8b-988e-4697-be56-da423e1fd93b",
        "name": "Singed",
        "email": "singed@mail.com",
        "crm": "CRM-PE-1591593",
        "accepts_presential_appointments": false,
        "office_state": "PE",
        "office_city": "Zaun",
        "office_neighbor": "Espinheiro",
        "office_zip_code": "52.020-060",
        "office_street": "Rua do Espinheiro",
        "office_number": "434",
        "image_link": "https://ddragon.leagueoflegends.com/cdn/img/champion/splash/Singed_0.jpg",
        "appointment_price": "120",
        "specialties": ["Poções", "Dependência Química"]
      }
    ]
  },
  "accessToken": {
    "token": "Zaun-é-um-grande-distrito-abaixo-da-cidade,-aos-pés-de-cânions-profundos-e-vales-que-cortam-Piltover",
    "reference_id": "b0d190a9-d3bc-42d5-97b3-9746a8e45c31",
    "created_at": "2009-10-27T00:00:00.493+00:00",
    "updated_at": "2021-11-16T20:38:35.493+00:00",
    "id": 285
  }
}
```

Referente ao login no sistema para profissionais da saúde. Profissionais e clínicas cadastrados na Lab Saúde podem realizar o login.

Para realizar a autenticação, é preciso enviar no request um objeto contendo os campos `email` e `password`, ambos strings.

### Objeto de resposta descomplicado - login de PROFISSIONAIS

| Campo                           | Descrição                                                                         | Tipo           |
| ------------------------------- | --------------------------------------------------------------------------------- | -------------- |
| type                            | Tipo do login, no caso do login para profissionais sempre será "medic"            | String         |
| user                            | Objeto contendo as informações básicas do profissional                            | Objeto         |
| id                              | ID do profissional no banco da Lab                                                | UUID           |
| name                            | Nome do profissional da saúde                                                     | String         |
| email                           | E-mail do profissional                                                            | String         |
| crm                             | CRM do profissional, seguindo o padrão `CRM-UF-NÚMERO`                            | String         |
| accepts_presential_appointments | Booleano que informa se o profissional aceita consultas presenciais ou não        | Booleano       |
| office_state                    | Estado do endereço onde o profissional exerce sua profissão                       | String         |
| office_city                     | Cidade do endereço onde o profissional exerce sua profissão                       | String         |
| office_neighbor                 | Bairro do endereço onde o profissional exerce sua profissão                       | String         |
| office_zip_code                 | CEP do endereço onde o profissional exerce sua profissão                          | String         |
| office_street                   | Rua do endereço onde o profissional exerce sua profissão                          | String         |
| office_number                   | Número do endereço onde o profissional exerce sua profissão                       | String         |
| image_link                      | Link da imagem do profissional, caso exista, caso não exista, será retornado null | String ou null |
| appointment_price               | Preço cheio da consulta com o profissional                                        | Objeto         |
| specialties                     | Lista de especialidades do profissional                                           | Array[String]  |
| accessToken                     | Objeto contendo as informações do token de acesso do login                        | Objeto         |
| token                           | Token para ser utilizado em requests futuros                                      | String         |
| reference_id                    | ID do profissional referente ao token no banco da Lab                             | UUID           |
| created_at                      | Timestamp representando data de criação do token                                  | Timestamp      |
| updated_at                      | Timestamp representando modificações feitas no banco de dados                     | Timestamp      |
| id (accessToken)                | ID incremental do token, referente ao ID do token no banco                        | Número         |

### Objeto de resposta descomplicado - login de CLÍNICAS

| Campo            | Descrição                                                                                  | Tipo                 |
| ---------------- | ------------------------------------------------------------------------------------------ | -------------------- |
| type             | Tipo do login, no caso do login para profissionais sempre será "clinic"                    | String               |
| user             | Objeto contendo as informações básicas da clínica                                          | Objeto               |
| id               | ID da clínica no banco da Lab                                                              | UUID                 |
| state            | Estado do endereço onde a clínica atua                                                     | String               |
| city             | Cidade do endereço onde a clínica atua                                                     | String               |
| street           | Rua do endereço onde a clínica atua                                                        | String               |
| number           | Número do endereço onde a clínica atua                                                     | String               |
| complement       | Complemento do endereço onde a clínica atua                                                | String               |
| name             | Nome da clínica                                                                            | String               |
| cnpj             | CNPJ da clínica                                                                            | String               |
| email            | E-mail da clínica                                                                          | String               |
| medics           | Lista de profissionais que atuam na clínica, segundo o padrão de dados do exemplo anterior | Array[Profissionais] |
| accessToken      | Objeto contendo as informações do token de acesso do login                                 | Objeto               |
| token            | Token para ser utilizado em requests futuros                                               | String               |
| reference_id     | ID da clínica referente ao token no banco da Lab                                           | UUID                 |
| created_at       | Timestamp representando data de criação do token no banco de dados da Lab                  | Timestamp            |
| updated_at       | Timestamp representando modificações feitas no token no banco de dados                     | Timestamp            |
| id (accessToken) | ID incremental do token no banco da Lab                                                    | Número               |

# Pacientes

É possível realizar diversas ações por meio da API da Lab Saúde. Algumas das mais importantes são, sem dúvidas, as operações em cima dos pacientes, tanto pagos quanto gratuitos.

A seguir, veremos as várias operações possíveis de serem feitas.

## Criar um paciente pago

```python
import requests

body = {
  "name": "Luke Skywalker",
  "email": "luke@jedi.com",
  "password": "ultimo-jedi@123",
  "passwordConfirmation": "ultimo-jedi@123",
  "cpf": "786.985.230-99",
  "phone": "+99 (99) 9.9999-9999",
  "birth_date": "25/09/1951",
  "system_wallet_number": "123",
  "company_id": "JEDI",
  "payer": true
}

headers = {"x_tenant_id": "homolog"}

response = requests.post('https://api.lab-saude.com/tenants/users', data=body, headers=headers)
```

```shell
curl --location --request POST 'https://api.lab-saude.com/tenants/users' \
-H 'x_tenant_id: homolog' \
-H 'Content-Type: application/json' \
--data-raw '{
  "name": "Luke Skywalker",
  "email": "luke@jedi.com",
  "password": "ultimo-jedi@123",
  "passwordConfirmation": "ultimo-jedi@123",
  "cpf": "786.985.230-99",
  "phone": "+99 (99) 9.9999-9999",
  "birth_date": "25/09/1951",
  "system_wallet_number": "123",
  "company_id": "JEDI",
  "payer": true
}'
```

```javascript
const axios = require("axios");

const body = {
  name: "Luke Skywalker",
  email: "luke@jedi.com",
  password: "ultimo-jedi@123",
  passwordConfirmation: "ultimo-jedi@123",
  cpf: "786.985.230-99",
  phone: "+99 (99) 9.9999-9999",
  birth_date: "25/09/1951",
  system_wallet_number: "123",
  company_id: "JEDI",
  payer: true,
};

const apiResponse = await axios.post(
  "https://api.lab-saude.com/tenants/users",
  body,
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
    "name": "Luke Skywalker",
    "cpf": "786.985.230-99",
    "email": "luke@jedi.com",
    "user_id": "e21a8cf0-0580-4a1a-8125-da129f64d128",
    "phone": "+99 (99) 9.9999-9999"
  },
  "token": "may-the-force-be-with-you"
}
```

Neste endpoint é possível criar uma conta para pacientes pagantes. Ou seja, aqueles que poderão utilizar o aplicativo da Lab Saúde.

É necessário passar algumas informações básicas para que a pessoa tenha sua conta cadastrada no banco da Lab, assim como integrado com as parceiras do ecossistema Lab Saúde.

### Objeto de resposta descomplicado

| Campo    | Descrição                                    | Tipo    |
| -------- | -------------------------------------------- | ------- |
| status   | O status do request HTTP                     | Inteiro |
| response | Objeto contendo informações do paciente      | Objeto  |
| name     | Nome do paciente                             | String  |
| cpf      | cpf do paciente                              | String  |
| email    | E-mail do paciente                           | String  |
| user_id  | ID do paciente no banco da Lab               | UUID    |
| phone    | Número de telefone do paciente               | String  |
| token    | Token que será utilizado em requests futuros | String  |

## Criar um paciente gratuito

```python
import requests

body = {
  "fullName": "Baby Yoda",
  "email": "baby.yoda@jedi.com",
  "password": "1234",
  "passwordConfirmation": "1234",
  "CPF": "522.803.300-91",
  "cellPhone": "+99 (99) 9.9999-9999"
}

headers = {"x_tenant_id": "homolog"}

response = requests.post('https://api.lab-saude.com/tenants/users/free', data=body, headers=headers)
```

```shell
curl --location --request POST 'https://api.lab-saude.com/tenants/users/free' \
-H 'x_tenant_id: homolog' \
-H 'Content-Type: application/json' \
--data-raw '{
  "fullName": "Baby Yoda",
  "email": "baby.yoda@jedi.com",
  "password": "1234",
  "passwordConfirmation": "1234",
  "CPF": "522.803.300-91",
  "cellPhone": "+99 (99) 9.9999-9999"
}'
```

```javascript
const axios = require("axios");

const body = {
  fullName: "Baby Yoda",
  email: "baby.yoda@jedi.com",
  password: "1234",
  passwordConfirmation: "1234",
  CPF: "522.803.300-91",
  cellPhone: "+99 (99) 9.9999-9999",
};

const apiResponse = await axios.post(
  "https://api.lab-saude.com/tenants/users/free",
  body,
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
    "name": "Baby Yoda",
    "cpf": "522.803.300-91",
    "email": "baby.yoda@jedi.com",
    "user_id": "9d17d3bd-4a78-43fa-8291-795ff27e0d2a",
    "phone": "+99 (99) 9.9999-9999"
  },
  "token": "may-the-force-be-with-you"
}
```

Neste endpoint é possível criar uma conta para pacientes gratuitos. Ou seja, aqueles que poderão utilizar somente o sistema para pacientes gratuitos da Lab Saúde.

Assim como no endpoint anterior, são necessárias informações básicas para o cadastro do paciente no banco. Diferente do paciente pago, o paciente gratuito não é integrado com as marcas parceiras do ecossistema Lab. Por isso, são enviadas menos informações nesse request.

### Objeto de resposta descomplicado

| Campo    | Descrição                                    | Tipo    |
| -------- | -------------------------------------------- | ------- |
| status   | O status do request HTTP                     | Inteiro |
| response | Objeto contendo informações do paciente      | Objeto  |
| name     | Nome do paciente                             | String  |
| cpf      | cpf do paciente                              | String  |
| email    | E-mail do paciente                           | String  |
| user_id  | ID do paciente no banco da Lab               | UUID    |
| phone    | Número de telefone do paciente               | String  |
| token    | Token que será utilizado em requests futuros | String  |

## Criar vários pacientes pagos de uma só vez

```python
import requests

payload={}
files=[
  ('file',('Exemplo de CSV para registro de usuários na Lab Saúde', open('/path/até/o/arquivo/Exemplo de CSV para registro de usuários na Lab Saúde','rb'),'text/csv'))
]
headers = {"x_tenant_id": "homolog"}

response = requests.post("https://homolog.api.lab-saude.com/messages/user-register-email/lab-saude", headers=headers, data=payload, files=files)
```

```shell
curl --location --request POST 'https://api.lab-saude.com/doctors/login/clinics-medic' \
-H 'x_tenant_id: homolog' \
-H 'Content-Type: application/json' \
--data-raw '{
    "email": "email-do-profissional-ou-clinica-vem-aqui@mail.com",
    "password": "senha-do-profissional-ou-clinica-vem-aqui"
}'

curl --location --request POST 'https://homolog.api.lab-saude.com/messages/user-register-email/lab-saude' \
-H 'x_tenant_id: homolog' \
--form 'file=@"/path/até/o/arquivo/Exemplo de CSV para registro de usuários na Lab Saúde"'
```

```javascript
const axios = require("axios");
const FormData = require("form-data");
const fs = require("fs");
const data = new FormData();

data.append(
  "file",
  fs.createReadStream(
    "/path/até/o/arquivo/Exemplo de CSV para registro de usuários na Lab Saúde"
  )
);

const response = await axios.post(
  "https://homolog.api.lab-saude.com/messages/user-register-email/lab-saude",
  data,
  {
    headers: {
      x_tenant_id: "homolog",
      ...data.getHeaders(),
    },
  }
);
```

> O comando acima retorna um status 204 (no content). O que significa que não há corpo de retorno.

Às vezes, quando temos que realizar muitos cadastros de uma vez pode ser útil utilizar este endpoint.

A API recebe um CSV que segue o padrão:

| Nome              | E-mail                    | CPF            |
| ----------------- | ------------------------- | -------------- |
| Darth Vader       | darth.vader@sith.com      | 602.424.060-09 |
| Emperor Palpatine | emperor@sith.com          | 621.077.290-00 |
| Darth Maul        | darth.maul@sith.com       | 842.857.610-68 |
| General Grievous  | general.grievous@sith.com | 883.615.030-64 |
| Conde Dookan      | conde.dookan@sith.com     | 545.084.210-40 |
| General Hux       | general.hux@sith.com      | 709.482.110-75 |

As informações referentes ao login no aplicativo da Lab Saúde serão enviadas por e-mail para os pacientes cadastrados.

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
