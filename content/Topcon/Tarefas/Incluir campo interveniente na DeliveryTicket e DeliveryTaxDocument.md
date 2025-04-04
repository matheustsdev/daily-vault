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
- [ ] Implementar integração do interveniente baseado no novo campo ClientId (4h)
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

