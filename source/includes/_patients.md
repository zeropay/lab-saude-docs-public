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

response = requests.post('https://homolog.api.lab-saude.com/tenants/users', data=body, headers=headers)
```

```shell
curl --location --request POST 'https://homolog.api.lab-saude.com/tenants/users' \
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
  "https://homolog.api.lab-saude.com/tenants/users",
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

response = requests.post('https://homolog.api.lab-saude.com/tenants/users/free', data=body, headers=headers)
```

```shell
curl --location --request POST 'https://homolog.api.lab-saude.com/tenants/users/free' \
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
  "https://homolog.api.lab-saude.com/tenants/users/free",
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
  ('file', ('Exemplo de CSV para registro de usuários na Lab Saúde', open('/path/até/o/arquivo/Exemplo de CSV para registro de usuários na Lab Saúde','rb'),'text/csv'))
]
headers = {"x_tenant_id": "homolog"}

response = requests.post("https://homolog.api.lab-saude.com/messages/user-register-email/lab-saude", headers=headers, data=payload, files=files)
```

```shell
curl --location --request POST 'htps://homolog.api.lab-saude.com/doctors/login/clinics-medic' \
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

const response = await axios.get(
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

## Listar paciente pelo id

```python
import requests

user_id = "uuid-do-usuario-vem-aqui"

headers = {
  'x_tenant_id': 'homolog',
  'x_auth_token': 'may-the-force-be-with-you'
}

response = requests.get("https://homolog.api.lab-saude.com/tenants/users/" + user_id, headers=headers)
```

```shell
curl --location --request GET 'https://homolog.api.lab-saude.com/tenants/users/uuid-do-usuario-vem-aqui' \
-H 'x_tenant_id: homolog' \
-H 'x_auth_token: may-the-force-be-with-you'
```

```javascript
const axios = require("axios");

const userId = "uuid-do-usuario-vem-aqui";

const apiResponse = await axios.get(
  `https://homolog.api.lab-saude.com/tenants/users/${userId}`,
  {
    headers: {
      x_tenant_id: "homolog",
      x_auth_token: "may-the-force-be-with-you",
    },
  }
);
```

> O comando acima retorna um JSON com a seguinte estrutura:

```json
{
  "id": "uuid-do-usuario-vem-aqui",
  "name": "Luke Skywalker",
  "email": "luke@jedi.com",
  "password": "senha-criptografada",
  "cpf": "786.985.230-99",
  "phone": "+99 (99) 9.9999-9999",
  "company_id": "JEDI",
  "created_at": "1951-09-25T18:19:01.911+00:00",
  "updated_at": "2021-11-18T18:19:01.911+00:00",
  "system_wallet_number": "123",
  "rg": null,
  "mother_name": null,
  "birth_date": "25/09/1951",
  "gender": null,
  "civil_state": null,
  "profession": null,
  "image_link": null,
  "one_signal_id": null,
  "payer": true,
  "requires_more_info": false
}
```

Este endpoint retorna as informações de um paciente específico, dependendo de qual ID seja passado na rota.

A API retorna um objeto contendo todos os dados cadastrados no banco referentes ao paciente em questão.

### Objeto de resposta descomplicado

| Campo                | Descrição                                                                                                                                                  | Tipo           |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------- |
| id                   | ID do paciente no banco da Lab                                                                                                                             | UUID           |
| name                 | Nome do paciente                                                                                                                                           | String         |
| email                | E-mail do paciente                                                                                                                                         | String         |
| password             | Senha do paciente criptografada utilizando Argon2                                                                                                          | String         |
| cpf                  | CPF do paciente                                                                                                                                            | String         |
| phone                | Número de telefone do paciente                                                                                                                             | String         |
| company_id           | Identificador que referencia um paciente à uma empresa do ecossistema Lab                                                                                  | String         |
| created_at           | Timestamp da data e hora de criação do paciente no banco                                                                                                   | Timestamp      |
| updated_at           | Timestamp da data e hora da última atualização do paciente no banco                                                                                        | Timestamp      |
| system_wallet_number | Caso seja um paciente pagante, recebe o número da carteira do paciente na System Saúde, parceira da Lab Saúde. Caso seja um paciente gratuito, recebe null | String ou null |
| rg                   | R.G. do paciente                                                                                                                                           | String ou null |
| mother_name          | Nome da mãe do paciente                                                                                                                                    | String ou null |
| birth_date           | Data de nascimento do paciente                                                                                                                             | String ou null |
| gender               | Gênero do paciente                                                                                                                                         | String ou null |
| civil_state          | Estado civil do paciente                                                                                                                                   | String ou null |
| profession           | Profissão do paciente                                                                                                                                      | String ou null |
| image_link           | Link para imagem de perfil do paciente                                                                                                                     | String ou null |
| one_signal_id        | ID do paciente no OneSignal, utilizado para enviar push notifications                                                                                      | String ou null |
| payer                | Booleano que informa se o paciente é ou não um paciente pagante                                                                                            | Booleano       |
| requires_more_info   | Booleano que informa se o paciente precisa adicionar mais informações para que seu cadastro fique completo                                                 | Booleano       |

## Listar paciente pelo CPF

```python
import requests

