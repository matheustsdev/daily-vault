---
Ticket: 486806
Status: Em análise
Data de início: 2025-03-06
Data de fim:
---

# Análises
1. 


# Soluções
# 1. Ajuste dos delivery tickets com mesma delivery
```sql
SELECT rd.id_delivery, rdt.code, rdt.created_at, rdtd.document_number, rdtd.cancellation_date , rdtd.created_at
FROM reg_delivery_tax_documents rdtd
INNER JOIN reg_schedules rs ON rs.id_schedule = rdtd.id_scheduling_fk
INNER JOIN reg_delivery_tickets rdt ON rdt.id_delivery_ticket = rdtd.id_delivery_ticket_fk
INNER JOIN reg_deliveries rd ON rd.id_delivery = rdt.id_delivery_fk
INNER JOIN reg_load_tickets rlt ON rlt.id_load_ticket = rdt.id_load_ticket_fk
WHERE rd.id_delivery IN (
	SELECT rdt_sub.id_delivery_fk
	FROM reg_delivery_tax_documents rdtd_sub
	INNER JOIN reg_delivery_tickets rdt_sub ON rdt_sub.id_delivery_ticket = rdtd_sub.id_delivery_ticket_fk
	GROUP BY rdt_sub.id_delivery_fk
	HAVING COUNT(DISTINCT rdtd_sub.document_number) > 1
)
AND rdt.deleted = false
ORDER BY rd.id_delivery, rdtd.document_number;
```

```sql
INSERT INTO topcon_dispatch.reg_deliveries
(id_tenant, id_scheduling_fk, volume, id_vehicle_fk, loading_forecast, on_the_way_forecast, at_construction_forecast, start_unloading_forecast, finish_unloading_forecast, returning_forecast, at_the_plant_forecast, status, deleted, created_at, updated_at, deleted_at, id_reservation_fk, "sequence", external_id, preparing_forecast)
SELECT
id_tenant,
id_scheduling_fk,
volume,
id_vehicle_fk,
loading_forecast,
on_the_way_forecast,
at_construction_forecast,
start_unloading_forecast,
finish_unloading_forecast,
returning_forecast,
at_the_plant_forecast,
status,
deleted,
created_at,
updated_at,
deleted_at,
id_reservation_fk,
"sequence",
id_delivery AS external_id, -- O external_id recebe o id_delivery original
preparing_forecast
FROM topcon_dispatch.reg_deliveries
WHERE id_delivery IN (

);
```

```sql
WITH RankedDocuments AS (
	SELECT
	rd.id_delivery,
	ROW_NUMBER() OVER (
		PARTITION BY rd.id_delivery
		ORDER BY rdt.created_at DESC
	) AS row_num,
	rdt.id_delivery_ticket AS id_delivery_ticket
	FROM reg_delivery_tax_documents rdtd
	INNER JOIN reg_schedules rs ON rs.id_schedule = rdtd.id_scheduling_fk
	INNER JOIN reg_delivery_tickets rdt ON rdt.id_delivery_ticket = rdtd.id_delivery_ticket_fk
	INNER JOIN reg_deliveries rd ON rd.id_delivery = rdt.id_delivery_fk
	INNER JOIN reg_load_tickets rlt ON rlt.id_load_ticket = rdt.id_load_ticket_fk
	WHERE rd.id_delivery IN (
	SELECT rdt_sub.id_delivery_fk
	FROM reg_delivery_tax_documents rdtd_sub
	INNER JOIN reg_delivery_tickets rdt_sub
	ON rdt_sub.id_delivery_ticket = rdtd_sub.id_delivery_ticket_fk
	GROUP BY rdt_sub.id_delivery_fk
	HAVING COUNT(DISTINCT rdtd_sub.document_number) > 1
)
)
SELECT id_delivery, row_num, id_delivery_ticket
FROM RankedDocuments
WHERE row_num = 1
ORDER BY id_delivery;
```

 ```sql
UPDATE reg_delivery_tickets rdt
SET id_delivery_fk = rd.id_delivery
FROM reg_deliveries rd
WHERE rd.external_id = CAST(rdt.id_delivery_fk AS varchar)
AND rdt.id_delivery_ticket IN (
	'35426187-e640-4109-8d0a-e802c56b3b86',
	'45670c07-dec1-4d2c-b344-17a6b00d17dd',
	'a3deb490-3ea0-4f95-a2a0-ef5088142f8b',
	'3fe349e8-5667-418e-819d-27e643e0f58f',
	'2cc93d6c-6c0f-42a6-a755-c894c30f8e09',
	'9d342ab7-bd6c-46d6-941f-a2f436f95789'
);
```


 ```sql
CREATE UNIQUE INDEX unique_delivery_ticket_per_delivery_idx
ON reg_delivery_tickets(id_tenant, id_delivery_fk)
WHERE deleted = FALSE;
```
# 2. Ajuste dos load tickets relational com mesmo delivery e load ticket