---
Ticket: 485867
Status: Em análise
Data de início: 2025-02-26
Data de fim:
---

# Análises
1. O custo unitário está sendo salvo errado na DeliveryTicketSupply. Com Geovane vimos que provavelmente é erro de lógica de cálculo
	![[Pasted image 20250226143657.png]]
	Dessa forma, como não o CostValue não era nulo, já usava ele direto.
2. Outros campos que usavam o CostValue no cálculo também estavam errados

# Soluções
1. Adição de parenteses em cada membro da divisão
2. Foi executado o SCRIPT para visualização e correção dos dados.
	Select:
	```sql
	SELECT
rdt.id_tenant,
rs.code,
rlti.quantity_real_total,
rs."name",
crfvsc.cost_value / crfvsc.mass AS unitCostValueRaw,
ROUND(COALESCE(crfvsc.cost_value / crfvsc.mass, 0), 3) AS unitCostValue,
ROUND(COALESCE((((rlti.quantity_theoretical_dry_total / lt.volume) * rd.volume) + (((rlti.quantity_theoretical_dry_total / lt.volume) * COALESCE(lt.reused_volume, 0)) / lt.volume)) * (crfvsc.cost_value / crfvsc.mass), 0.0), 2) AS theoreticalTotalCost,
ROUND(COALESCE(((rlti.quantity_real_total / lt.volume) * rd.volume) * (crfvsc.cost_value / crfvsc.mass), 0), 2) AS realTotalCost
FROM reg_load_tickets lt
LEFT JOIN reg_delivery_tickets rdt ON rdt.id_load_ticket_fk = lt.id_load_ticket
LEFT JOIN rel_loads_tickets_deliveries rltd ON rltd.id_load_ticket_fk = lt.id_load_ticket AND rltd.id_load_ticket_delivery = (
SELECT sub_rltd.id_load_ticket_delivery
FROM rel_loads_tickets_deliveries sub_rltd
WHERE sub_rltd.id_load_ticket_fk = lt.id_load_ticket
LIMIT 1
)
LEFT JOIN reg_deliveries rd ON rd.id_delivery = rltd.id_delivery_fk
LEFT JOIN reg_load_ticket_items rlti ON rlti.id_load_ticket_fk = lt.id_load_ticket
LEFT JOIN rel_concrete_recipe_family_version_supplies crfvs ON crfvs.id_concrete_recipe_family_version_fk = lt.id_concrete_recipe_family_version_fk AND crfvs.id_supply_fk = rlti.id_supply_fk
LEFT JOIN rel_concrete_recipe_family_version_supply_characteristics crfvsc ON crfvsc.id_concrete_recipe_family_version_supply_fk = crfvs.id_concrete_recipe_family_version_supply AND crfvsc.id_concrete_recipe_family_version_supply_characteristic = (
SELECT sub_crfvsc.id_concrete_recipe_family_version_supply_characteristic
FROM rel_concrete_recipe_family_version_supply_characteristics sub_crfvsc
WHERE sub_crfvsc.id_concrete_recipe_family_version_supply_fk = crfvs.id_concrete_recipe_family_version_supply
ORDER BY sub_crfvsc.deleted_at DESC
LIMIT 1
)
LEFT JOIN reg_delivery_ticket_supplies rdts ON rdts.id_supply_fk = rlti.id_supply_fk AND rdt.id_delivery_ticket = rdts.id_delivery_ticket_fk
INNER JOIN reg_supplies rs ON rs.id_supply = rdts.id_supply_fk
AND lt.id_tenant = '8d4b8d26-c6dd-4270-baff-840d11fc1c52';
	```

	Update:
	```sql
	UPDATE reg_delivery_ticket_supplies rdts
SET
unit_cost_value = ROUND(COALESCE(crfvsc.cost_value / crfvsc.mass, 0), 3),
theoretical_total_cost = ROUND(COALESCE((((rlti.quantity_theoretical_dry_total / lt.volume) * rd.volume) + (((rlti.quantity_theoretical_dry_total / lt.volume) * COALESCE(lt.reused_volume, 0)) / lt.volume)) * (crfvsc.cost_value / crfvsc.mass), 0), 2),
real_total_cost = ROUND(COALESCE(((rlti.quantity_real_total / lt.volume) * rd.volume) * (crfvsc.cost_value / crfvsc.mass), 0), 2)
FROM reg_load_tickets lt
LEFT JOIN reg_delivery_tickets rdt ON rdt.id_load_ticket_fk = lt.id_load_ticket
LEFT JOIN rel_loads_tickets_deliveries rltd
ON rltd.id_load_ticket_fk = lt.id_load_ticket
AND rltd.id_load_ticket_delivery = (
SELECT sub_rltd.id_load_ticket_delivery
FROM rel_loads_tickets_deliveries sub_rltd
WHERE sub_rltd.id_load_ticket_fk = lt.id_load_ticket
LIMIT 1
)
LEFT JOIN reg_deliveries rd ON rd.id_delivery = rltd.id_delivery_fk
LEFT JOIN reg_load_ticket_items rlti
ON rlti.id_load_ticket_fk = lt.id_load_ticket
LEFT JOIN rel_concrete_recipe_family_version_supplies crfvs
ON crfvs.id_concrete_recipe_family_version_fk = lt.id_concrete_recipe_family_version_fk
AND crfvs.id_supply_fk = rlti.id_supply_fk
LEFT JOIN rel_concrete_recipe_family_version_supply_characteristics crfvsc
ON crfvsc.id_concrete_recipe_family_version_supply_fk = crfvs.id_concrete_recipe_family_version_supply
AND crfvsc.id_concrete_recipe_family_version_supply_characteristic = (
SELECT sub_crfvsc.id_concrete_recipe_family_version_supply_characteristic
FROM rel_concrete_recipe_family_version_supply_characteristics sub_crfvsc
WHERE sub_crfvsc.id_concrete_recipe_family_version_supply_fk = crfvs.id_concrete_recipe_family_version_supply
ORDER BY sub_crfvsc.deleted_at DESC
LIMIT 1
)
WHERE rdts.id_delivery_ticket_fk = rdt.id_delivery_ticket
AND rdts.id_supply_fk = rlti.id_supply_fk
AND rdts.id_tenant = '8d4b8d26-c6dd-4270-baff-840d11fc1c52';
	```