user_cpf = "cpf-do-usuario-vem-aqui"

headers = {
  'x_tenant_id': 'homolog',
  'x_auth_token': 'may-the-force-be-with-you'
}

response = requests.get("https://homolog.api.lab-saude.com/tenants/users/" + user_cpf, headers=headers)
```

```shell
curl --location --request GET 'https://homolog.api.lab-saude.com/tenants/users/cpf-do-usuario-vem-aqui' \
-H 'x_tenant_id: homolog' \
-H 'x_auth_token: may-the-force-be-with-you'
```

```javascript
const axios = require("axios");

const userCpf = "cpf-do-usuario-vem-aqui";

const apiResponse = await axios.get(
  `https://homolog.api.lab-saude.com/tenants/users/${userCpf}`,
  {
    headers: {
      x_tenant_id: "homolog",
      x_auth_token: "may-the-force-be-with-you",
    },
  }
);
```

> O comando acima retorna um JSON com a seguinte estrutura:

```json
{
  "id": "e21a8cf0-0580-4a1a-8125-da129f64d128",
  "name": "Luke Skywalker",
  "email": "luke@jedi.com",
  "password": "senha-criptografada",
  "cpf": "cpf-do-usuario-vem-aqui",
  "phone": "+99 (99) 9.9999-9999",
  "company_id": "JEDI",
  "created_at": "1951-09-25T18:19:01.911+00:00",
  "updated_at": "2021-11-18T18:19:01.911+00:00",
  "system_wallet_number": "123",
  "rg": null,
  "mother_name": null,
  "birth_date": "25/09/1951",
  "gender": null,
  "civil_state": null,
  "profession": null,
  "image_link": null,
  "one_signal_id": null,
  "payer": true,
  "requires_more_info": false
}
```

De maneira similar ao endpoint anterior, este retorna as informações de um paciente específico, dependendo de qual CPF seja passado na rota.

A API retorna um objeto contendo todos os dados cadastrados no banco referentes ao paciente em questão.

### Objeto de resposta descomplicado

| Campo                | Descrição                                                                                                                                                  | Tipo           |
| -------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------- |
| id                   | ID do paciente no banco da Lab                                                                                                                             | UUID           |
| name                 | Nome do paciente                                                                                                                                           | String         |
| email                | E-mail do paciente                                                                                                                                         | String         |
| password             | Senha do paciente criptografada utilizando Argon2                                                                                                          | String         |
| cpf                  | CPF do paciente                                                                                                                                            | String         |
| phone                | Número de telefone do paciente                                                                                                                             | String         |
| company_id           | Identificador que referencia um paciente à uma empresa do ecossistema Lab                                                                                  | String         |
| created_at           | Timestamp da data e hora de criação do paciente no banco                                                                                                   | Timestamp      |
| updated_at           | Timestamp da data e hora da última atualização do paciente no banco                                                                                        | Timestamp      |
| system_wallet_number | Caso seja um paciente pagante, recebe o número da carteira do paciente na System Saúde, parceira da Lab Saúde. Caso seja um paciente gratuito, recebe null | String ou null |
| rg                   | R.G. do paciente                                                                                                                                           | String ou null |
| mother_name          | Nome da mãe do paciente                                                                                                                                    | String ou null |
| birth_date           | Data de nascimento do paciente                                                                                                                             | String ou null |
| gender               | Gênero do paciente                                                                                                                                         | String ou null |
| civil_state          | Estado civil do paciente                                                                                                                                   | String ou null |
| profession           | Profissão do paciente                                                                                                                                      | String ou null |
| image_link           | Link para imagem de perfil do paciente                                                                                                                     | String ou null |
| one_signal_id        | ID do paciente no OneSignal, utilizado para enviar push notifications                                                                                      | String ou null |
| payer                | Booleano que informa se o paciente é ou não um paciente pagante                                                                                            | Booleano       |
| requires_more_info   | Booleano que informa se o paciente precisa adicionar mais informações para que seu cadastro fique completo                                                 | Booleano       |

## Listar todos os pacientes

```python
import requests

