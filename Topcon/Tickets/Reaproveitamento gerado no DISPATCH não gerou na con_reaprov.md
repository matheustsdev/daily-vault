---
Ticket: 487003
Status: Concluído
Data de início: 2025-03-13
Data de fim: 2025-03-17
---

# Análises
1. Não existe nenhum registro de tentativa de integração no log da integração. Ao tentar forçar, o reuso integrou normalmente pois não possui fluxo de validação
2. Não era um problema pois a validação que definia se o reaproveitamento deveria ou não ser integrado só permitia integração de reaproveitamentos que houvesse emissão de nota e, nesse caso, não havia sido emitido nota


# Soluções
1. O ocorrido, até então, não era um problema pois a validação que definia se o reaproveitamento deveria ou não ser integrado só permitia integração de reaproveitamentos que houvesse emissão de nota e, nesse caso, não havia sido emitido nota. Porém, após conversa com Luiz, foi definido que a regra deveria ser alterada, integrando mesmo sem nota fiscal emitida e atualizando o dado após a emissão. Além disso também foi adicionado a integração do descarte na tabela con_reaprov

