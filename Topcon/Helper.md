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
| 77cde178-5dc5-49d0-81e4-441adb7ddce8 | Concrelongo                   |
| 5ad98a18-cfb4-4acb-8132-c85d6b394229 | Pedreira Costa                |
| 749164e1-24db-4f74-80bc-59ba196c7c50 | Sultepa                       |
## OCP Apim keys
### Homologation

| Aplicação | Cliente    | Key                              |
| --------- | ---------- | -------------------------------- |
| TECH      | Concreserv | c6e465a0a75241d283cb3eb9341f669f |

# Respostas sustentação

## HF concluída com atualização agendada

```
Olá William Nicolas! 

Temos boas notícias sobre a sua solicitação! 
A correção foi concluída e será disponibilizada em ambiente de produção no final do dia 02/06 (segunda-feira).

Atenciosamente
Matheus Teixeira
```

# Scripts SQL
## Adição de usuário DISPATCH/IDENTITY

```sql
DO $$
DECLARE
    user_id UUID := 'User id'; -- Atualizar ID do usuário
    email TEXT := 'Email minusculo'; -- Atualizar Email do usuário
    tenant_id UUID := '8d4b8d26-c6dd-4270-baff-840d11fc1c52'; -- Atualizar ID do Tenant
    full_name TEXT := 'Nome'; -- Nome completo pode ser extraído do email se necessário
    is_admin BOOLEAN := true; -- Atualizar status de administrador
BEGIN

    -- Inserção na tabela topcon_identity.reg_users
    INSERT INTO topcon_identity.reg_users (
        id_user, id_tenant, deleted, created_at, updated_at, deleted_at,
        id_b2c, full_name, active, activated_at, last_activity_at, last_data_share_accept, 
        "admin", user_name, normalized_user_name, email, normalized_email, email_confirmed, 
        password_hash, security_stamp, concurrency_stamp, phone_number, phone_number_confirmed, 
        two_factor_enabled, lockout_end, lockout_enabled, access_failed_count
    )
    VALUES (
        user_id, tenant_id, false, NOW(), NOW(), NULL,
        user_id, full_name, true, NULL, NULL, NULL, 
        is_admin, email, UPPER(email), email, UPPER(email), true, 
        'AQAAAAEAACcQAAAAEBaQqocNWc/AIQvuaFdm3fDTZUVQ9p6lbl6gyaiYvpYFmeZ2johbZlz2i3ck8AC3EA==', 
        'UHFPAWBV5MWTDMYO2VVHOBEE2RZG56X3', 'dc10933f-bed1-47cc-9567-a14929a855a1', 
        NULL, false, false, NULL, false, 0
    );

    -- Inserção na tabela topcon_dispatch.reg_users
    INSERT INTO topcon_dispatch.reg_users (
        id_user, id_b2c, full_name, active, "admin", id_tenant, deleted, created_at, updated_at, deleted_at
    )
    VALUES (
        user_id, user_id, full_name, true, is_admin, tenant_id, false, NOW(), NULL, NULL
    );

END $$;
```



# Clientes com legado (integrator)

- maxmohr - hml: OK
- maxmohr - prd: OK  
- ciplan - hml: OK  
- ciplan - prd: integrando  
- Concreserv - hml: OK  
- Concreserv - prd: OK  
- Concrelongo - hml:  
- Concrelongo - prd: ok  
- Pedreira Costa - prd: ok