headers = {
  'x_tenant_id': 'homolog',
  'x_auth_token': 'may-the-force-be-with-you'
}

response = requests.get('https://homolog.api.lab-saude.com/tenants/users', headers=headers)
```

```shell
curl --location --request GET 'https://homolog.api.lab-saude.com/tenants/users' \
-H 'x_tenant_id: homolog' \
-H 'x_auth_token: may-the-force-be-with-you'
```

```javascript
const axios = require("axios");

const apiResponse = await axios.post(
  "https://homolog.api.lab-saude.com/tenants/users",
  {
    headers: {
      x_tenant_id: "homolog",
      x_auth_token: "may-the-force-be-with-you",
    },
  }
);
```

> O comando acima retorna um JSON com a seguinte estrutura:

```json
[
  {
    "id": "e21a8cf0-0580-4a1a-8125-da129f64d128",
    "name": "Luke Skywalker",
    "email": "luke@jedi.com",
    "password": "senha-criptografada",
    "cpf": "786.985.230-99",
    "phone": "+99 (99) 9.9999-9999",
    "company_id": "JEDI",
    "created_at": "1951-09-25T18:19:01.911+00:00",
    "updated_at": "2021-11-18T18:19:01.911+00:00",
    "system_wallet_number": "123",
    "rg": null,
    "mother_name": null,
    "birth_date": "25/09/1951",
    "gender": null,
    "civil_state": null,
    "profession": null,
    "image_link": null,
    "one_signal_id": null,
    "payer": true,
    "requires_more_info": false
  },
  {
    "id": "9d17d3bd-4a78-43fa-8291-795ff27e0d2a",
    "name": "Baby Yoda",
    "email": "baby.yoda@jedi.com",
    "password": "senha-criptografada",
    "cpf": "522.803.300-91",
    "phone": "+99 (99) 9.9999-9999",
    "company_id": "lab-saude",
    "created_at": "1972-10-21T18:31:57.298+00:00",
    "updated_at": "2021-11-18T18:31:57.298+00:00",
    "system_wallet_number": null,
    "rg": null,
    "mother_name": null,
    "birth_date": null,
    "gender": null,
    "civil_state": null,
    "profession": null,
    "image_link": null,
    "one_signal_id": null,
    "payer": false,
    "requires_more_info": false
  }
]
```

Este endpoint serve para listar todos os pacientes cadastrados no banco. Feito especialmente para o ambiente de testes, é uma boa ferramenta para realizar queries que atuem sobre todos os pacientes e também para realizar debug durante o desenvolvimento.

A API retorna um array contendo todos os pacientes cadastrados no banco (tanto pagantes como gratuitos), assim como as suas informações.

### Objeto de resposta descomplicado

| Campo | Descrição          | Tipo             |
| ----- | ------------------ | ---------------- |
| -     | Array de pacientes | Array[Pacientes] |

## Modificar senha

```python
import requests

