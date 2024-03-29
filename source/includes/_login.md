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

response = requests.post('htps://homolog.api.lab-saude.com/tenants/login/user', data={"Authorization": payload}, headers=headers)
```

```shell
curl --location --request POST 'htps://homolog.api.lab-saude.com/tenants/login/user' \
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
  "htps://homolog.api.lab-saude.com/tenants/login/user",
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

response = requests.post('htps://homolog.api.lab-saude.com/tenants/login/free', data=payload, headers=headers)
```

```shell
curl --location --request POST 'htps://homolog.api.lab-saude.com/tenants/login/free' \
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
  "htps://homolog.api.lab-saude.com/tenants/login/free",
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

response = requests.post('htps://homolog.api.lab-saude.com/doctors/login/clinics-medic', data=payload, headers=headers)
```

```shell
curl --location --request POST 'htps://homolog.api.lab-saude.com/doctors/login/clinics-medic' \
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
  "htps://homolog.api.lab-saude.com/doctors/login/clinics-medic",
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