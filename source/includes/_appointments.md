# Consultas

Uma peça central do ecossistema da Lab Saúde são as consultas.

Nas consultas temos o encontro de pacientes e profissionais, além de transações com cartão.

A seguir, veremos as operações mais recorrentes e importantes da API para essa entidade.

## Criar uma consulta

```python
import requests

body = {
    "medicId": "ID-de-um-profissional-valido",
    "userId": "ID-de-um-usuario-valido" or "PROTO",
    "patient_name": "Tony Stark",
    "patient_cpf": "999.999.999-99",
    "address": "Avenida Malibu da California, 999, Malibu",
    "price": 100,
    "data": "2021-12-25T16:00:00.882Z",
    "specialty": "Cardiologia"
}

headers = {"x_tenant_id": "homolog", "x_auth_token": "I-am-Iron-Man"}

response = requests.post('https://homolog.api.lab-saude.com/tenants/appointments', data=body, headers=headers)
```

```shell
curl --location --request POST 'https://homolog.api.lab-saude.com/tenants/appointments' \
-H 'x_tenant_id: homolog' \
-H 'x_auth_token: I-am-Iron-Man' \
-H 'Content-Type: application/json' \
--data-raw '{
    "medicId": "ID-de-um-profissional-valido",
    "userId": "ID-de-um-usuario-valido" || "PROTO",
    "patient_name": "Tony Stark",
    "patient_cpf": "999.999.999-99",
    "address": "Avenida Malibu da California, 999, Malibu",
    "price": 100,
    "data": "2021-12-25T16:00:00.882Z",
    "specialty": "Cardiologia"
}'
```

```javascript
const axios = require("axios");

const body = {
  medicId: "ID-de-um-profissional-valido",
  userId: "ID-de-um-usuario-valido" || "PROTO",
  patient_name: "Tony Stark",
  patient_cpf: "999.999.999-99",
  address: "Avenida Malibu da California, 999, Malibu",
  price: 100,
  data: "2021-12-25T16:00:00.882Z",
  specialty: "Cardiologia",
};

const apiResponse = await axios.post(
  "https://homolog.api.lab-saude.com/tenants/appointments",
  body,
  {
    headers: {
      x_tenant_id: "homolog",
      x_auth_token: "I-am-Iron-Man",
    },
  }
);
```

> O comando acima retorna um JSON com a seguinte estrutura:

```json
{
  "statusCode": 200,
  "body": {
    "appointment": {
      "medic_id": "b5e1db2f-8ba8-44a0-8469-ae6ce130e459",
      "user_id": "064e8bb2-564b-4944-8eaa-fe1e602aa168",
      "patient_cpf": "999.999.999-99",
      "address": "Avenida Malibu da California, 999, Malibu",
      "price": 100,
      "data": "2021-12-25T16:00:00.882Z",
      "specialty": "Cardiologia",
      "id": "18415e13-bf47-40e6-a7c4-a7822cc90a6a",
      "created_at": "2021-11-19T19:56:04.324+00:00",
      "updated_at": "2021-11-19T19:56:04.324+00:00"
    },
    "calendar": {
      "id": "18415e13-bf47-40e6-a7c4-a7822cc90a6a",
      "startDate": "2021-12-25T16:00:00.882Z",
      "endDate": "2021-12-25T16:00:00.882Z",
      "doctorEmail": "estranho@vingadores.com",
      "doctorName": "Dr. Estranho",
      "patientEmail": "tony@vingadores.com",
      "patientName": "Tony Stark"
    }
  }
}
```

Neste endpoint é criada uma consulta simples, relacionando um paciente e um profissional.

Além disso, como pode ser visto no objeto de retorno, também é criado um evento no Google Calendar de ambos os participantes (caso o e-mail cadastrado deles seja aplicável ao Google Calendar).

Um ponto importante de se notar é que a consulta é criada, por padrão, com o status de pagamento pendente. Por isso, para que ela se torne uma consulta válida e seja possível entrar na chamada pelos sistemas Lab Saúde, ela precisa ser paga.