3. Foi forçado a integração de todas as notas porém não foi corrigido já que o problema é no DISPATCH e não na integração
	Após testar em QA muitos problemas foram resolvidos e outros não. Depois de um bom tempo de análise Geovane e eu chegamos a conclusão que provavelmente é devido ao banco de QA do legado não estar sincronizado com o DISPATCH. 
3. Foi forçado integração em HML a fim de teste
4. No ticket Luiz solicitou que enviasse o registro das remessas não canceladas com notas canceladas
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
5. No dia 18/03, a partir das 19:40 foi forçado a integração da remessas e produção. Foram forçados 14152 registros em que 962 foram para a fila morta, resultando assertividade de 93,2%.
6. Foi feito uma atualização no DISPATCH e na integração atualizando o cálculo do material por m³ e o material total para ser o somatório dos valores dos insumos na con_item_nf/DeliveryTicketSupply
```sql
select filial, serie, num_nf, material_m3, material_total, qtde_m3_bt from con_nf where num_nf = 2428 and filial=1911 and serie=3;

select filial, ser, num_nf, SUM(preco_un), SUM(preco_un * qt) from con_item_nf where num_nf = 2428 and filial=1911 and ser=3 group by num_nf, filial, ser;
```
7. Será feito uma atualização dos valores das remessas no dispatch e no legado.
   
DISPATCH
   ```sql
WITH aggregated_values AS (
		SELECT
		rdt.id_delivery_ticket,
		SUM(
			CASE
			WHEN rdts.quantity_real_total IS NOT NULL AND rdts.quantity_real_total > 0
			THEN rdts.real_total_cost
			ELSE rdts.theoretical_total_cost
			END
		) / MAX(rd.volume) AS total_cost
	FROM reg_delivery_tickets rdt
	INNER JOIN reg_delivery_ticket_supplies rdts
	ON rdts.id_delivery_ticket_fk = rdt.id_delivery_ticket
	INNER JOIN reg_deliveries rd
	ON rd.id_delivery = rdt.id_delivery_fk
	GROUP BY rdt.id_delivery_ticket
)
UPDATE reg_delivery_ticket_concretes rdtc
SET material_price_per_volume = av.total_cost
FROM aggregated_values av
WHERE rdtc.id_delivery_ticket_fk = av.id_delivery_ticket;
SELECT rdt.code , SUM(rdts.theoretical_total_cost) AS soma_teorica_calculada, SUM(rdts.real_total_cost) AS soma_real_calculada, MAX(rdtc.material_price_per_volume) AS preco_por_volume, MAX(rd.volume) AS volume,MAX(rdtc.material_price_per_volume)*MAX(rd.volume) AS valor_total_atual FROM reg_delivery_tickets rdt
INNER JOIN reg_delivery_ticket_supplies rdts ON rdts.id_delivery_ticket_fk = rdt.id_delivery_ticket
INNER JOIN reg_delivery_ticket_concretes rdtc ON rdtc.id_delivery_ticket_fk = rdt.id_delivery_ticket
INNER JOIN reg_deliveries rd ON rd.id_delivery = rdt.id_delivery_fk
GROUP BY rdt.code;
```

LEGADO
```sql
SELECT cin.filial, cin.ser, cin.num_nf, SUM(cin.vl_tot) / MAX(cn.qtde_m3_bt) as total_un_items, SUM(cin.vl_tot) as total_items, MAX(cn.qtde_m3_bt) as volume, MAX(cn.material_m3) as valor_un_atual, MAX(cn.material_total) as valor_total_atual FROM con_nf cn
INNER JOIN con_item_nf cin ON cn.filial = cin.filial
	AND cn.serie = cin.ser
    AND cn.num_nf = cin.num_nf
GROUP BY cin.filial, cin.ser, cin.num_nf;

UPDATE con_nf cn
JOIN (
	SELECT cin.filial, cin.ser, cin.num_nf, SUM(cin.vl_tot) / SUM(cn.qtde_m3_bt) as total_un_items, SUM(cin.vl_tot) as total_items, MAX(cn.qtde_m3_bt) as volume FROM con_nf cn
	INNER JOIN con_item_nf cin ON cn.filial = cin.filial
	AND cn.serie = cin.ser
    AND cn.num_nf = cin.num_nf
	GROUP BY cin.filial, cin.ser, cin.num_nf
) av ON cn.filial = av.filial
	AND cn.serie = av.ser
    AND cn.num_nf = av.num_nf
SET material_m3 = av.total_un_items,
material_total = av.total_items;
```
