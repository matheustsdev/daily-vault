---
Ticket: 476522
Status: Concluído
Data de início: 2025-01-08
Data de fim: 2025-01-13
---
- Forçar integração da nota 2463 de Blumenau
- Estudar se o session id nas mensagens do service bus é suficiente para garantir o processamento em série
- Foi decidido que vai ser implementado o campo OriginSequenceNumber para ordenar as mensagens no integrator por esse campo, sendo ele o Id original da primeira mensagem lida pela FUNCTION