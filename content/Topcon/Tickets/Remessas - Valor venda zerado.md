---
Ticket: 500181
Status: Em análise
Data de início: 2025-05-09
Data de fim:
---
# Problemas
# Problema 1
A programação foi criada sem traço, dessa forma não é possível calcular o valor da remessa devido a ausência de produto

## Solução 1
Para resolver para o cliente a questão das remessas sem valor, foi executado os scritps abaixo

```sql
WITH sub_query AS (
	SELECT rip.id_item_product , rlt.volume, rip.sales_value, rlt.volume * rip.sales_value AS total FROM reg_delivery_ticket_concretes rdtc
	INNER JOIN reg_delivery_tickets rdt ON rdt.id_delivery_ticket = rdtc.id_delivery_ticket_fk
	INNER JOIN reg_load_tickets rlt ON rlt.id_load_ticket = rdt.id_load_ticket_fk
	INNER JOIN reg_schedules rs ON rs.id_schedule = rdt.id_scheduling_fk
	INNER JOIN reg_items_products rip ON rip.id_contract_fk = rs.id_contract_fk AND rip.id_trace_fk = rs.id_product_weighted_fk
	WHERE rs.code IN (14636, 14642, 14690)
)
UPDATE reg_schedules rs 
SET id_item_product_trace_fk = sub.id_item_product
FROM sub_query sub
WHERE rs.code IN (14636, 14642, 14690);
	
	
WITH sub_query AS (
	SELECT rdtc.id_delivery_ticket_concrete, rlt.volume, rip.sales_value FROM reg_delivery_ticket_concretes rdtc
	INNER JOIN reg_delivery_tickets rdt ON rdt.id_delivery_ticket = rdtc.id_delivery_ticket_fk
	INNER JOIN reg_load_tickets rlt ON rlt.id_load_ticket = rdt.id_load_ticket_fk
	INNER JOIN reg_schedules rs ON rs.id_schedule = rdt.id_scheduling_fk
	INNER JOIN reg_items_products rip ON rip.id_contract_fk = rs.id_contract_fk AND rip.id_trace_fk = rs.id_product_weighted_fk
	WHERE rdtc.sale_price_per_volume IS NULL
)
UPDATE reg_delivery_ticket_concretes rdtc 
SET sale_price_per_volume = sub.sales_value
FROM sub_query sub
WHERE rdtc.sale_price_per_volume IS NULL
AND rdtc.id_delivery_ticket_concrete = sub.id_delivery_ticket_concrete;
```


## Solução 2




# Problema 2



## Solução 1



## Solução 2


# Casos de teste 

> [!tip] Dica
> Não esquecer de testes voltados para argamassa, mesmo quando não parecer fazer sentido

## ✅❌ Caso 1: nome do teste

- **Pré-condições:**
    - 
    - 
    - 
- **Passos do Teste:**
    1. 
    2. 
    3. 
- **Resultado Esperado:** 
- **Status:** ✅ PASSOU | ❌ FALHOU
- **Observações:** (A ser preenchido se necessário)

## ✅❌ Caso 2: nome do teste

- **Pré-condições:**
    - 
    - 
    - 
- **Passos do Teste:**
    1. 
    2. 
    3. 
- **Resultado Esperado:** 
- **Status:** ✅ PASSOU | ❌ FALHOU
- **Observações:** (A ser preenchido se necessário)

