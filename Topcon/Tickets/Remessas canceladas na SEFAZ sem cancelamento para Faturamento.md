---
Ticket: 485633
Status: Concluído
Data de início: 2025-02-25
Data de fim: 2025-02-28
---

# Análises
1. Notas foram canceladas sem cancelar remessa.
2. Ao tentar cancelar remessa pela expedição está dando erro


# Soluções
1. A solução total está incluida na tarefa do Wil de cancelar as duas simultaneamente. O ticket foi finalizado apenas informando todas as notas nesse estado e as remessas.

```sql
SELECT rcbp.name AS central, rdtd.document_number AS numero_da_nota, rdt.code AS numero_da_remessa, rdtc.status AS status_da_remessa, rd.status AS status_da_entrega, rdt.already_billed AS ja_faturado FROM reg_delivery_tickets rdt
INNER JOIN reg_delivery_tax_documents rdtd ON rdtd.id_delivery_ticket_fk = rdt.id_delivery_ticket
INNER JOIN reg_delivery_tax_document_authorizations rdtda ON rdtda.id_delivery_tax_document_fk = rdtd.id_delivery_tax_document
INNER JOIN reg_delivery_ticket_concretes rdtc ON rdtc.id_delivery_ticket_fk = rdt.id_delivery_ticket
INNER JOIN reg_deliveries rd ON rd.id_delivery = rdt.id_delivery_fk
INNER JOIN reg_schedules rs ON rs.id_schedule = rdt.id_scheduling_fk
INNER JOIN reg_concrete_batching_plants rcbp ON rcbp.id_concrete_batching_plant = rs.id_concrete_batching_plant_fk
WHERE rdtd.cancelled = TRUE AND rdt.id_tenant = '8d4b8d26-c6dd-4270-baff-840d11fc1c52' AND rdtc.status <> 'CANCELED';
```
