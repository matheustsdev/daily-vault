---
Ticket: 532027
Status: Concluído
Data de início: 2025-10-07
Data de fim: 2025-10-07
---
# Problemas
## Problema 1
Não está integrando algumas filiais para a subsidiária no DISPATCH
### Causa
Configuração errada do tenant no appsettings do INTEGRATOR
### Solução 1
Ajuste do tenant
# Casos de teste

## ✅❌ Caso 1: Forçar integração das filiais

- **Pré-condições:**
    - Ter integrator rodando
- **Passos do Teste:**
    1. Abrir banco do legado
    2. Forçar update do item da fis_filial não adicionada
- **Resultado Esperado:** Integração da subsidiária para o DISPATCH
- **Status:** ✅ PASSOU
- **Observações:**