### Objeto de resposta descomplicado

| Campo           | Descrição                                                                         | Tipo      |
| --------------- | --------------------------------------------------------------------------------- | --------- |
| statusCode      | O status do request HTTP                                                          | Inteiro   |
| body            | Objeto contendo a resposta da API                                                 | Objeto    |
| appointment     | Objeto contendo as informações básicas da consulta criada                         | Objeto    |
| medic_id        | ID do profissional que irá atender à consulta                                     | UUID      |
| user_id         | ID do paciente que irá atender à consulta                                         | UUID      |
| patient_cpf     | CPF do paciente que irá atender à consulta                                        | String    |
| address         | Endereço da consulta                                                              | String    |
| price           | Preço da consulta                                                                 | Número    |
| data            | Data da consulta                                                                  | ISOString |
| specialty       | Especialidade da consulta                                                         | String    |
| id[appointment] | ID da consulta no banco da Lab                                                    | UUID      |
| created_at      | Data de criação da consulta no banco                                              | Timestamp |
| updated_at      | Data de atualização da consulta no banco                                          | Timestamp |
| calendar        | Objeto contendo as informações do evento criado no calendário dos participantes   | Objeto    |
| id[calendar]    | ID do evento criado no calendário                                                 | String    |
| startDate       | Data de início do evento no calendário                                            | Timestamp |
| endDate         | Data de fim do evento no calendário                                               | Timestamp |
| clinic_id       | ID da clínica a qual o profissional foi vinculado na hora do cadastro (se houver) | String    |
| doctorEmail     | E-mail do profissional                                                            | String    |
| doctorName      | Nome do profissional                                                              | String    |
| patientEmail    | E-mail do paciente                                                                | String    |
| patientName     | Nome do paciente                                                                  | String    |

## Criar e pagar uma consulta

```python
import requests

body = {
    "userId": "d8e27ac1-f860-4149-9e36-cdea0801dcb7",
    "medicId": "8b6937e4-ae89-49cb-8c28-e305fe01a76f",
    "card_id": "d5e71ac1-f7f5-4285-bc8b-7dc51376986d",
    "address": "Avenida Malibu da California, 999, Malibu",
    "price": 100,
    "date": "2021-12-25T16:00:00.882Z",
    "location_type": "Teleconsulta"
}

headers = {"x_tenant_id": "homolog", "x_auth_token": "I-am-Iron-Man"}

response = requests.post('https://homolog.api.lab-saude.com/tenants/appointments/create/pay', data=body, headers=headers)
```

```shell
curl --location --request POST 'https://homolog.api.lab-saude.com/tenants/appointments/create/pay' \
-H 'x_tenant_id: homolog' \
-H 'x_auth_token: I-am-Iron-Man' \
-H 'Content-Type: application/json' \
--data-raw '{
    "userId": "d8e27ac1-f860-4149-9e36-cdea0801dcb7",
    "medicId": "8b6937e4-ae89-49cb-8c28-e305fe01a76f",
    "card_id": "d5e71ac1-f7f5-4285-bc8b-7dc51376986d",
    "address": "Avenida Malibu da California, 999, Malibu",
    "price": 100,
    "date": "2021-12-25T16:00:00.882Z",
    "location_type": "Teleconsulta"
}'
```

```javascript
const axios = require("axios");

const body = {
  userId: "d8e27ac1-f860-4149-9e36-cdea0801dcb7",
  medicId: "8b6937e4-ae89-49cb-8c28-e305fe01a76f",
  card_id: "d5e71ac1-f7f5-4285-bc8b-7dc51376986d",
  address: "Avenida Malibu da California, 999, Malibu",
  price: 100,
  date: "2021-12-25T16:00:00.882Z",
  location_type: "Teleconsulta",
};

const apiResponse = await axios.post(
  "https://homolog.api.lab-saude.com/tenants/appointments/create/pay",
  body,
  {
    headers: {
      x_tenant_id: "homolog",
      x_auth_token: "I-am-Iron-Man",
    },
  }
);
```

