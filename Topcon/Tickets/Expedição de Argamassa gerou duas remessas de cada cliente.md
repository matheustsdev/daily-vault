---
Ticket: 486806
Status: Concluído
Data de início: 2025-03-06
Data de fim: 2025-03-13
---
# Análises
1. 


# Soluções
# 1. Ajuste dos delivery tickets com mesma delivery
A solução definitiva foi aplicado adicionando uma constraint tornando único o id da delivery na tabela de delivery tickets, já validando deleção e o tenant.

Porém para adicionar essa tratativa é necessário ajustar os casos em que isso ocorre. Para isso foram criadas cópias das entregas que possuem remessa duplicada adicionando o id da entrega original como external_id da cópia. Em seguida a remessa duplicada é vinculada à entrega que foi copiada, dessa forma garantindo a não ocorrência da duplicidade de delivery id na tabela de delivery ticket.

Caso a entrega possua mais de 2 delivery tickets vinculados, será necessário corrigir manualmente as demais entregas criando duplicatas da entrega original com outro id e referenciando de alguma forma o id_delivery original (não é possível usar o mesmo id_delivery pois o external_id é único, então sugiro utilizar algo como "2 - {deliveryId}", "3 - {deliveryId}"), já que os scripts criados só conseguem lidar com os casos de duplicidade.

Segue o processo utilizado.

1. Inicialmente é utilizado o script abaixo para verificar quais entregas estão duplicadas além de já verificar se existem casos em que o deliveryId se repete mais de 2 vezes.
```sql
WITH RankedDocuments AS (
	SELECT
	rd.id_delivery,
	ROW_NUMBER() OVER (
		PARTITION BY rd.id_delivery
		ORDER BY rdt.created_at DESC
	) AS row_num,
	rdt.id_delivery_ticket AS id_delivery_ticket
	FROM reg_delivery_tickets rdt
	LEFT JOIN reg_delivery_tax_documents rdtd ON rdt.id_delivery_ticket = rdtd.id_delivery_ticket_fk
	INNER JOIN reg_schedules rs ON rs.id_schedule = rdt.id_scheduling_fk
	INNER JOIN reg_deliveries rd ON rd.id_delivery = rdt.id_delivery_fk
	INNER JOIN reg_load_tickets rlt ON rlt.id_load_ticket = rdt.id_load_ticket_fk
	WHERE rd.id_delivery IN (
		SELECT rdt_sub.id_delivery_fk
		FROM reg_delivery_tickets rdt_sub
		LEFT JOIN reg_delivery_tax_documents rdtd_sub
		ON rdt_sub.id_delivery_ticket = rdtd_sub.id_delivery_ticket_fk
		GROUP BY rdt_sub.id_delivery_fk
		HAVING COUNT(DISTINCT rdt_sub.code) > 1
	)
)
SELECT id_delivery, row_num, id_delivery_ticket
FROM RankedDocuments
WHERE row_num = 1
ORDER BY id_delivery;
```

2. Em seguida adicionamos "WHERE row_num = 1" para retornar apenas os deliveryTicketIds criadas inicialmente. Com o retorno do script, todos os deliveryIds devem ser copiadas e adicionados no WHERE do script de INSERT INTO reg_deliveries
   ```sql
WITH RankedDocuments AS (
	SELECT
	rd.id_delivery,
	ROW_NUMBER() OVER (
		PARTITION BY rd.id_delivery
		ORDER BY rdt.created_at DESC
	) AS row_num,
	rdt.id_delivery_ticket AS id_delivery_ticket
	FROM reg_delivery_tickets rdt
	LEFT JOIN reg_delivery_tax_documents rdtd ON rdt.id_delivery_ticket = rdtd.id_delivery_ticket_fk
	INNER JOIN reg_schedules rs ON rs.id_schedule = rdt.id_scheduling_fk
	INNER JOIN reg_deliveries rd ON rd.id_delivery = rdt.id_delivery_fk
	INNER JOIN reg_load_tickets rlt ON rlt.id_load_ticket = rdt.id_load_ticket_fk
	WHERE rd.id_delivery IN (
		SELECT rdt_sub.id_delivery_fk
		FROM reg_delivery_tickets rdt_sub
		LEFT JOIN reg_delivery_tax_documents rdtd_sub
		ON rdt_sub.id_delivery_ticket = rdtd_sub.id_delivery_ticket_fk
		GROUP BY rdt_sub.id_delivery_fk
		HAVING COUNT(DISTINCT rdt_sub.code) > 1
	)
)
SELECT id_delivery, row_num, id_delivery_ticket
FROM RankedDocuments
WHERE row_num = 1
ORDER BY id_delivery;
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

Em seguida deve ser utilizado o script inicial filtrando o row_num = 1, como anteriormente, e dessa vez copiar todos os delivery ticket id e inserir no UPDATE abaixo

 ```sql
UPDATE reg_delivery_tickets rdt
SET id_delivery_fk = rd.id_delivery
FROM reg_deliveries rd
WHERE rd.external_id = CAST(rdt.id_delivery_fk AS varchar)
AND rdt.id_delivery_ticket IN (
	
);
```

Em seguida o index pode ser criado sem problemas

 ```sql
CREATE UNIQUE INDEX unique_delivery_ticket_per_delivery_idx
ON reg_delivery_tickets(id_tenant, id_delivery_fk)
WHERE deleted = FALSE;
```

Em produção foi necessário alterar manualmente outros registros que possuíam mais de 2 delivery tickets para a mesma delivery. Para não perder a referência do ID original, foi adicionado o prefixo "REFER " nos external ids resultando em "REFER id_delivery_original"

Segue script que pode verificar os dados alterados em produção
```sql
SELECT rdt.code AS codigo_da_remessa, rdt.created_at AS data_da_remessa, rsfd."name" AS central, rdtd.document_number AS numero_da_nota, rdtd.issue_date AS data_da_nota, rdtd.cancelled nota_esta_cancelada FROM reg_deliveries rd
LEFT JOIN reg_schedules rs ON rs.id_schedule = rd.id_scheduling_fk
INNER JOIN reg_delivery_tickets rdt ON CAST(rdt.id_delivery_fk AS varchar) = rd.external_id OR REGEXP_REPLACE(rd.external_id, '^REFER ', '', 'g') = CAST(rdt.id_delivery_fk AS varchar)
LEFT JOIN reg_delivery_tax_documents rdtd ON rdtd.id_delivery_ticket_fk = rdt.id_delivery_ticket
LEFT JOIN reg_subsidiaries_fiscal_data rsfd ON rsfd.id_subsidiary_fiscal_data = rdtd.id_subsidiary_fiscal_data_fk
WHERE rd.external_id IS NOT NULL;
```

