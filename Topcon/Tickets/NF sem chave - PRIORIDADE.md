---
Ticket: 503767
Status: Em análise
Data de início: 2025-05-22
Data de fim: 2025-05-27
---
# Problemas
# Problema 1

Uma autorização da nota (DeliveryTaxDocumentAuthorization) foi integrada antes da remessa (DeliveryTicket), resultando no sucesso ao adicionar dados na topnfe e falha ao tentar atualizar os dados da fis_nf já que não existia o registro.

## Solução 1

Implementar um fluxo de, caso integração fora da ordem, forçar integração da remessa referente à essa autorização.
# Casos de teste

## ✅ Caso 1: Teste de fluxo

- **Pré-condições:**
    - Autorização de nota no DISPATCH
    - Prévia integração no legado
- **Passos do Teste:**
    1. Remover dado da nota nas tabelas fis_nf e con_nf
    2. Forçar integração da delivery tax document authorization via API
- **Resultado Esperado:** Deve ser adicionado ambos os dados novamente com a data de inserção referente ao dia atual
- **Status:** ✅ PASSOU
- **Observações:** Foi necessário apenas alterar a fila em que havia alocado inicialmente (legacy) para a de reprocessamento (reprocess) para manter a semântica