> O comando acima retorna um JSON com a seguinte estrutura:

```json
{
  "statusCode": 200,
  "body": {
    "appointment": {
      "medic_id": "8b6937e4-ae89-49cb-8c28-e305fe01a76f",
      "user_id": "d8e27ac1-f860-4149-9e36-cdea0801dcb7",
      "patient_cpf": "999.999.999-99",
      "price": 100,
      "address": "Avenida Malibu da California, 999, Malibu",
      "data": "2021-12-25T16:00:00.882Z",
      "payment_status": "Concluído",
      "charge_id": "9ae58c21-0860-459f-bdd8-948076963c30",
      "location_type": "Teleconsulta",
      "id": "187ce1a6-31b7-4dcb-a935-a2485aa652c4",
      "created_at": "2021-11-19T20:26:22.380+00:00",
      "updated_at": "2021-11-19T20:26:22.380+00:00"
    },
    "calendar": {
      "id": "187ce1a6-31b7-4dcb-a935-a2485aa652c4",
      "startDate": "2021-12-25T16:00:00.882Z",
      "endDate": "2021-12-25T16:00:00.882Z",
      "doctorEmail": "estranho@vingadores.com",
      "doctorName": "Dr. Estranho",
      "patientEmail": "tony@vingadores.com",
      "patientName": "Tony Stark"
    }
  }
}
```

De forma análoga ao ponto anterior, aqui também conseguimos criar uma consulta relacionando um profissional e um paciente.

No entanto, desta vez, ela já é paga na hora da criação. Para tanto, é preciso enviar no corpo da requisição, um ID de um cartão de crédito cadastrado na Lab Saúde. Caso você ainda não tenha cadastrado nenhum cartão na sua conta da Lab, basta ir para a seção de cartões e seguir o passo-a-passo da [criação de um cartão]() <!-- TODO Add link para criação de cartão -->

Uma vez que o pagamento é processado e confirmado, você receberá um objeto de resposta muito parecido com o do ponto anterior. No entanto, desta vez, o status de pagamento da consulta já está como confirmado. O que quer dizer que ela já é uma consulta válida em todo o ecossistema Lab Saúde.

<aside class="notice">
Você pode notar que no objeto de resposta deste endpoint existe um campo a mais, chamado <code>charge_id</code> ele faz referência ao ID da cobrança feito ao cartão de crédito cadastrado.
</aside>

## Criar horários de atendimento de um profissional

```python
import requests

body = {
	"schedules": [
		{
			"day": "Segunda-feira",
			"start_hour": "08:00",
			"end_hour": "12:00"
		},
		{
			"day": "Terça-feira",
			"start_hour": "08:00",
			"end_hour": "12:00"
		},
		{
			"day": "Segunda-feira",
			"start_hour": "13:00",
			"end_hour": "15:00"
		}
	]
}


headers = {"x_tenant_id": "homolog", "x_auth_token": "meu-precioso"}

user_id = "ID-do-profissional-aqui"

response = requests.post('https://homolog.api.lab-saude.com/doctors/medics/create-schedule/' + user_id, data=body, headers=headers)
```

```shell
curl --location --request POST 'https://homolog.api.lab-saude.com/doctors/medics/create-schedule/ID-do-profissional-aqui' \
-H 'x_tenant_id: homolog' \
-H 'x_auth_token: meu-precioso' \
-H 'Content-Type: application/json' \
--data-raw '{
	"schedules": [
		{
			"day": "Segunda-feira",
			"start_hour": "08:00",
			"end_hour": "12:00"
		},
		{
			"day": "Terça-feira",
			"start_hour": "08:00",
			"end_hour": "12:00"
		},
		{
			"day": "Segunda-feira",
			"start_hour": "13:00",
			"end_hour": "15:00"
		}
	]
}
'
```

