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