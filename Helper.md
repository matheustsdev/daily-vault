- Busca por fragmento de texto no elastic:
	```
	GET /topcon-integrator-desktop_dispatch-prd/_search
	{
	  "query": {
	    "bool": {
	      "must": [
	        {
	          "match_phrase": {
	            "message": "nfe_nNF = 23"
	          }
	        }
	      ]
	    }
	  }
	}
	```

# Dados rápidos
## Tenants

| Tenant Id                            | Cliente                       |
| ------------------------------------ | ----------------------------- |
| a8246ab8-7ebc-4792-969e-d56f5678066c | Topcon Suite - Infraestrutura |
| 8d4b8d26-c6dd-4270-baff-840d11fc1c52 | Maxmohr                       |
| 1e2e61d5-4957-4aa3-af29-5f476d3cdaee | Supertex                      |
| c6676f7c-07fa-4de3-99d7-8801e4742c30 | Supremo                       |
| b3b49022-a0b8-4ea5-84fd-69b1ec08e225 | Concreserv                    |
| 17473288-488b-4ac5-bd97-bebbdb63c237 | Siajsa                        |
| dfce6b13-b4b3-4ef2-b464-cf341e709948 | Concrecity                    |
| 8b0ab8a2-1925-4d2b-bf21-af70af0f8e9a | Supertex                      |
## OCP Apim keys
### Homologation

| Aplicação | Cliente    | Key                              |
| --------- | ---------- | -------------------------------- |
| TECH      | Concreserv | c6e465a0a75241d283cb3eb9341f669f |

# Checar usuários do IDENTITY no DISPATCH