```javascript
const axios = require("axios");

const body = {
  schedules: [
    {
      day: "Segunda-feira",
      start_hour: "08:00",
      end_hour: "12:00",
    },
    {
      day: "Terça-feira",
      start_hour: "08:00",
      end_hour: "12:00",
    },
    {
      day: "Segunda-feira",
      start_hour: "13:00",
      end_hour: "15:00",
    },
  ],
};

const userId = "ID-do-profissional-aqui";

const apiResponse = await axios.post(
  `https://homolog.api.lab-saude.com/doctors/medics/create-schedule/${userId}`,
  body,
  {
    headers: {
      x_tenant_id: "homolog",
      x_auth_token: "meu-precioso",
    },
  }
);
```

> O comando acima retorna um status 204 (no content). O que significa que não há corpo de retorno.

É importante observar que somente profissionais que atendem PARCIALMENTE à Lab Saúde, ou seja, que não tem disponibilidade total, podem criar um horário de atendimento.

Uma vez que o profissional foi cadastrado com disponibilidade parcial, é preciso cadastrar os seus horários de atendimento, para que o profissional consiga realizar consultas na plataforma da Lab nos horários cadastrados.

<aside class="notice">
Profissionais que tenham a propriedade <code>disponibility</code> cadastrada como <code>Total</code> terão, por padrão, todos os horários disponíveis para atendimento na Lab Saúde.
</aside>

## Listar horários disponíveis de um profissional

```python
import requests

user_id = "uuid-do-profissional-vem-aqui"

headers = {
  'x_tenant_id': 'homolog',
  'x_auth_token': 'meu-precioso'
}

response = requests.get("https://homolog.api.lab-saude.com/doctors/medics/medic-disponibility/" + user_id, headers=headers)
```

```shell
curl --location --request GET 'https://homolog.api.lab-saude.com/doctors/medics/medic-disponibility/uuid-do-profissional-vem-aqui' \
-H 'x_tenant_id: homolog' \
-H 'x_auth_token: meu-precioso'
```

```javascript
const axios = require("axios");

const userId = "uuid-do-profissional-vem-aqui";

const apiResponse = await axios.get(
  `https://homolog.api.lab-saude.com/doctors/medics/medic-disponibility/${userId}`,
  {
    headers: {
      x_tenant_id: "homolog",
      x_auth_token: "meu-precioso",
    },
  }
);
```

> O comando acima retorna um JSON com a seguinte estrutura:

```json
{
  "full_disponibility": false,
  "dates": [
    "2021-11-10T15:00:00.000Z",
    "Mon Nov 15 2021 10:30:00 GMT-0300 (-03)"
  ],
  "disponibilities": {
    "Segunda-feira": [
      {
        "id": "aaa8d33a-1e0e-4659-823e-0816b8be1578",
        "start": "08:00",
        "end": "12:00"
      },
      {
        "id": "bdaff5a4-48a7-48c1-900e-652f53190a0e",
        "start": "13:00",
        "end": "15:00"
      }
    ],
    "Terça-feira": [
      {
        "id": "935e6710-a6e7-405f-8c79-6e502485fad7",
        "start": "08:00",
        "end": "12:00"
      }
    ]
  }
}
```

Este endpoint retorna os horários disponíveis para marcação de consultas de um profissional em específico.

A API retorna um objeto contendo algumas informações, como se sua disponibilidade é total ou parcial, as datas futuras de consultas e os horários padrão disponíveis para marcação de consultas.

### Objeto de resposta descomplicado

| Campo              | Descrição                                                                                      | Tipo              |
| ------------------ | ---------------------------------------------------------------------------------------------- | ----------------- |
| full_disponibility | Flag que indica se o profissional tem disponibilidade total ou parcial                         | Booleano          |
| dates              | Datas das próximas consultas do profissional na Lab Saúde                                      | Array[Timestamps] |
| disponibilities    | Horários cadastrados como disponíveis no banco na hora da criação dos horários do profissional | Objeto            |

## Modificar disponibilidade de um profissional

```python
import requests

user_id = "ID-do-usuario-vem-aqui"

payload = {
	"disponibility": "Total"
}

