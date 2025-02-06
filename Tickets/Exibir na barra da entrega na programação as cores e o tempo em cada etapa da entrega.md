---
Ticket: 462262
Status: Concluído
Data de início: 2025-01-23
Data de fim: 2025-02-06
---
# Análise

## 1. Status a caminho como "final" das viagens intermediárias de argamassa

Caio viu que as viagens intermediárias de argamassa estão vindo: a caminho -> descarregando -> a caminho. Porém como está a tratativa de status, o DISPATCH forçava o status para descarregando.

![[Pasted image 20250124100507.png]]

## 2. Tratativa da argamassa nos processor não considera a campo novo e nem as peculiaridades da mesma

Necessário fazer algumas tratativas para lidar com os estados em ordem que não funcionam para concreto e o campo novo
# Solução

## 1. Adição do campo LeaveConstruction
Vai ser adicionado o campo LeaveConstruction na DeliveryTicketConcrete para registrar o momento em que o veículo sai da obra, dessa forma podemos utilizar o ultimo "a caminho" das viagens intermediárias de argamassa para atualizar o status para "Entregue". 

Além disso esse campo será utilizado para calcular a taxa de permanência para argamassa.

## 2. Fazer a tratativa no Travel e no VehicleState processor