# Profissionais

Os profissionais da saúde exercem um papel fundamental dentro do ecossistema da Lab Saúde.

Podemos realizar inúmeras operações com eles dentro da nossa API.

A seguir, veremos as mais recorrentes e importantes.

## Criar um profissional

```python
import requests

body = {
  "name": "Bilbo Bolseiro",
  "email": "bilbo@lotr.com",
  "gender": "Masculino",
  "password": "bolseiro@123",
  "passwordConfirmation": "bolseiro@123",
  "specialties":  ["Viagens", "Anéis"],
  "bio": "Renomado explorador da Terra-Média, sempre disposto a ajudar aqueles que precisam",
  "experience": "Viajou com um grupo de anões e um feiticeiro para uma montanha mística cheia de ouro",
  "formation": "Bachalerado em Psicologia - Universidade Federal da Terra Média",
  "council_type": "CRP",
  "council_state": "23",
  "council_code": "123456",
  "office_city": "Condado",
  "office_complement": "Casa de madeira",
  "office_neighbor": "Vila dos Hobbits",
  "office_number": "111",
  "office_state": "Terra Média",
  "office_street": "Avenida Hobbits de Souza",
  "office_zip_code": "99.999-999",
  "primary_phone": "(99) 9.9999-9999",
  "secondary_phone": "(99) 9.9999-9999",
  "disponibility": "Total",
  "appointment_price": "50.00",
  "online_appointment_price": "70.00",
  "accepts_presential_appointments": True,
  "accepts_online_appointments": True,
  "clinic_id": "clinica-dos-hobbits"
}

headers = {"x_tenant_id": "homolog"}

response = requests.post('https://homolog.api.lab-saude.com/doctors/medics', data=body, headers=headers)
```

```shell
curl --location --request POST 'https://homolog.api.lab-saude.com/doctors/medics' \
-H 'x_tenant_id: homolog' \
-H 'Content-Type: application/json' \
--data-raw '{
  "name": "Bilbo Bolseiro",
  "email": "bilbo@lotr.com",
  "gender": "Masculino",
  "password": "bolseiro@123",
  "passwordConfirmation": "bolseiro@123",
  "specialties":  ["Viagens", "Anéis"],
  "bio": "Renomado explorador da Terra-Média, sempre disposto a ajudar aqueles que precisam",
  "experience": "Viajou com um grupo de anões e um feiticeiro para uma montanha mística cheia de ouro",
  "formation": "Bachalerado em Psicologia - Universidade Federal da Terra Média",
  "council_type": "CRP",
  "council_state": "23",
  "council_code": "123456",
  "office_city": "Condado",
  "office_complement": "Casa de madeira",
  "office_neighbor": "Vila dos Hobbits",
  "office_number": "111",
  "office_state": "Terra Média",
  "office_street": "Avenida Hobbits de Souza",
  "office_zip_code": "99.999-999",
  "primary_phone": "(99) 9.9999-9999",
  "secondary_phone": "(99) 9.9999-9999",
  "disponibility": "Total",
  "appointment_price": "50.00",
  "online_appointment_price": "70.00",
  "accepts_presential_appointments": true,
  "accepts_online_appointments": true,
  "clinic_id": "clinica-dos-hobbits"
}'
```

```javascript
const axios = require("axios");

const body = {
  name: "Bilbo Bolseiro",
  email: "bilbo@lotr.com",
  gender: "Masculino",
  password: "bolseiro@123",
  passwordConfirmation: "bolseiro@123",
  specialties: ["Viagens", "Anéis"],
  bio: "Renomado explorador da Terra-Média, sempre disposto a ajudar aqueles que precisam",
  experience:
    "Viajou com um grupo de anões e um feiticeiro para uma montanha mística cheia de ouro",
  formation: "Bachalerado em Psicologia - Universidade Federal da Terra Média",
  council_type: "CRP",
  council_state: "23",
  council_code: "123456",
  office_city: "Condado",
  office_complement: "Casa de madeira",
  office_neighbor: "Vila dos Hobbits",
  office_number: "111",
  office_state: "Terra Média",
  office_street: "Avenida Hobbits de Souza",
  office_zip_code: "99.999-999",
  primary_phone: "(99) 9.9999-9999",
  secondary_phone: "(99) 9.9999-9999",
  disponibility: "Total",
  appointment_price: "50.00",
  online_appointment_price: "70.00",
  accepts_presential_appointments: true,
  accepts_online_appointments: true,
  clinic_id: "clinica-dos-hobbits",
};

const apiResponse = await axios.post(
  "https://homolog.api.lab-saude.com/doctors/medics",
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
  "id": "ddff880b-9147-4294-940e-52ea60434a18",
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
  "appointment_price": "50.00",
  "online_appointment_price": "70.00",
  "clinic_id": "clinica-dos-hobbits",
  "disponibility": "Total",
  "specialties": ["Viagens", "Anéis"]
}
```

Para realizar qualquer interação nas rotas dos profissionais é preciso, primeiro, criar um profissional.