headers = {
  'x_tenant_id': 'homolog',
  'x_auth_token': 'meu-precioso'
}

response = requests.put("https://homolog.api.lab-saude.com/doctors/medics/disponibility/" + user_id, data=payload, headers=headers)
```

```shell
curl --location --request PUT 'https://homolog.api.lab-saude.com/doctors/medics/disponibility/ID-do-usuario-vem-aqui' \
-H 'x_tenant_id: homolog' \
-H 'x_auth_token: meu-precioso' \
-H 'Content-Type: application/json' \
--data-raw '{
	"disponibility": "Total"
}'
```

```javascript
const axios = require("axios");

const userId = "ID-do-usuario-vem-aqui";

const body = {
  disponibility: "Total",
};

const apiResponse = await axios.put(
  `https://homolog.api.lab-saude.com/doctors/medics/disponibility/${userId}`,
  body,
  {
    headers: {
      x_tenant_id: "homolog",
      x_auth_token: "meu-precioso",
    },
  }
);
```

> O comando acima retorna um JSON com a seguinte estrutura:

```json
{
  "message": "Medic disponibility successfully changed"
}
```

Este endpoint serve exclusivamente para modificarmos o tipo de disponibilidade de um profissional.

Caso queira que ela seja `Total`, basta adicionar isso no corpo do request, caso contrário, você só precisa adicionar `Parcial`.

A API retorna uma mensagem indiciando o sucesso da ação.

<aside class="notice">
Caso você opte por trocar para disponibilidade <code>parcial</code>, lembre de <a href="#criar-horarios-de-atendimento-de-um-profissional">criar um horário de atendimento</a> na Lab Saúde.
</aside>

## Modificar profissional

```python
import requests

user_id = "ID-do-profissional-vem-aqui"

payload = {
    "clinic_id": "bolseiros-clinic"
}

headers = {
  'x_tenant_id': 'homolog',
  'x_auth_token': 'meu-precioso'
}

response = requests.put('https://homolog.api.lab-saude.com/doctors/medics/' + user_id, data=payload, headers=headers)
```

```shell
curl --location --request PUT 'https://homolog.api.lab-saude.com/doctors/medics/ID-do-profissional-vem-aqui' \
-H 'x_tenant_id: homolog' \
-H 'x_auth_token: meu-precioso' \
-H 'Content-Type: application/json' \
--data-raw '{
    "clinic_id": "bolseiros-clinic"
}'
```

```javascript
const axios = require("axios");

const body = {
  clinic_id: "bolseiros-clinic",
};

const userId = "ID-do-profissional-vem-aqui";

const apiResponse = await axios.put(
  `https://homolog.api.lab-saude.com/doctors/medics/${userId}`,
  body,
  {
    headers: {
      x_tenant_id: "homolog",
      x_auth_token: "meu-precioso",
    },
  }
);
```

> O comando acima retorna um JSON com a seguinte estrutura:

```json
{
  "id": "ID-do-profissional-vem-aqui",
  "name": "Bilbo Bolseiro",
  "email": "bilbo@lotr.com",
  "type": "CRP",
  "state": "23",
  "number": "123456",
  "accepts_presential_appointments": true,
  "accepts_online_appointments": true,
  "office_state": "Terra Média",
  "office_city": "Condado",
  "office_neighbor": "Vila dos Hobbits",
  "office_zip_code": "99.999-999",
  "office_street": "Avenida Hobbits de Souza",
  "office_number": "111",
  "image_link": null,
  "appointment_price": "50.00",
  "online_appointment_price": 70,
  "clinic_id": "bolseiros-clinic",
  "disponibility": "Total",
  "requires_more_info": false,
  "company": "lab-saude",
  "specialties": ["Viagens", "Anéis"]
}
```

Por meio deste endpoint é possível modificar qualquer informação padrão de um profissional.

Tudo o que você precisa fazer é enviar no corpo da requisição a informação a ser modificada.

A API retorna o objeto do profissional com suas informações atualizadas.
