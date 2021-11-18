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
