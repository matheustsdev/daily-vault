---
Ticket: 462262
Status: Em análise
Data de início: 2025-01-23
Data de fim:
---
# Análise

## 1. Status a caminho como "final" das viagens intermediárias de argamassa

Caio viu que as viagens intermediárias de argamassa estão vindo: a caminho -> descarregando -> a caminho. Porém como está a tratativa de status, o DISPATCH forçava o status para descarregando.

![[Pasted image 20250124100507.png]]

# Solução

## Adição do campo LeaveConstruction
Vai ser adicionado o campo LeaveConstruction na DeliveryTicketConcrete para registrar o momento em que o veículo sai da obra, dessa forma podemos utilizar o ultimo "a caminho" das viagens intermediárias de argamassa para atualizar o status para "Entregue". 

Além disso esse campo será utilizado para calcular a taxa de permanência para argamassa.