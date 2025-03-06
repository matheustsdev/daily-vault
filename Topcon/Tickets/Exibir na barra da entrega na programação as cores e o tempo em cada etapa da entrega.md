---
Ticket: 462262
Status: Concluído
Data de início: 2025-01-23
Data de fim: 2025-02-06
---
# Análises

## 1. Status a caminho como "final" das viagens intermediárias de argamassa

Caio viu que as viagens intermediárias de argamassa estão vindo: a caminho -> descarregando -> a caminho. Porém como está a tratativa de status, o DISPATCH forçava o status para descarregando.

![[Pasted image 20250124100507.png]]

## 2. Tratativa da argamassa nos processor não considera a campo novo e nem as peculiaridades da mesma

Necessário fazer algumas tratativas para lidar com os estados em ordem que não funcionam para concreto e o campo novo

## 3. Envio dos dados pelo FLEET

Algumas atualizações do FLEET não enviavam o id da viagem ou do estado, o que fazia a mensagem não chegar até o DISPATCH
## 4. Retornou com um problema que ocorreu devido o FLEET não gerar as viagens
# Soluções

## 1. Adição do campo LeaveConstruction
Vai ser adicionado o campo LeaveConstruction na DeliveryTicketConcrete para registrar o momento em que o veículo sai da obra, dessa forma podemos utilizar o ultimo "a caminho" das viagens intermediárias de argamassa para atualizar o status para "Entregue". 

Além disso esse campo será utilizado para calcular a taxa de permanência para argamassa.

## 2. Fazer a tratativa no Travel e no VehicleState processor

## 3. Adicionar ID em alguns update/insert do FLEET

## 4. Enviar todos os estados da viagem ao atualizar um estado

Para garantir a validação de todos os estados, o estado que vem na raiz do objeto VehicleState é injetado na lista de estados dentro da Travel. Isso se faz necessário pois o evento é enviado antes da atualização do estado que o gerou.

---
# Processo de teste

Para resetar viagem no FLEET e tentar utilizar o simulador novamente:
1. Resetar tudo
2. Executar um comando com o simulador pra deixar "Na usina"
3. Escolher uma travel pra started
4. Adicionar essa travel como current do veículo

## FLEET
```sql
----- Get vehicle data -----

SELECT rv.id_vehicle, rv.license_plate, rt.num_doc, tvst.description AS vehicle_state, rl."name" AS location FROM reg_vehicles rv
LEFT JOIN reg_travels rt ON rt.id_travel = rv.id_current_travel_fk
LEFT JOIN reg_vehicles_states rvs ON rvs.id_vehicle_state = rv.id_current_state_fk
LEFT JOIN tpe_vehicles_states_types tvst ON tvst.id_vehicle_state_type = rvs.id_vehicle_state_type_fk
LEFT JOIN reg_locations rl ON rl.id_location = rv.id_current_location_fk
WHERE rv.external_id = '141107d6-bdc1-4552-bb3d-8f35033acf6d';

----- Get travels data -----  
SELECT * FROM reg_travels rt
INNER JOIN reg_locations rl ON rl.id_location = rt.id_destination_location_fk
WHERE rt.description = 'HOJE'
ORDER BY rt.num_doc;


----- Delete all vehicle states linked -----

DELETE FROM reg_vehicles_states rvs WHERE rvs.id_travel_fk IN (
'a4ccc0d1-1b85-4f62-9619-3919717e70f2',
'00838911-6140-4565-8d46-736a93d3fcdc',
'6b2bf255-6abe-40bc-bd42-fca47d266944'

);


----- Clear vehicle -----

UPDATE reg_vehicles SET
id_current_state_fk = NULL,
id_current_location_fk = NULL,
id_current_travel_fk = NULL
WHERE external_id = '141107d6-bdc1-4552-bb3d-8f35033acf6d';


----- Clear travels -----

UPDATE reg_travels SET
duration = NULL,
started = FALSE,
already_discharged = FALSE,
average_speed = NULL,
finish_hour_meter = 0,
finish_odometer = 0,
start_hour_meter = 0,
start_odometer = 0,
finish_date = NULL,
id_end_location_fk = NULL,
document_date = timezone('utc'::text, now()),
last_due_unload_date = null
WHERE description = 'HOJE';
```

## DISPATCH

```sql
----- Get delivery ticket data -----

SELECT
rdtc.id_delivery_ticket_concrete,
rdt.id_delivery_ticket,
rdt.code,
rc2."name",
rdtc.status,
rdtc.invoice_issued,
rdtc.at_the_plant_start,
rdtc.on_the_way,
rdtc.at_construction,
rdtc.start_unloading,
rdtc.finish_unloading,
rdtc.leave_construction,
rdtc."returning",
rdtc.at_the_plant
FROM reg_delivery_tickets rdt
INNER JOIN reg_delivery_ticket_concretes rdtc ON rdtc.id_delivery_ticket_fk = rdt.id_delivery_ticket
INNER JOIN reg_schedules rs ON rs.id_schedule = rdt.id_scheduling_fk
INNER JOIN reg_contracts rc ON rc.id_contract = rs.id_contract_fk
INNER JOIN reg_constructions rc2 ON rc2.id_construction = rc.id_construction_fk
WHERE rdt.code IN (441,442,443)
ORDER BY rdt.code;


----- Clear delivery ticket concretes -----

UPDATE reg_delivery_ticket_concretes SET
on_the_way = NULL,
at_construction = NULL,
start_unloading = NULL,
finish_unloading = NULL,
"returning" = NULL,
at_the_plant = NULL,
distance_traveled = NULL,
finish_odometer = NULL,
initial_odometer = NULL,
initial_hour_meter = NULL,
finish_hour_meter = NULL,
at_the_plant_start = NULL,
leave_construction = NULL,
status = 'INVOICE_ISSUED'
WHERE id_delivery_ticket_concrete IN (
'11409db7-52d0-4570-a4e1-bca4921151ce',
'2f611919-a925-4b78-9e22-c6c2794032c0',
'dbe7400f-9084-49ec-8eb0-3910309b31dc'
);
```