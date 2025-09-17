---
Ticket: 521672
Status: Em análise
Data de início: 2025-09-09
Data de fim:
---
# Problemas
## Problema 1
Não está sendo exibido uma taxa de adicional noturno para o contrato 999-3671-25, nem para seleção no Frontend ao emitir remessa.
### Causa
O problema se deu porque na integração não era enviado a propriedade Active do adicional, fazendo com que sempre fosse setado para falso ao integrar, já que falso era o valor default
### Solução 1
Foi feito ajuste na integração adicionando a propriedade.

### Solução 2
Foi adicionado um endpoint para forçar o recálculo dos adicionais do período.

## Problema 2
O cálculo para validar a necessidade da cobrança de adicional noturna não está sendo calculado corretamente.
### Causa
Não tratava corretamente o horário de fim já que muitas vezes este era menor que o inicial numericamente.
### Solução 1
Alteração da validação numérica apenas para criação de datas em si para comparar. Além disso foi adicionado um condicional para adicionar um dia caso o horário de fim seja menor que horário de início (ex: de 17 à 2).
# Casos de teste

## ✅❌ Caso 1: nome do teste

- **Pré-condições:**
    - 
    - 
    - 
- **Passos do Teste:**
    1. 
    2. 
    3. 
- **Resultado Esperado:** 
- **Status:** ✅ PASSOU | ❌ FALHOU
- **Observações:** (A ser preenchido se necessário)

## ✅❌ Caso 2: nome do teste

- **Pré-condições:**
    - 
    - 
    - 
- **Passos do Teste:**
    1. 
    2. 
    3. 
- **Resultado Esperado:** 
- **Status:** ✅ PASSOU | ❌ FALHOU
- **Observações:** (A ser preenchido se necessário)

