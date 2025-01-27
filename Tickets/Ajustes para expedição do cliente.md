---
Ticket: 480284
Status: Em análise
Data de início: 2025-01-27
Data de fim:
---

# Análise
## 1. Falta dados na documentação de API

## 2. Ajuste na validação para criar remessa/nota

A validação para a emissão da remessa verifica a existência de subsidiária mesmo que esteja configurado para não emitir nota

Adicionar na doc do ItemPumping o enum:
BillingCalculationType
 - MINIMUM_FEE_PLUS_SURPLUS,
 - NO_CHARGE
# Solução

## 1. Foi feito o ajuste na documentação
## 2. Foi adicionado na validação a verificação de necessidade de nota
