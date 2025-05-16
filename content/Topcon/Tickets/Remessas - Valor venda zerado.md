---
Ticket: 500181
Status: Em análise
Data de início: 2025-05-09
Data de fim:
---
# Problemas
# Problema 1
A programação foi criada sem traço, dessa forma não é possível calcular o valor da remessa devido a ausência de produto.

Não foi possível replicar o problema e chegar na causa raiz, portanto a única solução foi corrigindo o valor
## Solução 1
Para resolver para o cliente a questão das remessas sem valor, foi executado os scripts abaixo,

```sql
--- Update item product of schedule
WITH sub_query AS (
	SELECT rs.id_schedule, rip.id_item_product , rlt.volume, rip.sales_value, rlt.volume * rip.sales_value AS total FROM reg_delivery_ticket_concretes rdtc
	INNER JOIN reg_delivery_tickets rdt ON rdt.id_delivery_ticket = rdtc.id_delivery_ticket_fk
	INNER JOIN reg_load_tickets rlt ON rlt.id_load_ticket = rdt.id_load_ticket_fk
	INNER JOIN reg_schedules rs ON rs.id_schedule = rdt.id_scheduling_fk
	INNER JOIN reg_items_products rip ON rip.id_contract_fk = rs.id_contract_fk AND rip.id_trace_fk = rs.id_product_weighted_fk
	WHERE rs.code IN (14636, 14642, 14690)
)
UPDATE reg_schedules rs 
SET id_item_product_trace_fk = sub.id_item_product
FROM sub_query sub
WHERE sub.id_schedule = rs.id_schedule;


--- Recalculate sale value of delivery ticket concrete
WITH sub_query AS (
	SELECT rdtc.id_delivery_ticket_concrete, rlt.volume, rip.sales_value FROM reg_delivery_ticket_concretes rdtc
	INNER JOIN reg_delivery_tickets rdt ON rdt.id_delivery_ticket = rdtc.id_delivery_ticket_fk
	INNER JOIN reg_load_tickets rlt ON rlt.id_load_ticket = rdt.id_load_ticket_fk
	INNER JOIN reg_schedules rs ON rs.id_schedule = rdt.id_scheduling_fk
	INNER JOIN reg_items_products rip ON rip.id_contract_fk = rs.id_contract_fk AND rip.id_trace_fk = rs.id_product_weighted_fk
	WHERE rs.code IN (14636, 14642, 14690)
)
UPDATE reg_delivery_ticket_concretes rdtc 
SET sale_price_per_volume = sub.sales_value
FROM sub_query sub
WHERE rdtc.id_delivery_ticket_concrete = sub.id_delivery_ticket_concrete;


--- Recalculate sale value of delivery tax document item
WITH sub_query AS (
	SELECT rdtdi.id_delivery_tax_document_item, rdtc.sale_price_per_volume AS unit_price, rdtc.sale_price_per_volume * rd.volume AS total_value, rdtd.document_number, rsfd.code, rm.service_related_percentage FROM reg_delivery_ticket_concretes rdtc
	INNER JOIN reg_delivery_tickets rdt ON rdt.id_delivery_ticket = rdtc.id_delivery_ticket_fk
	INNER JOIN reg_schedules rs ON rs.id_schedule = rdt.id_scheduling_fk
	INNER JOIN reg_delivery_tax_documents rdtd ON rdtd.id_scheduling_fk = rs.id_schedule
	INNER JOIN reg_delivery_tax_document_items rdtdi ON rdtdi.id_delivery_tax_document_fk = rdtd.id_delivery_tax_document
	INNER JOIN reg_subsidiaries_fiscal_data rsfd ON rsfd.id_subsidiary_fiscal_data = rdtd.id_subsidiary_fiscal_data_fk AND rsfd.id_tenant = '8d4b8d26-c6dd-4270-baff-840d11fc1c52'
	INNER JOIN reg_municipalities rm ON rm.id_municipality = rsfd.id_municipality_fk AND rm.id_tenant = '8d4b8d26-c6dd-4270-baff-840d11fc1c52'
	INNER JOIN reg_deliveries rd ON rd.id_delivery = rdt.id_delivery_fk
	WHERE rs.code IN (14636, 14642, 14690)
)
UPDATE reg_delivery_tax_document_items rdtdi
SET unit_price = sub.unit_price,
total_value = sub.total_value
FROM sub_query sub
WHERE rdtdi.id_delivery_tax_document_item = sub.id_delivery_tax_document_item;


--- Recalculate materials value of delivery tax document
WITH sub_query AS (
	SELECT rdtd.id_delivery_tax_document, rdtd.materials_value, rdtdi.total_value, rdtdi.unit_price, rsfd.code FROM reg_delivery_ticket_concretes rdtc
	INNER JOIN reg_delivery_tickets rdt ON rdt.id_delivery_ticket = rdtc.id_delivery_ticket_fk
	INNER JOIN reg_schedules rs ON rs.id_schedule = rdt.id_scheduling_fk
	INNER JOIN reg_delivery_tax_documents rdtd ON rdtd.id_scheduling_fk = rs.id_schedule
	INNER JOIN reg_delivery_tax_document_items rdtdi ON rdtdi.id_delivery_tax_document_fk = rdtd.id_delivery_tax_document
	INNER JOIN reg_subsidiaries_fiscal_data rsfd ON rsfd.id_subsidiary_fiscal_data = rdtd.id_subsidiary_fiscal_data_fk AND rsfd.id_tenant = '8d4b8d26-c6dd-4270-baff-840d11fc1c52'
	INNER JOIN reg_municipalities rm ON rm.id_municipality = rsfd.id_municipality_fk AND rm.id_tenant = '8d4b8d26-c6dd-4270-baff-840d11fc1c52'
	INNER JOIN reg_deliveries rd ON rd.id_delivery = rdt.id_delivery_fk
	WHERE rs.code IN (14636, 14642, 14690)
)
UPDATE reg_delivery_tax_documents rdtd
SET materials_value = sub.total_value
FROM sub_query sub
WHERE rdtd.id_delivery_tax_document = sub.id_delivery_tax_document;
```

# Casos de teste 

> [!warning] Atenção
> Não foram feitos testes devido a não codificação de correções

