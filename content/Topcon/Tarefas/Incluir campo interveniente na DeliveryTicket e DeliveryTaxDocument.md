---
Tarefa: TDP-1675
Desenvolvedor: Eu
Método de code review: 
Responsabilidade:
  - Desenvolver
Data de início: 2025-04-02
Data de fim:
---
# Quebra técnica (24h/37h)

- [/] Criar parâmetro de "origem do interveniente" e seu log (8h)
- [x] Adicionar campo de "ClientId" na remessa e nota (4h)
- [x] Criar script para atualizar todos os dados anteriores das remessas e notas com o "ClientId" (4h)
- [ ] Implementar integração do interveniente baseado no novo campo ClientId (6h)
- [ ] Atualizar envio de nota fiscal para utilizar o novo campo ClientId (4h)

## Criar parâmetro de "origem do interveniente" e seu log (8h)

- [x] Adicionar o campo IntervenerSource na entidade DeliveryTaxDocumentParams
- [x] Adicionar log na alteração da entidade DeliveryTaxDocumentParams
- [x] Adicionar campo para alterar o dado no Frontend

# Adicionar campo de "ClientId" na remessa e nota (4h)

Sem muitos problemas

# Criar script para atualizar todos os dados anteriores das remessas e notas com o "ClientId" (4h)

```sql
UPDATE reg_delivery_tickets rdt
SET id_client_fk = rc.id_client_fk
FROM reg_schedules rs
INNER JOIN reg_contracts rc ON rs.id_contract_fk = rc.id_contract
WHERE rdt.id_scheduling_fk = rs.id_schedule;

UPDATE reg_delivery_tax_documents rdtd
SET id_client_fk = rsfd.id_client_fk
FROM reg_subsidiaries_fiscal_data rsfd 
WHERE rsfd.id_subsidiary_fiscal_data = rdtd.id_subsidiary_fiscal_data_fk;
```

# Implementar integração do interveniente baseado no novo campo ClientId (6h)

Devido a opção de utilizar o remetente com endereço da obra foi decidido criar uma nova tabela que irá armazenar os dados de endereço da cada nota, dessa forma a validação do parâmetro só precisa ser feita no DISPATCH e refatorado na FUNCTION para que utilize os dados dessa tabela para gerar a nota.

Foi criado também um script para preencher essa tabela com os dados das notas já emitidas.
```sql
INSERT INTO reg_delivery_tax_document_address 
(
    id_tenant,
    id_delivery_tax_document_fk,
    zip_code,
    address,
    "number",
    complement,
    district,
    id_municipality_fk,
    phone,
    created_at
)
SELECT 
    rdtd.id_tenant ,
    rdtd.id_delivery_tax_document,
    rsfd.zip_code,
    rsfd.address, 
    rsfd.number,
    rsfd.complement,
    rsfd.district,
    rsfd.id_municipality_fk,
    CONCAT(rsfd.ddd, rsfd.phone_number),
    rdtd.created_at
FROM 
    reg_delivery_tax_documents rdtd
INNER JOIN reg_subsidiaries_fiscal_data rsfd ON rdtd.id_subsidiary_fiscal_data_fk = rsfd.id_subsidiary_fiscal_data;
```

No caso do contato:
	- Caso o parâmetro seja "DESTINATÁRIO" deve ser usado o contato registrado no cliente, utilizando o primeiro contato do tipo "TELEFONE". Caso não exista nenhum deve ser gerado um erro erro tratado que exibirá o motivo: "Erro ao emitir nota fiscal. Telefone do cliente não encontrado"
	- Caso o parâmetro seja diferente de "DESTINATÁRIO" deve ser utilizado o contato da subsidiaria (filial)

