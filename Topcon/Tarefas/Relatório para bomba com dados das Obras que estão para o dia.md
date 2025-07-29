---
Tarefa: TDP-1964
Desenvolvedor: Eu
Método de code review: 
Responsabilidade:
  - Desenvolver
Data de início: 2025-07-06
Data de fim:
---
# Quebra técnica (12h/12h)

- [x] Criar SQL programação (3h)
- [x] Criar SQL expedição (3h)
- [ ] Ajustar modelo do relatório (12h)
- [ ] Ajustar modelo do relatório expedição (1h)

- [ ] Tem que trazer a imagem do banco no lado esquerdo do titulo
- [x] Aumentar o espaço para Inicio e Fim, talvez até mudando o Volume para M³
- [x] Trocar dados de saida/chegada central/obra para uma tabela
- [x] Bombistas e auxiliares em campo unico para cada, separado por /
- [x] Mover a data de emissão para o topo superior direito do TITULO, não do cabeçalho
- [ ] Adicionar rodapé padrão Topcon
- [x] Remover linhas da parte depois da tabela de dados
- [x] Na parte de informações da bomba (bomba e placa) trocar por uma informação só -> bomba: code - placa
- [x] Na distancia de tubulação corrigir formatação para inteiro
- [x] No Volume deixar uma casa decimal


![[Pasted image 20250717173026.png]]

# SQL

## Bombeamento de programação

```sql
SELECT DISTINCT 
reg_schedules.code AS schedule_code,
CASE WHEN reg_schedules.pump_date IS NULL THEN ''
ELSE TO_CHAR(reg_schedules.start_date, 'DD/MM/YYYY') END AS start_date,
CASE WHEN reg_schedules.pump_date IS NULL THEN ''
ELSE TO_CHAR(reg_schedules.pump_date, 'HH24:MI:SS') END AS pump_hour,
reg_clients.company_name, reg_constructions.name,
reg_constructions.address,
reg_constructions.complement, reg_constructions.district,
reg_constructions.zip_code,
reg_municipalities.name AS municipalities_name, reg_municipalities.state AS municipalities_state,
reg_sellers.full_name AS saller_name,
CASE WHEN reg_schedules.pipe_distance IS NULL THEN '0.0'
ELSE reg_schedules.pipe_distance::TEXT END AS pipe_distance,
reg_deliveries.volume AS volume_pumped,
reg_schedules.volume,
reg_concrete_recipes.numeration || ' - ' || reg_concrete_recipes.description AS product,
reg_concrete_batching_plants.name AS central_name,
reg_concrete_batching_plants.name AS central,
reg_vehicles.code AS pump_code,
reg_vehicles.license_plate AS pump_license_plate,
reg_employees."name" AS driver_name
FROM reg_schedules reg_schedules 
INNER JOIN reg_concrete_recipes reg_concrete_recipes
ON reg_concrete_recipes.id_concrete_recipe = reg_schedules.id_product_fk
AND reg_concrete_recipes.id_tenant = reg_schedules.id_tenant
INNER JOIN reg_concrete_batching_plants reg_concrete_batching_plants
ON reg_concrete_batching_plants.id_concrete_batching_plant = reg_schedules.id_concrete_batching_plant_fk
AND reg_concrete_batching_plants.id_tenant = reg_schedules.id_tenant
INNER JOIN reg_deliveries reg_deliveries
ON reg_deliveries.id_scheduling_fk  = reg_schedules.id_schedule 
AND reg_deliveries.id_tenant = reg_schedules.id_tenant
INNER JOIN reg_contracts reg_contracts
ON reg_contracts.id_contract = reg_schedules.id_contract_fk
AND reg_contracts.id_tenant = reg_schedules.id_tenant
INNER JOIN reg_clients reg_clients
ON reg_clients.id_client = reg_contracts.id_client_fk
AND reg_clients.id_tenant = reg_contracts.id_tenant
LEFT JOIN reg_sellers reg_sellers
ON reg_sellers.id_seller = reg_clients.id_seller_fk
AND reg_sellers.id_tenant = reg_clients.id_tenant
INNER JOIN reg_constructions reg_constructions
ON reg_constructions.id_construction = reg_contracts.id_construction_fk
AND reg_constructions.id_tenant = reg_contracts.id_tenant
INNER JOIN reg_municipalities reg_municipalities
ON reg_municipalities.id_municipality = reg_constructions.id_municipality_fk 
AND reg_municipalities.id_tenant = reg_constructions.id_tenant
LEFT JOIN reg_concrete_batching_plant_regionals reg_concrete_batching_plant_regionals
ON reg_concrete_batching_plant_regionals.id_concrete_batching_plant_regional = reg_concrete_batching_plants.id_concrete_batching_plant_regional_fk
AND reg_concrete_batching_plant_regionals.id_tenant = reg_concrete_batching_plants.id_tenant
LEFT JOIN reg_concrete_batching_plant_groups reg_concrete_batching_plant_groups
ON reg_concrete_batching_plant_groups.id_concrete_batching_plant_group = reg_concrete_batching_plant_regionals.id_concrete_batching_plant_group_fk
AND reg_concrete_batching_plant_groups.id_tenant = reg_concrete_batching_plant_regionals.id_tenant
INNER JOIN reg_vehicles reg_vehicles 
	ON reg_schedules.id_pump_fk = reg_vehicles.id_vehicle
	AND reg_vehicles.id_tenant = reg_schedules.id_tenant
LEFT JOIN reg_allocations reg_allocations
	ON reg_allocations.id_vehicle_fk = reg_vehicles.id_vehicle
	AND reg_allocations.id_tenant = reg_vehicles.id_tenant
	AND reg_allocations.deleted = FALSE
	AND reg_allocations.available = TRUE
	AND reg_allocations.start_date >= '2025-06-01 00:00:00.000'
	AND (reg_allocations.finish_date >= '2025-06-30 00:00:00.000' OR reg_allocations.finish_date IS NULL)
LEFT JOIN reg_employees reg_employees
	ON reg_employees.id_employee = reg_allocations.id_driver_fk
	AND reg_employees.id_tenant = reg_allocations.id_tenant
WHERE reg_schedules.id_pump_fk IS NOT NULL
AND reg_schedules.created_at >= '2025-06-01 00:00:00.000' AND reg_schedules.created_at <= '2025-06-30 00:00:00.000'  
AND ('8d4b8d26-c6dd-4270-baff-840d11fc1c52' = '' OR reg_schedules.id_tenant = CAST('8d4b8d26-c6dd-4270-baff-840d11fc1c52' AS UUID)) 
AND ('' = '' OR reg_concrete_batching_plants.id_concrete_batching_plant = ANY(string_to_array('', ',')::UUID[])) 
AND ('' = '' OR reg_clients.id_client = ANY(string_to_array('', ',')::UUID[]))  
AND ('' = '' OR reg_schedules.id_schedule = ANY(string_to_array('', ',')::UUID[]))  
AND ('' = '' OR reg_contracts.id_contract = ANY(string_to_array('', ',')::UUID[]))
ORDER BY reg_schedules.code;
```

# Dúvidas
- Devo adicionar informação do número do contrato no relatório de programação?

# Correções

- Ajustar espaçamento dos nomes  
- Ajustar formato Topcon: cabeçalho e rodape  
- Adicionar dados do contrato  
- Adicionar observação externa da programação  
- Remover referencia de acesso  
- Ver possibilidade de adicionar contato

# Casos de teste

- [ ] 
- [ ] 
- [ ] 



