---
Ticket: 499777
Status: Concluído
Data de início: 2025-05-05
Data de fim: 2025-05-06
---
# Problema 1

Ao adicionar bomba, as datas das programações recorrentes se mantem pro dia atual.

## Solução 1

Ajuste na tratativa das programações recorrente para adicionar as datas de bombas corretamente em cada recorrência.

# Casos de teste

## ✅ Caso 1: Criar programação recorrente com bomba

- **Pré-condições:**
    - Contrato possuir serviço de bombeamento
- **Passos do Teste:**
    1. Adicionar recorrência na programação de 3 programações
    2. Adicionar bombeamento (horário)
    3. Finalizar criação da programação
- **Resultado Esperado:** 3 programações criadas com bombeamento para diferentes dias seguindo a regra adicionada na recorrência sendo exibidas corretamente no Gantt
- **Status:** ✅ PASSOU 
- **Observações:** 

## ✅ Caso 2: Criar programação recorrente sem bomba

- **Pré-condições:**
    - Contrato possuir serviço de bombeamento
- **Passos do Teste:**
    1. Adicionar recorrência na programação de 3 programações
    2. Finalizar criação da programação
- **Resultado Esperado:** 3 programações sem bombeamento criadas para diferentes dias seguindo a regra adicionada na recorrência sendo exibidas corretamente no Gantt
- **Status:** ✅ PASSOU
- **Observações:** 

## ✅ Caso 3: Criar programação sem recorrência sem bomba

- **Pré-condições:**
- **Passos do Teste:**
    1. Adicionar bombeamento (horário)
    2. Finalizar criação da programação
- **Resultado Esperado:** 1 programação sem bombeamento criada sendo exibidas corretamente no Gantt
- **Status:** ✅ PASSOU
- **Observações:** 

## ✅ Caso 4: Criar programação sem recorrência com bomba

- **Pré-condições:**
    - Contrato possuir serviço de bombeamento
- **Passos do Teste:**
    1. Adicionar recorrência na programação de 3 programações
    2. Adicionar bombeamento (horário)
    3. Selecionar uma central auxiliar
    4. Finalizar criação da programação
- **Resultado Esperado:** 3 programações criadas com bombeamento para diferentes dias seguindo a regra adicionada na recorrência sendo exibidas corretamente no Gantt.
- **Status:** ✅ PASSOU
- **Observações:** 

## ✅ Caso 5: Criar programação com recorrência com bomba e com central auxiliar

- **Pré-condições:**
    - Contrato possuir serviço de bombeamento
    - Possuir mais de uma central
- **Passos do Teste:**
    1. Adicionar recorrência na programação de 3 programações
    2. Adicionar bombeamento (horário)
    3. Adicionar uma central auxiliar
    4. Finalizar criação da programação
- **Resultado Esperado:** 6 programações criadas com bombeamento para diferentes dias seguindo a regra adicionada na recorrência sendo exibidas corretamente no Gantt, sendo 3 para uma central e 3 para outra
- **Status:** ✅ PASSOU
- **Observações:** 

## ✅ Caso 4: Criar programação sem recorrência sem bomba e com central auxiliar

- **Pré-condições:**
    - Contrato possuir serviço de bombeamento
    - Possuir mais de uma central
- **Passos do Teste:**
    1. Adicionar uma central auxiliar
    2. Finalizar criação da programação
- **Resultado Esperado:** 2 programações para centrais corretas
- **Status:** ✅ PASSOU
- **Observações:** 


Emitiram a nota e depois fizeram redosagem.