Nesse endpoint, criamos uma conta para um novo profissional, passando informações básicas de nome, email, endereço da clínica onde atua, se recebe consultas presenciais e/ou telemedicina, suas especialidades, suas informações complementares como formação, experiência, etc. Esses dados seguem o padrão do que é pedido na tela de cadastro do [sistema para profissionais da saúde](https://profissionais.lab-saude.com).

### Objeto de resposta descomplicado

| Campo                           | Descrição                                                                                          | Tipo           |
| ------------------------------- | -------------------------------------------------------------------------------------------------- | -------------- |
| id                              | ID do profissional no banco da Lab                                                                 | UUID           |
| name                            | Nome do profissional                                                                               | String         |
| email                           | E-mail do profissional                                                                             | String         |
| type                            | Tipo do conselho do profissional (CRM, CRP, CRO, etc)                                              | String         |
| state                           | Estado do conselho do profissional de acordo com o padrão do conselho                              | String         |
| number                          | Número do registro do profissional no respectivo conselho                                          | String         |
| accepts_presential_appointments | Booleano que indica se o profissional aceita atendimentos presenciais                              | Booleano       |
| accepts_online_appointments     | Booleano que indica se o profissional aceita atendimentos por telemedicina                         | Booleano       |
| office_state                    | Estado do local de atendimento do profissional                                                     | String         |
| office_city                     | Cidade do local de atendimento do profissional                                                     | String         |
| office_neighbor                 | Bairro do local de atendimento do profissional                                                     | String         |
| office_zip_code                 | CEP do local de atendimento do profissional                                                        | String         |
| office_street                   | Logradouro do local de atendimento do profissional                                                 | String         |
| office_number                   | Complemento do local de atendimento do profissional                                                | String         |
| appointment_price               | Preço cobrado pelo atendimento presencial                                                          | String         |
| clinic_id                       | ID da clínica a qual o profissional foi vinculado na hora do cadastro (se houver)                  | String         |
| disponibility                   | Tipo de disponibilidade do profissional para consultas na Lab Saúde, pode ser "Total" ou "Parcial" | String         |
| specialties                     | Lista contendo as especialidades do profissional                                                   | Array[Strings] |

## Criar PROTO profissionais

```python
import requests

body = {
	"medics": [
		{
			"name": "Frodo Bolseiro",
			"crm": "123456789",
			"office_state": "Terra Média",
            "office_city": "Condado",
            "office_neighbor": "Vila dos Hobbits",
            "office_zip_code": "99.999-998",
            "office_street": "Avenida Hobbits de Souza",
            "office_number": "112",
            "council_type": "CRP",
            "council_state": "23",
            "council_code": "123457",
			"specialties": ["Anéis", "Podologia"]
		},
		{
			"name": "Gandalf, The White",
			"crm": "147258369",
			"office_state": "Terra Média",
            "office_city": "Algum Lugar",
            "office_neighbor": "Vila dos Magos",
            "office_zip_code": "99.999-888",
            "office_street": "Avenida Magos de Souza",
            "office_number": "n/a",
            "council_type": "CRP",
            "council_state": "23",
            "council_code": "123458",
			"specialties": ["Feiticaria"]
		}
	]
}


headers = {"x_tenant_id": "homolog"}

company_id = "terra-media"

response = requests.post('https://homolog.api.lab-saude.com/doctors/medics/proto/' + company_id, data=body, headers=headers)
```

```shell
curl --location --request POST 'https://homolog.api.lab-saude.com/doctors/medics/proto/terra-media' \
-H 'x_tenant_id: homolog' \
-H 'Content-Type: application/json' \
--data-raw '{
	"medics": [
		{
			"name": "Frodo Bolseiro",
			"crm": "123456789",
			"office_state": "Terra Média",
            "office_city": "Condado",
            "office_neighbor": "Vila dos Hobbits",
            "office_zip_code": "99.999-998",
            "office_street": "Avenida Hobbits de Souza",
            "office_number": "112",
            "council_type": "CRP",
            "council_state": "23",
            "council_code": "123457",
			"specialties": ["Anéis", "Podologia"]
		},
		{
			"name": "Gandalf, The White",
			"crm": "147258369",
			"office_state": "Terra Média",
            "office_city": "Algum Lugar",
            "office_neighbor": "Vila dos Magos",
            "office_zip_code": "99.999-888",
            "office_street": "Avenida Magos de Souza",
            "office_number": "n/a",
            "council_type": "CRP",
            "council_state": "23",
            "council_code": "123458",
			"specialties": ["Feiticaria"]
		}
	]
}
'
```

```javascript
const axios = require("axios");

const body = {
  medics: [
    {
      name: "Frodo Bolseiro",
      crm: "123456789",
      office_state: "Terra Média",
      office_city: "Condado",
      office_neighbor: "Vila dos Hobbits",
      office_zip_code: "99.999-998",
      office_street: "Avenida Hobbits de Souza",
      office_number: "112",
      council_type: "CRP",
      council_state: "23",
      council_code: "123457",
      specialties: ["Anéis", "Podologia"],
    },
    {
      name: "Gandalf, The White",
      crm: "147258369",
      office_state: "Terra Média",
      office_city: "Algum Lugar",
      office_neighbor: "Vila dos Magos",
      office_zip_code: "99.999-888",
      office_street: "Avenida Magos de Souza",
      office_number: "n/a",
      council_type: "CRP",
      council_state: "23",
      council_code: "123458",
      specialties: ["Feiticaria"],
    },
  ],
};
const companyId = "terra-media";

const apiResponse = await axios.post(
  `https://homolog.api.lab-saude.com/doctors/medics/proto/${companyId}`,
  body,
  {
    headers: {
      x_tenant_id: "homolog",
    },
  }
);
```

> O comando acima retorna um status 204 (no content). O que significa que não há corpo de retorno.

Assim como na seção de pacientes, às vezes é preciso criar uma grande quantidade de profissionais de uma vez.

Com isso em mente, esse endpoint recebe algumas informações básicas para a criação de um profissional e os cadastra no banco de dados.

Estes profissionais com o cadastro incompleto são chamados de PROTO-profissionais. Mais tarde, eles poderão completar seu cadastro dentro do [sistema profissional](https://profissionais.lab-saude.com), ou pela própria API.

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
