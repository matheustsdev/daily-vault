---
Ticket: 501561
Status: Concluído
Data de início: 2025-05-15
Data de fim: 2025-05-16
---
# Problemas
# ✅ Problema 1: Include faltando

Ao editar uma entrega, muitos campos utilizados no Gantt não estão no include da query da programação atualizada, resultando em quebra da página.
## Solução 1

Adicionar uma variável que irá possuir todos os campos necessários para a query da programação no Gantt e utilizar nos pontos necessários
# Casos de teste

## ✅ Caso 1: Expedir entregas

- **Pré-condições:**
    - Criar uma programação
- **Passos do Teste:**
    1. Criar programação
    2. Clicar no botão de expedição
    3. Finalizar o processo de expedição
- **Resultado Esperado:** Sucesso no processo de expedição
- **Status:** ✅ PASSOU
- **Observações:**

## ✅ Caso 2: Cancelar entrega

- **Pré-condições:**
    - Possuir uma entrega
- **Passos do Teste:**
    1. Clicar no botão de cancelar na programação
- **Resultado Esperado:** Cancelamento da entrega
- **Status:** ✅ PASSOU
- **Observações:**

