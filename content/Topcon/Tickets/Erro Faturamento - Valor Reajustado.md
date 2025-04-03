---
Ticket: 482005
Status: Concluído
Data de início: 2025-02-07
Data de fim: 2025-02-10
---
# Análise
## 1. Agrupamento da leitura de eventos da integração

Vou validar com Luiz o processo inteiro, para verificar possíveis falhas.

Vimos que, no processo:
1. Atualizar valor do produto
2. Atualizar valor do produto novamente (sem aprovar a alteração anterior)
3. Aprovar versão

É gerado evento para as 2 atualizações porém nada foi integrado no DISPATCH. Além disso os eventos são de delete e insert, os dados não são atualizados e sim removidos e reinseridos

O erro se deu devido à uma incongruência no script SQL que a integração utiliza para ler os eventos na ger_db_events, que agrupava os eventos de uma maneira que perdia os eventos de update/insert quando havia um evento de delete. Como a atualização do valor da con_proposta_item é feita removendo e e reinserindo o dado, o evento se perdia.
# Solução
## 1. Agrupamento da leitura de eventos da integração

Foi corrigido script utilizado para ler os eventos, substituindo update/insert por upsert, já que não faz diferença para a integração e adicionando agrupamento por evento também, já considerando esse upsert