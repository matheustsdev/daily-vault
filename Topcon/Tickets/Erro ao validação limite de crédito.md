---
Ticket: 481599
Status: Concluído
Data de início: 2025-02-14
Data de fim: 2025-02-14
---
Ticket retornou
# Análise
1. A API de atualização do Client não forçava a validação de análise de crédito
2. Os item pumpings não possuem valores, não conseguindo calcular o total usado para validar a programação
3. Exibição do nome da programação/contrato dos dados da programação
# Solução
1. Adicionei a validação ao atualizar via API
2. Adicionar nullish para 0 no item pumping
3. Validar quantidade de items ao splitar contrato
