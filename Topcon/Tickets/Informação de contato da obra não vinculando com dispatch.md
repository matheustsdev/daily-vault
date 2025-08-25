---
Ticket: 516051
Status: Em análise
Data de início: 2025-08-22
Data de fim:
---
# Problemas
# Problema 1
O contrato não possui contato vinculado. Checando na integração a única forma de integrar contato é via *con_contrato*, portanto caso exista uma alteração na *con_obras* que não force a criação de uma nova versão de contrato, essa obra não vai ser integrada e o contato também não já que é um campo da obra.
## Solução 1
Adicionar uma trigger de update na *con_obras* que gere um evento de contrato.

# Problema 2
Ao chamar o endpoint de update não estava ocorrendo a atualização do *ContactInfo* devido a remoção dos filhos da entidade *Contact* antes de envia-la para o *Manipulate*.
## Solução 1
Tratar a limpeza dos objetos para que os *Manipulates* possuam os campos necessários.

# Casos de teste

## ✅ Caso 1: Geração do evento

- **Pré-condições:**
    - Possuir ambiente do legado
    - 
    - 
- **Passos do Teste:**
    1. Adicionar a trigger no banco do legado
    2. Forçar um update na *con_obras*
- **Resultado Esperado:** A existência de um evento de contrato daquele vinculado à obra
- **Status:** ✅ PASSOU | ❌ FALHOU
- **Observações:** 

## ✅ Caso 2: Chamada à API

- **Pré-condições:**
    - Possuir uma mensagem/corpo de Contrato com os campos inclusos
    - Pode também ser feito o GET como na *FUNCTION* para obter o mesmo objeto que ela obteria e alterar apenas o necessário
    - 
- **Passos do Teste:**
    1. Requisitar via Insomnia/Postman o endpoint para atualizar o Contrato (PUT)
- **Resultado Esperado:** Verificar se o contrato e seus filhos foram atualizados corretamente
- **Status:** ✅ PASSOU
- **Observações:** 