payload = {
  "cpf": "786.985.230-99",
  "email": "luke@jedi.com"
}

headers = {
  'x_tenant_id': 'homolog',
  'x_auth_token': 'may-the-force-be-with-you'
}

response = requests.put('https://homolog.api.lab-saude.com/tenants/users/recovery-password', data=payload, headers=headers)
```

```shell
curl --location --request PUT 'https://homolog.api.lab-saude.com/tenants/users/recovery-password' \
-H 'x_tenant_id: homolog' \
-H 'x_auth_token: may-the-force-be-with-you' \
-H 'Content-Type: application/json'
--data-raw '{
    "cpf": "786.985.230-99",
    "email": "luke@jedi.com"
}'
```

```javascript
const axios = require("axios");

const body = {
  cpf: "786.985.230-99",
  email: "luke@jedi.com",
};

const apiResponse = await axios.put(
  "https://homolog.api.lab-saude.com/tenants/users/recovery-password",
  body,
  {
    headers: {
      x_tenant_id: "homolog",
      x_auth_token: "may-the-force-be-with-you",
    },
  }
);
```

> O comando acima retorna um JSON com a seguinte estrutura:

```json
{
  "message": "An e-mail will be sent."
}
```

Por meio deste endpoint é possível recuperar a senha de um paciente.

Basta apenas enviar o e-mail e cpf do paciente em questão. Dentro de alguns minutos ele receberá um e-mail explicando como recuperar sua senha.

A API retorna uma mensagem padrão de sucesso avisando que o e-mail será enviado.

### Objeto de resposta descomplicado

| Campo   | Descrição                                                    | Tipo   |
| ------- | ------------------------------------------------------------ | ------ |
| message | Mensagem indicando que um e-mail será enviado para o usuário | String |

## Modificar imagem de perfil

```python
import requests

user_id = "uuid-do-paciente-aqui"

files=[
  ('profile_image', ('nome-da-imagem.jpeg', open('/path/para/a/imagem/nome-da-imagem.jpg','rb'),'image/jpeg'))
]

headers = {
  'x_tenant_id': 'homolog',
  'x_auth_token': 'may-the-force-be-with-you'
}

response = requests.put('https://homolog.api.lab-saude.com/tenants/users/profile-image/' + user_id, headers=headers, files=files)
```

```shell
curl --location --request PUT 'https://homolog.api.lab-saude.com/tenants/users/profile-image/uuid-do-paciente-aqui' \
-H 'x_tenant_id: homolog' \
-H 'x_auth_token: may-the-force-be-with-you' \
--form 'profile_image=@"/path/para/a/imagem/nome-da-imagem.jpg"'
```

```javascript
const axios = require("axios");
const FormData = require("form-data");
const fs = require("fs");
const data = new FormData();
data.append(
  "profile_image",
  fs.createReadStream("/path/para/a/imagem/nome-da-imagem.jpg")
);

const apiResponse = await axios.put(
  "https://homolog.api.lab-saude.com/tenants/users/profile-image",
  data,
  {
    headers: {
      x_tenant_id: "homolog",
      x_auth_token: "may-the-force-be-with-you",
      ...data.getHeaders(),
    },
  }
);
```

> O comando acima retorna um JSON com a seguinte estrutura:

```json
{
  "status": 200,
  "response": {
    "userImage": "link-para-a-nova-imagem-de-perfil"
  }
}
```

De forma simples e rápida é possível modificar a imagem de perfil de um paciente.

Basta apenas enviar o arquivo da nova imagem em JPEG, com o nome `profile_image` para este endpoint.

A API retorna um objeto contendo um link para a nova imagem de perfil do paciente no S3 da AWS.

### Objeto de resposta descomplicado

| Campo     | Descrição                                 | Tipo   |
| --------- | ----------------------------------------- | ------ |
| status    | Status da requisição HTTP                 | Número |
| response  | Objeto de resposta                        | Objeto |
| userImage | Link para a nova imagem do paciente no S3 | String |
