---
Ticket: 538559
Status: Concluído
Data de início: 2025-11-10
Data de fim: 2025-11-13
---
# Problemas
## Problema 1
Valores zerados na integração externa do cliente
### Causa
Quando há um reaproveitamento ou redirecionamento total da carga, o valor teórico na *DeliveryTicketSupply* fica zerado, ocasionando erros na integração dos clientes que não utilizam legado.

### Solução 1
Criar novo campo: *TheoricalBalanceQuantity*, para armazenar o valor tratado devido os reaproveitamentos e manter o valor teórico preenchido. 

Dessa forma, é necessário atualizar a *FUNCTION* e o *INTEGRATOR* para utilizarem o novo campo.
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

